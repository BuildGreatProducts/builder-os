# BuilderOS

> **Formerly PLAID.** BuilderOS is the successor to [PLAID — Product Led AI Development](https://github.com/BuildGreatProducts/plaid). The PLAID skills have been refactored into standalone BuilderOS skills; new development happens here.

**An operating system for builders.** BuilderOS is a collection of agent skills that give product builders a repeatable system for ideating, designing, and building software products with AI — turning coding agents from autocomplete into disciplined collaborators that ship reviewed, tested work.

## Why

AI coding agents are fast, but speed without discipline produces features that "compile" rather than features that work. BuilderOS encodes the habits of strong product teams — plan-driven work, mandatory review, end-to-end testing, honest reporting — as skills your agent follows automatically.

## How it works

Each skill is a `SKILL.md` file that teaches an AI coding agent a workflow. Install the skills into your project (see [Installation](#installation)), then trigger them with natural prompts. Every skill is **fully standalone** — but they're designed to chain: each one writes its output as a document in your project's `docs/` folder, and downstream skills pick those documents up automatically.

```
Ideate                Plan & Design              Build                  Launch
─────────             ─────────────              ─────────              ─────────
Idea Generator   →    Product Planner       →    Build Loop        →    Launch Checklist
Idea Validator        Design System
   │                     │                          │                      │
   ▼                     ▼                          ▼                      ▼
docs/                 docs/                      working,               docs/
product-idea.md       product-vision.md          reviewed,              launch-checklist.md
validation-report.md  prd.md                     tested
                      product-roadmap.md         product
                      design.md
```

## Skills

### Ideate

| Skill | What it does | Output |
|---|---|---|
| [Idea Generator](skills/idea-generator/SKILL.md) | Guided discovery of a product idea by mining what you already know or do — business or expertise. Captures context, synthesizes 3–5 candidate directions, scores them on a five-axis scorecard, and sharpens the winner. | `docs/product-idea.md` |
| [Idea Validator](skills/idea-validator/SKILL.md) | Pressure-tests an idea before you invest in building. Finds the core assumption, ranks fatal flaws, maps real competition (including "doing nothing"), plans your first 10 customers, defines a 2-week MVP test, and delivers a blunt strong / weak / pivot verdict. | `docs/validation-report.md` |

### Plan & Design

| Skill | What it does | Output |
|---|---|---|
| [Product Planner](skills/product-planner/SKILL.md) | A structured vision-intake conversation (8 sections, AI-suggested answers throughout) followed by generation of your three core product documents: strategy & brand, a coding-agent-ready technical spec, and a phased build plan with task checkboxes. Picks up `docs/product-idea.md` automatically if it exists. | `docs/VISION.md`, `docs/product-vision.md`, `docs/prd.md`, `docs/product-roadmap.md` |
| [Design System](skills/design-system/SKILL.md) | Translates images (screenshots, mockups, Figma URLs) into a design system in [Google's open design.md format](https://github.com/google-labs-code/design.md) — YAML design tokens plus prose rationale any coding agent can implement from. | `docs/design.md` |

### Build

| Skill | What it does | Output |
|---|---|---|
| [Build MVP](skills/build-mvp/SKILL.md) | Executes the entire roadmap end to end: works through every task in `docs/product-roadmap.md` in order — implementing, testing, and verifying each before moving on — marking checkboxes, updating the status line, and opening a PR per phase, until the magic moment works. Agent-agnostic. | Working code, a PR per phase, completed roadmap |

For tighter, review-gated increments, use a **Build Loop** instead: nothing ships on "it compiles" — every increment is **built → reviewed → tested end to end → fixed** before the agent reports "done." Works from the Product Planner's roadmap or a direct prompt. Available in three flavors, adapted to each agent's native review tooling:

| Skill | Agent | Review mechanism |
|---|---|---|
| [Build Loop – Claude Code](skills/build-loop-claude-code/SKILL.md) | Claude Code | `/review`, plus `/security-review` for sensitive surfaces |
| [Build Loop – Codex](skills/build-loop-codex/SKILL.md) | OpenAI Codex CLI | `/review` on uncommitted changes, plus a custom security pass |
| [Build Loop – Cursor](skills/build-loop-cursor/SKILL.md) | Cursor | Cursor's `/review` |

**Trigger with:** "run the build loop", "build the next task", "continue the plan", or "build this feature properly".

### Launch

| Skill | What it does | Output |
|---|---|---|
| [Launch Checklist](skills/launch-checklist/SKILL.md) | Audits your actual codebase — stack, services, env vars, payments, deploy config — then writes a plain-English, step-by-step path from "works on my machine" to "customers can use it." Every step is marked 🧑 you / 🤖 agent / 🤝 together, with time estimates, costs, and a "you'll know it worked when..." check. | `docs/launch-checklist.md` |

## The docs/ folder

All skills read and write a shared `docs/` folder at your project root. Run them in any order — each skill works alone, but when an upstream document exists, the next skill uses it to pre-fill its questions:

| Document | Written by | Consumed by |
|---|---|---|
| `product-idea.md` | Idea Generator | Idea Validator, Product Planner |
| `validation-report.md` | Idea Validator | you |
| `VISION.md` | Product Planner (intake) | Product Planner (generation), Design System |
| `product-vision.md` | Product Planner | Design System, Build MVP, Build Loop |
| `prd.md` | Product Planner | Build MVP, Build Loop |
| `product-roadmap.md` | Product Planner | Build MVP, Build Loop (progress tracked via checkboxes) |
| `design.md` | Design System | Product Planner, Build MVP, Build Loop |
| `launch-checklist.md` | Launch Checklist | you |

## Installation

BuilderOS uses the [skills CLI](https://github.com/vercel-labs/skills). Run these from your project's root directory.

### Everything in one go

```sh
npx skills add BuildGreatProducts/builder-os
```

Installs every BuilderOS skill at once. Use this if you want the full ideate → plan → design → build → launch pipeline.

### Individual skills

```sh
npx skills add BuildGreatProducts/builder-os/skills/idea-generator
npx skills add BuildGreatProducts/builder-os/skills/idea-validator
npx skills add BuildGreatProducts/builder-os/skills/product-planner
npx skills add BuildGreatProducts/builder-os/skills/design-system
npx skills add BuildGreatProducts/builder-os/skills/build-mvp
npx skills add BuildGreatProducts/builder-os/skills/build-loop-claude-code
npx skills add BuildGreatProducts/builder-os/skills/build-loop-codex
npx skills add BuildGreatProducts/builder-os/skills/build-loop-cursor
npx skills add BuildGreatProducts/builder-os/skills/launch-checklist
```

Every skill is fully self-contained — install just the Design System to turn a screenshot into design tokens, or just a Build Loop to execute a roadmap someone else wrote. You only need the Build Loop for the coding agent you actually use.

### By skill name

```sh
npx skills add BuildGreatProducts/builder-os --skill product-planner
```

### Manual installation

Clone this repo and copy the skill folders you want into your project's skills directory (e.g. `.claude/skills/` for Claude Code).

## Getting started

1. Install the skills (see above).
2. Start anywhere in the lifecycle:
   - Have nothing? → "help me find an idea"
   - Have an idea? → "validate my idea"
   - Idea validated? → "plan my product"
   - Have a reference design? → "create a design system from this image"
   - Docs ready? → "build my MVP" (whole roadmap) or "run the build loop" (task by task)
   - Product built? → "create my launch checklist"

## Changelog

See [CHANGELOG.md](CHANGELOG.md).
