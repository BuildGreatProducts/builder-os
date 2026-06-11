# BuilderOS

**An operating system for builders.** BuilderOS is a collection of agent skills that give product builders a repeatable system for ideating, designing, and building software products with AI — turning coding agents from autocomplete into disciplined collaborators that ship reviewed, tested work.

## Why

AI coding agents are fast, but speed without discipline produces features that "compile" rather than features that work. BuilderOS encodes the habits of strong product teams — plan-driven work, mandatory review, end-to-end testing, honest reporting — as skills your agent follows automatically.

## How it works

Each skill is a `SKILL.md` file that teaches an AI coding agent a workflow. Install the skills into your project, then trigger them with natural prompts. Every skill is **fully standalone** — but they're designed to chain: each one writes its output as a document in your project's `docs/` folder, and downstream skills pick those documents up automatically.

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
| [Idea Generator](skills/Idea%20Generator/SKILL.md) | Guided discovery of a product idea by mining what you already know or do — business or expertise. Captures context, synthesizes 3–5 candidate directions, scores them on a five-axis scorecard, and sharpens the winner. | `docs/product-idea.md` |
| [Idea Validator](skills/Idea%20Validator/SKILL.md) | Pressure-tests an idea before you invest in building. Finds the core assumption, ranks fatal flaws, maps real competition (including "doing nothing"), plans your first 10 customers, defines a 2-week MVP test, and delivers a blunt strong / weak / pivot verdict. | `docs/validation-report.md` |

### Plan & Design

| Skill | What it does | Output |
|---|---|---|
| [Product Planner](skills/Product%20Planner/SKILL.md) | A structured vision-intake conversation (8 sections, AI-suggested answers throughout) followed by generation of your three core product documents: strategy & brand, a coding-agent-ready technical spec, and a phased build plan with task checkboxes. | `docs/vision.json`, `docs/product-vision.md`, `docs/prd.md`, `docs/product-roadmap.md` |
| [Design System](skills/Design%20System/SKILL.md) | Translates images (screenshots, mockups, Figma URLs) into a design system in [Google's open design.md format](https://github.com/google-labs-code/design.md) — YAML design tokens plus prose rationale any coding agent can implement from. | `docs/design.md` |

### Build

Quality-gated feature work: nothing ships on "it compiles" — every increment is **built → reviewed → tested end to end → fixed** before the agent reports "done." Works from the Product Planner's roadmap or a direct prompt. Available in three flavors, adapted to each agent's native review tooling:

| Skill | Agent | Review mechanism |
|---|---|---|
| [Build Loop – Claude Code](skills/Build%20Loop%20-%20Claude%20Code/SKILL.md) | Claude Code | `/review`, plus `/security-review` for sensitive surfaces |
| [Build Loop – Codex](skills/Build%20Loop%20-%20Codex/SKILL.md) | OpenAI Codex CLI | `/review` on uncommitted changes, plus a custom security pass |
| [Build Loop – Cursor](skills/Build%20Loop%20-%20Cursor/SKILL.md) | Cursor | Cursor's `/review` |

**Trigger with:** "run the build loop", "build the next task", "continue the plan", or "build this feature properly".

### Launch

| Skill | What it does | Output |
|---|---|---|
| [Launch Checklist](skills/Launch%20Checklist/SKILL.md) | Audits your actual codebase — stack, services, env vars, payments, deploy config — then writes a plain-English, step-by-step path from "works on my machine" to "customers can use it." Every step is marked 🧑 you / 🤖 agent / 🤝 together, with time estimates, costs, and a "you'll know it worked when..." check. | `docs/launch-checklist.md` |

## The docs/ folder

All skills read and write a shared `docs/` folder at your project root. Run them in any order — each skill works alone, but when an upstream document exists, the next skill uses it to pre-fill its questions:

| Document | Written by | Consumed by |
|---|---|---|
| `product-idea.md` | Idea Generator | Idea Validator, Product Planner |
| `validation-report.md` | Idea Validator | you |
| `vision.json` | Product Planner (intake) | Product Planner (generation), Design System |
| `product-vision.md` | Product Planner | Design System, Build Loop |
| `prd.md` | Product Planner | Build Loop |
| `product-roadmap.md` | Product Planner | Build Loop (tracks progress via checkboxes) |
| `design.md` | Design System | Product Planner, Build Loop |
| `launch-checklist.md` | Launch Checklist | you |

## Getting started

1. Clone this repo or copy the skill folders you want into your project's skills directory (e.g. `.claude/skills/` for Claude Code).
2. Start anywhere in the lifecycle:
   - Have nothing? → "help me find an idea"
   - Have an idea? → "validate my idea"
   - Idea validated? → "plan my product"
   - Have a reference design? → "create a design system from this image"
   - Docs ready? → "run the build loop"
   - Product built? → "create my launch checklist"

## Changelog

See [CHANGELOG.md](CHANGELOG.md).
