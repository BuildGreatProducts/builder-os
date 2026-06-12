# Changelog

All notable changes to BuilderOS are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [0.5.1] - 2026-06-12

### Changed
- **Build MVP no longer touches git during the build**: removed the per-phase branch/push/PR workflow and added an explicit rule that no git or GitHub actions happen until every phase is complete. Phase boundaries are now verification-only. A new Wrap up section runs once at the end: initialize git (with a stack-appropriate `.gitignore`), create the initial commit, then ask the user whether to connect a remote repo — never creating or pushing one unprompted

## [0.5.0] - 2026-06-11

### Added
- **Build MVP** (`skills/build-mvp`): executes the entire `docs/product-roadmap.md` end to end — implementing, testing, and verifying every task in order, marking checkboxes, updating the status line, and opening a PR per phase until the magic moment works. Agent-agnostic; complements the per-task Build Loop skills

### Changed
- Build MVP refactored from ProductOS's `studio-deploy-mvp-build`: renamed to `build-mvp`, document paths rewired from ProductOS conventions (`docs/PRD.md`, `docs/ROADMAP.md`, `docs/PRODUCT.md`, `docs/DESIGN.md`) to BuilderOS ones (`docs/prd.md`, `docs/product-roadmap.md`, `docs/product-vision.md`, `docs/design.md`), and the missing-docs handoff now points to the Product Planner skill instead of `studio-deploy-prd-roadmap`

## [0.4.0] - 2026-06-11

### Added
- **`npx skills add` installation** via the [skills CLI](https://github.com/vercel-labs/skills): install everything with `npx skills add BuildGreatProducts/builder-os`, a single skill by path (`.../skills/product-planner`) or by name (`--skill product-planner`). README gained a full Installation section
- MIT `LICENSE.txt` at the repo root

### Changed
- **All skill folders renamed to kebab-case** (`Build Loop - Claude Code` → `build-loop-claude-code`, `Product Planner` → `product-planner`, etc.) so folders can be copied straight into `.claude/skills/` and addressed by the skills CLI path convention; Build Loop frontmatter names updated to match their folders (`claude-code-build-loop` → `build-loop-claude-code`, and likewise for Codex and Cursor)
- **Product Planner checks for `docs/product-idea.md` before starting the intake**: a new Step 0 reads the captured idea first, confirms it's still the direction, and pre-fills intake sections 1–3 — the "What do you want to build?" cold open now only runs when no captured idea exists
- Build Loop skills gained license/author/version frontmatter to match the rest of the catalog

## [0.3.0] - 2026-06-11

### Changed
- **Product Planner** now captures intake answers as a human-readable `docs/VISION.md` instead of `docs/vision.json`. The `VISION-SCHEMA.md` resource was replaced by `VISION-TEMPLATE.md`, which defines the markdown document structure, field rules, and validation steps; founders can read and edit the vision file directly and re-run the Planner to regenerate downstream docs
- **Tech stack options expanded** (`resources/TECH-STACK-OPTIONS.md`): added Vite + React, React Router (Remix), Nuxt, and Astro for web; SwiftUI and Jetpack Compose for native mobile; Firebase, Hono, FastAPI, and Ruby on Rails for backend; Neon, Turso/SQLite, and MongoDB Atlas for database; Better Auth, Firebase Auth, and WorkOS/Auth0 for auth; Paddle and Adapty for payments
- **Tech stack research unlocked**: the Product Planner is now explicitly instructed to research beyond the options file (web search) when the founder names an unlisted tool, the product has unusual needs, or a listed option may be stale — presenting researched options in the same comparison format with verification dates for volatile facts like pricing
- Design System skill updated to read `docs/VISION.md` for pre-filled context

## [0.2.0] - 2026-06-11

### Added
- **Idea Generator**: guided product-idea discovery from your business or expertise — candidate directions, five-axis scorecard, sharpened pick. Writes `docs/product-idea.md`
- **Idea Validator**: pressure-tests an idea — core assumption, fatal flaws, competition map, first 10 customers, 2-week MVP test, strong/weak/pivot verdict. Writes `docs/validation-report.md`
- **Product Planner**: structured vision intake followed by generation of `docs/product-vision.md`, `docs/prd.md`, and `docs/product-roadmap.md` from `docs/vision.json`
- **Design System**: translates image references into a `docs/design.md` design system (Google design.md format) with YAML tokens and prose rationale
- **Launch Checklist**: audits the codebase and writes a plain-English go-live guide to `docs/launch-checklist.md` with you/agent/together step markers

### Changed
- All five new skills refactored to be fully standalone: PLAID/ProductOS branding, cross-skill `npx` install commands, and pipeline assumptions removed; skills now reference each other by BuilderOS name and degrade gracefully when a sibling skill isn't installed
- All skill outputs standardized to the `docs/` folder at the project root; `vision.json` moved from the project root to `docs/vision.json`
- Product Planner no longer requires Node.js: the `validate-vision.js` script dependency was replaced with direct validation against the schema in `resources/VISION-SCHEMA.md`, and the vision schema was reset to a clean v1.0 (removed `plaidVersion` field and migration machinery)
- Product Planner tech-stack intake no longer hard-leans toward specific vendors; recommendations now come from the comparison data in `resources/TECH-STACK-OPTIONS.md` based on product needs
- Launch Checklist output renamed from `docs/DEPLOY.md` to `docs/launch-checklist.md`
- README rewritten around the full Ideate → Plan & Design → Build → Launch lifecycle, including a `docs/` document-flow map

## [0.1.0] - 2026-06-11

### Added
- **Build Loop – Claude Code**: quality-gated build → review → test → fix workflow for Claude Code, using `/review` and `/security-review`
- **Build Loop – Codex**: the same workflow adapted for OpenAI Codex CLI, using `/review` on uncommitted changes with a custom security pass for sensitive surfaces
- **Build Loop – Cursor**: the same workflow adapted for Cursor's `/review`
- README outlining the BuilderOS vision, skill catalog, and getting-started guide
- This changelog
