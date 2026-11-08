# Disk Cleanup on macOS (Without Breaking Active Work)

> Reclaim disk space on a nearly-full macOS volume using a tiered risk model:
> zero-risk caches → rebuildable artifacts → archive → app-level.

## Goal

Free up significant disk space on a macOS machine that is >95% full, **without
deleting or disrupting work that is currently active**.

## Prerequisites

- macOS with APFS.
- Read-only investigation first: `df`, `du`, `ls`, `find`, `tmutil`.
- An external drive for archiving (optional but recommended).
- `sudo` access for system-level caches.

## Method: tiered risk model

Classify every candidate by **risk to active work**, then execute from lowest to
highest risk. Never start with the biggest folder — start with the safest.

### Step 0 — Baseline and identify active work

```bash
df -h /System/Volumes/Data
# Find recently modified project dirs (last 14 days) — these are "active"
find ~/Projects -maxdepth 2 -type d -mtime -14
```

Record the baseline. Anything modified recently is **protected**.

### Step 1 — Locate the big consumers

```bash
du -sh ~/* 2>/dev/null | sort -rh | head -20
du -sh ~/Library/* 2>/dev/null | sort -rh | head -20
du -sh /Library/Developer/* 2>/dev/null | sort -rh | head
```

### Tier A — Zero-risk caches (auto-regenerated)

| Target | Typical size | Command |
|---|---|---|
| Xcode DerivedData | 1–5 G | `rm -rf ~/Library/Developer/Xcode/DerivedData/*` |
| Package caches (pip/npm/gradle/go) | 5–20 G | `rm -rf ~/Library/Caches/{pip,go-build} ~/.npm/_cacache ~/.gradle/caches` |
| Tool caches (uv/puppeteer/playwright) | 2–10 G | `rm -rf ~/.cache/{uv,puppeteer} ~/Library/Caches/ms-playwright` |
| Old iOS DeviceSupport | 5–20 G | keep only the current iOS version |
| Old simulator runtimes | 10–60 G | `xcrun simctl runtime delete <UDID>` |
| Homebrew old versions | <1 G | `brew cleanup -s` |

> **SIP note:** `/Library/Developer/CoreSimulator/Caches/dyld` is SIP-protected.
> `rm` fails with `Operation not permitted` even as root. Leave it — it is
> regenerated and mostly belongs to runtimes you keep.

### Tier B — Rebuildable build artifacts

Safe to delete, but the next build is slower. **Never delete source or manifests.**

| Target | Rebuild with |
|---|---|
| Rust `target/` | `cargo build` |
| `node_modules/` | `npm install` / `pnpm install` |
| `.next/`, `dist/`, `build/` | project build command |

```bash
# Example: delete cold Rust target dirs (keep active ones)
rm -rf path/to/cold-project/target
```

### Tier C — Archive to external drive

Move cold projects and old archives off the main disk. Verify before deleting
the source:

```bash
rsync -a --dry-run src/ /Volumes/EXTERNAL/Archive/
rsync -a --remove-source-files src/ /Volumes/EXTERNAL/Archive/
```

### Tier D — App-level (needs human judgment)

- Unused SDKs (Android SDK, Anaconda, etc.)
- Docker: `docker system prune -a`
- Unused applications
- Editor caches (VS Code `workspaceStorage`, `CachedExtensionVSIXs`)

## Verification

```bash
df -h /System/Volumes/Data
# Confirm protected dirs still exist
ls -d ~/Projects/active-project ~/Projects/active-project/node_modules
```

## Risks

- **Simulator runtimes / DeviceSupport**: deleting forces a re-download (GBs).
  Keep the version you actively debug on.
- **Build artifacts**: only `target/` and `node_modules/` — never source or
  `Cargo.toml` / `package.json`.
- **Cross-volume moves**: always `rsync --dry-run` first.
- **SIP-protected paths**: don't fight them; they regenerate.

## Result (reference run)

A real run on a 460 GiB volume went from **12 GiB → 154 GiB free** (+142 GiB),
with all active projects intact. The largest single win was deleting unused
simulator runtimes (~62 GiB), followed by caches (~50 GiB) and cold Rust
`target/` dirs (~33 GiB).
