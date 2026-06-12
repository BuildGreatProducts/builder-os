---
name: build-mvp
description: Use inside a product repository when the user wants the full MVP built from their BuilderOS spec documents. Triggers on phrases like "build my MVP", "build the app", "execute the roadmap", "start the build", "work through the whole roadmap", "build everything", or any request to implement the entire plan rather than a single task or phase. Requires `docs/prd.md` and `docs/product-roadmap.md` (plus `docs/product-vision.md` and `docs/design.md` for context). Works through every roadmap task in order — implementing, testing, and verifying each before moving on, marking checkboxes and updating the status line — and runs until all tasks are complete and the magic moment works end to end, then initializes git with an initial commit and offers to connect a remote repo.
license: MIT
metadata:
  author: BuilderOS
  version: "1.0"
---

# Build MVP

Build the complete app by executing every task in `docs/product-roadmap.md`, in order, until all tasks are checked off.

## Setup

Read `docs/product-roadmap.md` first — it is the source of truth for what to build and in what order. `docs/prd.md` is the technical spec behind it; `docs/design.md` holds the visual design tokens; `docs/product-vision.md` holds the product strategy. Do not load these documents wholesale — each phase lists the specific Reference sections to read, plus whatever a task's Notes line points to. If `docs/product-roadmap.md` or `docs/prd.md` is missing, stop and tell the user to run the **Product Planner** skill first (part of BuilderOS: https://github.com/BuildGreatProducts/builder-os).

## Work loop

Repeat until every task in the roadmap is complete:

1. **Find the first unchecked task** (`- [ ]`). Tasks are ordered intentionally — never skip ahead.
2. **Read what the task needs** — its Files and Notes lines, plus the current phase's Reference sections if not yet read this session.
3. **Implement the task** exactly as specified — file paths, package names, and config values are deliberate. Follow the repo's `CLAUDE.md`/`AGENTS.md` guidelines: simplest implementation that satisfies the task, no speculative features, surgical changes only.
4. **Test and verify before moving on.** Run the verification step at the end of the task's Notes, run the app, run existing tests, and add tests for new logic. If verification fails, fix it first — never mark a failing task complete or start the next task with the app broken.
5. **Mark the task complete** — change `- [ ]` to `- [x]` and update the header status line (`**Status:** X/Y tasks complete`, `**Current Phase:** ...`).
6. **At each phase boundary:** run the app end to end and confirm the phase's Goal is true and demoable before starting the next phase. No git or GitHub actions here — version control happens once at the end, after every phase is complete.

## Rules

- The PRD's stack choices are final — implement them, never substitute alternatives.
- Visual styling comes from `docs/design.md` tokens — never invent colors, type, or spacing. If `docs/design.md` doesn't exist, follow the roadmap's foundation-phase guidance: prompt the user to run the **Design System** skill before styling work begins.
- If a task is ambiguous or conflicts with the PRD, check the PRD section it references; if still unclear, ask one specific question rather than guessing.
- If necessary work isn't covered by any task, surface it and propose adding a task — don't silently expand scope.
- Keep going until `**Status:** Y/Y tasks complete`: every task checked, every phase verified, the magic moment working end to end.
- No git or GitHub actions (init, commits, branches, pushes, PRs) until every phase is complete — version control is the wrap-up step, not part of the work loop.

## Wrap up

When every task is checked off and the magic moment has been verified end to end:

1. **Initialize git and create the initial commit.** If the project isn't a git repository yet, run `git init`, add a sensible `.gitignore` for the stack (dependencies, build output, `.env` files — never commit secrets), and commit everything with an initial commit message naming the product and noting the MVP build is complete. If the project is already a git repository, commit the build on the current branch instead.
2. **Ask the user if they want to connect a remote repo.** Don't create or push anything on your own — ask: "Want me to connect this to a remote repo? I can create one on GitHub (public or private) or connect an existing URL." If yes, set it up as `origin`, push, and confirm the push succeeded. If no, leave it local and tell them the commit is ready to push whenever they are.
