# BuilderOS

**An operating system for builders.** BuilderOS is a collection of agent skills that give product builders a repeatable system for ideating, designing, and building software products with AI — turning coding agents from autocomplete into disciplined collaborators that ship reviewed, tested work.

## Why

AI coding agents are fast, but speed without discipline produces features that "compile" rather than features that work. BuilderOS encodes the habits of strong product teams — plan-driven work, mandatory review, end-to-end testing, honest reporting — as skills your agent follows automatically.

## How it works

Each skill is a `SKILL.md` file that teaches an AI coding agent a workflow. Install the skill for the agent you use, then trigger it with a natural prompt — the agent picks up the workflow and follows it until the work is genuinely done.

```
skills/
├── Build Loop - Claude Code/   # for Anthropic's Claude Code
├── Build Loop - Codex/         # for OpenAI's Codex CLI
└── Build Loop - Cursor/        # for Cursor
```

## Skills

### Build Loop

A quality-gated feature workflow: nothing ships on "it compiles" — every increment is **built → reviewed → tested end to end → fixed** before the agent reports "done." Available in three flavors, adapted to each agent's native review tooling:

| Skill | Agent | Review mechanism |
|---|---|---|
| [Build Loop – Claude Code](skills/Build%20Loop%20-%20Claude%20Code/SKILL.md) | Claude Code | `/review`, plus `/security-review` for sensitive surfaces (auth, payments, user input, data access) |
| [Build Loop – Codex](skills/Build%20Loop%20-%20Codex/SKILL.md) | OpenAI Codex CLI | `/review` on uncommitted changes, plus a custom security-focused review pass for sensitive surfaces |
| [Build Loop – Cursor](skills/Build%20Loop%20-%20Cursor/SKILL.md) | Cursor | Cursor's `/review` |

**What the loop does:**

1. **Build** — works the first unchecked task from your plan file (or builds from your prompt, confirming scope and success criteria first). Surgical changes, no speculative scope, matches project conventions.
2. **Review** — runs the agent's code review and fixes *every* finding in scope: bugs, security issues, edge cases, performance. Checks UI changes against your design system spec so nothing bypasses design tokens. Re-runs review until clean.
3. **Test end to end** — runs the full test suite, adds tests for new logic, then exercises the feature as a real user would — including empty, loading, and error states.
4. **Fix** — anything testing surfaces goes back through the loop. A failing task is never marked complete.
5. **Continue** — checks the task off the plan and moves to the next, until the requested scope is done.
6. **Report** — tells you what was built, what review found and fixed, how it was verified, and what needs your attention — honestly, including anything flaky or only partially verified.

**Trigger it with:** "run the build loop", "build the next task", "continue the plan", or "build this feature properly".

## Getting started

1. Clone this repo or copy the skill folder for your agent into your project's skills directory (e.g. `.claude/skills/` for Claude Code).
2. Optionally add a plan file to your repo — any markdown task list with `- [ ]` checkboxes works.
3. Ask your agent to "run the build loop."

## Roadmap

BuilderOS covers the full product-building loop. Build is here today; skills for the earlier phases are coming:

- **Ideate** — finding, validating, and pressure-testing product ideas
- **Design** — turning ideas into specs, design systems, and buildable plans
- **Build** — disciplined, quality-gated implementation ✅

## Changelog

See [CHANGELOG.md](CHANGELOG.md).
