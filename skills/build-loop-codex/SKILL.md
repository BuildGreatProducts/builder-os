---
name: build-loop-codex
description: Use when building features with **Codex** (OpenAI Codex CLI) in any codebase and the work should go through a disciplined build → review → test → fix loop. Triggers on "run the build loop", "build the next task", "continue the plan", "build this feature properly", or any request to implement work from a plan file or a direct feature prompt. Builds from the plan (or the prompt if no plan exists), runs Codex's `/review` on uncommitted changes and fixes every issue found, tests and verifies the feature end to end, fixes anything testing surfaces, and reports back once complete. Repeats until all plan tasks are checked off.
license: MIT
metadata:
  author: BuilderOS
  version: "1.0"
---

# Codex Build Loop

Quality-gated feature work: nothing ships on "it compiles" — every increment is built, reviewed, tested end to end, and fixed before the user hears "done."

## Source of work

- **A plan file exists** (roadmap, refactor plan, or task list with `- [ ]` checkboxes — search the repo): work the first unchecked task. Tasks are ordered intentionally — never skip ahead. If the plan references spec docs, read only the sections relevant to the current task.
- **No plan (or the request is outside it):** build from the user's prompt. Restate it as a verifiable goal with 2–4 success criteria and confirm scope in one message before building.

## The loop

Run per task (or per prompted feature). Do not advance until every step passes.

1. **Build.** Implement exactly what the task specifies. Simplest implementation that satisfies it, surgical changes, no speculative scope. Match existing project conventions.

2. **Review.** Run **`/review`** and select **"Review uncommitted changes"**. If the change touches auth, payments, user input, or data access, run a second pass via **"Custom review instructions"** (e.g. "Focus on security vulnerabilities and unvalidated input"). Fix all findings in scope — bugs, security issues, edge cases, performance, style in files you touched. If the project has a design system spec (design tokens file, DESIGN.md, theme config), check UI changes against it — no hardcoded colors, type, or spacing that bypass tokens. Note pre-existing issues in untouched code for the report instead of fixing silently. Re-run `/review` until clean. If a finding contradicts the task or spec, the spec wins — flag the disagreement.

3. **Test end to end.** Run the task's verification step (or the success criteria). Run the full test suite — everything that passed before must still pass. Add tests for new logic. Then exercise the feature as a user would: run the app, walk the real flow including empty, loading, and error states.

4. **Fix.** Anything testing finds goes back through the loop: fix → `/review` → re-test. Never mark a failing task complete; never start the next task with the app broken.

5. **Continue.** Mark the task `- [x]`, update any progress/status line in the plan, and loop to the next task until the requested scope is complete.

6. **Report.** When done, tell the user: what was built and plan progress, review findings fixed and anything deferred, how it was verified (tests + flow walked), and what needs their attention next. Be honest about anything flaky or partially verified.

## Rules

- Skipped review or untested work = unfinished work.
- Don't relitigate plan decisions; if a task seems wrong, ask one specific question rather than guessing.
- Discovered work no task covers? Surface it and propose a task — never silently expand scope.
