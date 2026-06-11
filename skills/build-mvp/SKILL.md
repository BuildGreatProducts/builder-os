---
name: build-mvp
description: Use inside a product repository when the user wants the full MVP built from their BuilderOS spec documents. Triggers on phrases like "build my MVP", "build the app", "execute the roadmap", "start the build", "work through the whole roadmap", "build everything", or any request to implement the entire plan rather than a single task or phase. Requires `docs/prd.md` and `docs/product-roadmap.md` (plus `docs/product-vision.md` and `docs/design.md` for context). Works through every roadmap task in order — implementing, testing, and verifying each before moving on, marking checkboxes and updating the status line, opening a PR per phase — and runs until all tasks are complete and the magic moment works end to end.
license: MIT
metadata:
  author: BuilderOS
  version: "1.0"
---

# Build MVP

Build the complete app by executing every task in `docs/product-roadmap.md`, in order, until all tasks are checked off.

## Setup

Read `docs/product-roadmap.md` first — it is the source of truth for what to build and in what order. `docs/prd.md` is the technical spec behind it; `docs/design.md` holds the visual design tokens; `docs/product-vision.md` holds the product strategy. Do not load these documents wholesale — each phase lists the specific Reference sections to read, plus whatever a task's Notes line points to. If `docs/product-roadmap.md` or `docs/prd.md` is missing, stop and tell the user to run the **Product Planner** skill first (part of BuilderOS: https://github.com/BuildGreatProducts/BuilderOS).

## Work loop

Repeat until every task in the roadmap is complete:

1. **Find the first unchecked task** (`- [ ]`). Tasks are ordered intentionally — never skip ahead.
2. **Read what the task needs** — its Files and Notes lines, plus the current phase's Reference sections if not yet read this session.
3. **Implement the task** exactly as specified — file paths, package names, and config values are deliberate. Follow the repo's `CLAUDE.md`/`AGENTS.md` guidelines: simplest implementation that satisfies the task, no speculative features, surgical changes only.
4. **Test and verify before moving on.** Run the verification step at the end of the task's Notes, run the app, run existing tests, and add tests for new logic. If verification fails, fix it first — never mark a failing task complete or start the next task with the app broken.
5. **Mark the task complete** — change `- [ ]` to `- [x]` and update the header status line (`**Status:** X/Y tasks complete`, `**Current Phase:** ...`).
6. **At each phase boundary:** run the app end to end and confirm the phase's Goal is true and demoable. Commit on a branch named `phase-{N}/{phase-slug}`, push, and open a PR titled `Phase {N}: {Phase Title}` with the goal, completed task range, and what to verify. If an automated review agent is configured, address its feedback before continuing.

## Rules

- The PRD's stack choices are final — implement them, never substitute alternatives.
- Visual styling comes from `docs/design.md` tokens — never invent colors, type, or spacing. If `docs/design.md` doesn't exist, follow the roadmap's foundation-phase guidance: prompt the user to run the **Design System** skill before styling work begins.
- If a task is ambiguous or conflicts with the PRD, check the PRD section it references; if still unclear, ask one specific question rather than guessing.
- If necessary work isn't covered by any task, surface it and propose adding a task — don't silently expand scope.
- Keep going until `**Status:** Y/Y tasks complete`: every task checked, every phase verified, the magic moment working end to end.
