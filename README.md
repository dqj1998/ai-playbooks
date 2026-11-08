# AI Playbooks

> Practical, reusable plans, prompts, and methods for real-world AI-assisted engineering.

A living collection of playbooks I actually use: end-to-end task plans, reusable
prompts, orchestration patterns, and hard-won lessons. Everything here is
sanitized, reproducible, and meant to be copied, adapted, and improved.

[中文简介](#中文简介)

---

## What this is

Most AI content is either too abstract ("prompt better!") or too narrow (a single
tweet-sized trick). This repo sits in between: **complete, executable playbooks**
for real tasks — from cleaning up a full disk to orchestrating multi-agent coding
workflows.

Each playbook is a self-contained document you can drop into your own workflow.

## What's inside

```
.
├── plans/        # End-to-end task plans (goal → steps → verification)
├── prompts/      # Reusable, parameterized prompts
├── methods/      # Workflows, orchestration patterns, decision frameworks
├── templates/    # Copy-paste starting points
└── notes/        # Lessons learned, post-mortems, gotchas
```

## How to use

1. Browse the folder that matches your need.
2. Read the playbook's **Goal**, **Prerequisites**, and **Steps**.
3. Copy, adapt to your environment, and run.
4. If you improve it, open a PR.

Playbooks are written to be **tool-agnostic** where possible, with tool-specific
notes called out explicitly.

## Principles

- **Sanitized** — no secrets, credentials, private paths, or personal data.
- **Reproducible** — every step is verifiable; no "it just works" hand-waving.
- **Reversible** — destructive actions come with dry-run and rollback guidance.
- **Honest** — failures and dead ends are documented, not hidden.
- **Versioned** — playbooks evolve; history is kept.

## Example

- [`plans/disk-cleanup-macos.md`](plans/disk-cleanup-macos.md) — reclaim disk
  space on macOS without breaking active work, with a tiered risk model
  (zero-risk caches → rebuildable artifacts → archive → app-level).

## Contributing

Issues and PRs are welcome. Please:

- Keep playbooks self-contained and reproducible.
- Remove any sensitive or machine-specific data before submitting.
- Follow the existing structure: Goal / Prerequisites / Steps / Verification / Risks.

## License

- **Content** (plans, prompts, methods, docs) is licensed under
  [CC BY 4.0](LICENSE).
- **Code snippets** are licensed under [MIT](LICENSE-MIT).

You may reuse, adapt, and redistribute with attribution. See the license files
for details.

## Disclaimer

These playbooks are provided as-is. Destructive commands are clearly marked —
read them before running. You are responsible for your own system and data.

---

## 中文简介

本仓库收录我在真实工程中使用的 AI 使用方案：端到端的任务计划、可复用的提示词、
多智能体编排方法，以及踩坑记录。所有内容均已脱敏、可复现，欢迎直接取用、改造与回馈。

目录结构：`plans/`（任务计划）、`prompts/`（提示词）、`methods/`（方法与编排）、
`templates/`（模板）、`notes/`（经验与复盘）。

**许可**：文字内容采用 [CC BY 4.0](LICENSE)，代码片段采用 [MIT](LICENSE-MIT)。
