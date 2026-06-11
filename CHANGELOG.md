# Changelog

All notable changes to BuilderOS are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

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
