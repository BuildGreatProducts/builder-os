---
name: design-better
description: Use when the user is building or refactoring UI in their product codebase and wants designer-quality frontend code — not just functional code. Triggers on phrases like "design better", "make this UI feel more designed", "elevate the design quality", "build this with design best practices", "follow UX heuristics", "make this feel professional", "apply design polish", "review this for craft", or any request to apply UX/UI craft heuristics to code generation or review. Pairs with `docs/design.md` when it exists: the design system file owns **style** (colors, typography, spacing tokens, components), this skill owns **craft** (hierarchy, interaction, accessibility, motion, polish). Reference `docs/design.md` tokens by name for every visual decision; apply the 50-item heuristics catalogue below for every craft decision. When `docs/design.md` is absent, fall back to the codebase's existing design conventions and recommend the **Design System** skill. When a heuristic implies a token the design system doesn't have, flag it as a New Pattern to add via the **Design System** skill — never invent a value inline. Fully standalone.
license: MIT
metadata:
  author: BuilderOS
  version: "1.0"
  compatibility: Reads `docs/design.md` when present; otherwise falls back to the codebase's existing design tokens/conventions. No required documents or other skills.
---

# Design Better

A craft layer for any coding agent generating or reviewing UI. Style is owned by `docs/design.md`; this skill owns the timeless UX/UI craft heuristics — drawn from Nielsen Norman Group, Laws of UX, Shneiderman, Refactoring UI, WCAG 2.2, Don Norman, Apple HIG, Material 3, and 2025–2026 AI-product patterns — that separate "shipped" from "designed."

> **Boundary.** When a heuristic implies a visual property, reference a `docs/design.md` token by name (`{colors.primary}`, `{rounded.md}`). If `docs/design.md` doesn't have the token, flag it as a New Pattern to add via the **Design System** skill — never inline hex, font names, or arbitrary px. If `docs/design.md` doesn't exist at all, follow the codebase's existing convention for that property (theme config, CSS custom properties, component library) and note it.

## Inputs

- **`docs/design.md`** — the style layer (recommended, not required). When present, reference its tokens by name for every visual decision. When absent, this skill still applies: fall back to the codebase's existing design conventions (theme config, CSS custom properties, existing components) as the style source, and recommend running the **Design System** skill to establish a single token source of truth (part of BuilderOS: https://github.com/BuildGreatProducts/builder-os).
- **The thing being built or refactored** — a brief, a file, or uncommitted diff.
- **`docs/product-vision.md`** — optional. Its strategy and brand inform microcopy tone and help weight which heuristic categories dominate.

## Workflow

1. **Establish the style source.** If `docs/design.md` exists, parse its token catalogue (colors, typography, rounded, spacing, components) and the markdown rules (elevation, shape, Do's and Don'ts). If it doesn't, identify the codebase's existing design conventions — theme config (e.g. Tailwind/`tokens`), CSS custom properties, the component library in use — and treat those as the style source, then recommend the **Design System** skill so future work has a documented source of truth.

2. **Confirm scope and weight.** Restate the surface (signup / dashboard / chat / settings / detail / marketing) and which heuristic categories will dominate — a signup leans on Forms + States + Accessibility + Microcopy; a data dashboard leans on Hierarchy + IA + Cognitive Load; an AI chat leans on the AI-product subset.

3. **Generate or review using both layers.** The style source (`docs/design.md` tokens, or the codebase conventions) for every visual decision; the heuristics catalogue for every craft decision. Cite heuristics by number in reviews (*"H22 — disabled instead of erroring after submit"*).

4. **Pre-flight checklist before delivering:**
   - Every interactive element has default, hover/focus-visible, active, disabled, and loading (where async) states.
   - Every async surface designs and codes all four states: empty, loading, error, success.
   - Every form input has a programmatically associated `<label>`, the correct `inputmode`/`type`, an `autocomplete` attribute, and `onBlur` validation.
   - Every motion is 150–400 ms, eases by direction (out entering / in exiting / in-out moving), animates `transform`/`opacity` only, and honors `prefers-reduced-motion`.
   - Body text contrast ≥ 4.5:1, UI components ≥ 3:1, focus rings ≥ 3:1 at ≥ 2 px.
   - Pointer targets ≥ 24×24 CSS px (≥ 44×44 on touch).
   - Layout works at 320 CSS px wide with no horizontal scroll.
   - The primary action is the single visually dominant element on the view.
   - Every visual decision references a `docs/design.md` token (or a codebase convention when no design system exists). Missing tokens flagged as New Patterns, not invented inline.

5. **Deliver.** Note which heuristic categories dominated and surface any trade-off where a heuristic was knowingly skipped.

## The heuristics catalogue (50 items)

Each entry: number (cite in reviews) + short name + one-line rule + code action. Style-neutral throughout — defer every visual property to `docs/design.md` (or the codebase's design conventions when it's absent).

### Visual hierarchy & emphasis

**H1. One primary action per view (Von Restorff).** Make the primary CTA the single visually distinct element — use the most prominent design token color and largest button size. Secondary actions use quieter tokens.

**H2. Hierarchy from size + weight + color, not size alone (Refactoring UI).** De-emphasize secondary text by reducing weight or shifting to a secondary text color token before reducing font size.

**H3. Proximity groups, white space separates (Gestalt).** Stack related fields with a small `gap`; separate unrelated groups with a larger spacing token.

**H4. Common Region bounds groups (Gestalt).** When proximity isn't enough, bound with a card, divider, or tinted surface — using the design system's elevation/surface tokens.

**H5. White space is a tool — default to too much (Refactoring UI).** Generous padding reads as confident. Tightening should be deliberate, not default.

**H6. Snap to the spacing scale; nothing one pixel off (Polish).** Every margin/padding/gap is a design token. Alignment to the grid is the difference between "designed" and "drafted."

### Layout & alignment

**H7. Semantic HTML before divs + ARIA (WCAG 1.3.1).** Use `<main>`, `<nav>`, `<header>`, `<section>`, `<article>`, `<button>`, `<label>`. Reach for `role=` only when no native element fits.

**H8. Reading line length 50–75ch.** Cap prose containers with `max-width: 65ch` (or analogous).

**H9. Reflow at 320 CSS px (WCAG 1.4.10).** Every view works at 320 px wide with no horizontal scroll.

**H10. Text spacing tolerance (WCAG 1.4.12).** Layout survives user overrides of line-height (≥ 1.5×), letter-spacing (≥ 0.12em), word-spacing (≥ 0.16em).

### Typography craft

**H11. Use only documented type levels.** Every `font-size`/`font-weight` references a typography token.

**H12. Tabular figures in data (`font-variant-numeric: tabular-nums`).** Numbers in tables, dashboards, and ledgers align vertically.

**H13. Modern text-wrap (`balance` on headlines, `pretty` on prose).** Headlines read more deliberately with `text-wrap: balance`; body paragraphs avoid orphans with `text-wrap: pretty`.

### Color & contrast (non-stylistic)

**H14. Body text contrast ≥ 4.5:1 (WCAG 1.4.3 AA).** Verify every body/background token pair. Large text (≥18.66 px bold or ≥24 px) needs ≥ 3:1.

**H15. UI component contrast ≥ 3:1 (WCAG 1.4.11).** Form borders, icon-only buttons, focus rings, chart strokes all need ≥ 3:1 vs adjacent color.

**H16. Color is never the sole signal.** Pair every color-encoded state with an icon, label, or pattern.

### Interaction & feedback

**H17. Feedback within 100 ms (Doherty / Norman).** Every action that triggers logic confirms visually within 100 ms (press state, ripple, optimistic update, skeleton).

**H18. Three-state minimum for interactive elements.** Default, hover or focus-visible, active, disabled. Loading is a fifth where async.

**H19. Affordances signal interactivity.** Buttons look pressable; links visibly differ from body text — use design tokens for the visual treatment.

**H20. Discoverability over hidden interactions (Norman).** Critical actions are visible by default — never hidden behind hover, long-press, or undisclosed gestures.

**H21. Easy reversal (Shneiderman).** Undo on destructive actions; Cancel on every modal; Back without data loss; soft-delete with restore.

**H22. Constraints prevent errors (Norman).** Disable invalid options; use `min`, `max`, `step`, `pattern`, `type` on inputs; gate submit on validity.

### Forms

**H23. Single-column layout, top-aligned labels.** Stack vertically; labels above inputs; never placeholder-as-label.

**H24. `inputmode` + `type` for mobile keyboards.** `type="email"`, `inputmode="numeric"`/`"decimal"`, `type="tel"`, `type="url"`.

**H25. `autocomplete` attributes.** `email`, `name`, `new-password`, `one-time-code`, `postal-code`, etc. Saves users 30 seconds and respects password managers.

**H26. Inline validation on blur, clear on change.** Validate `onBlur`; clear errors `onChange` once valid. Validating every keystroke punishes typos.

**H27. Smart defaults absorb complexity (Tesler).** Detect timezone, locale, currency; pre-select the safe option; parse natural language ("tomorrow 3 pm"); pre-fill from previous inputs.

### States: empty, loading, error, success

**H28. Four states, every data view.** Empty, loading, error, success — each designed and coded using design tokens.

**H29. Skeleton over spinner for content (NN/g).** Use shape-matching placeholders that match final layout dimensions (prevents CLS).

**H30. Optimistic UI for low-risk actions.** Likes, toggles, follows update immediately; reconcile silently; rollback with a toast on failure.

**H31. Empty states are first impressions.** Every empty list has a one-line explanation of what should be there + a primary CTA.

**H32. Errors say what / why / how to fix.** Never just "Something went wrong." Always offer a retry or recovery path.

### Motion

**H33. UI motion in the 150–400 ms band (NN/g, Material 3).** Micro 100–150 ms (hover, toggle); standard 200–300 ms (menu, modal); large 300–400 ms (page transition). Repeated UI never animates over 500 ms.

**H34. Easing matches direction.** Ease-out entering (`cubic-bezier(0, 0, 0.2, 1)`); ease-in exiting; ease-in-out for moves between two on-screen states.

**H35. Animate `transform` and `opacity` only.** GPU-accelerated. Never animate `width`/`height`/`top`/`left`.

**H36. Honor `prefers-reduced-motion`.** Wrap non-essential motion in `@media (prefers-reduced-motion: reduce)`. Replace transforms with instant changes or short opacity fades — never remove the feedback signal entirely.

### Accessibility (WCAG 2.2 AA)

**H37. Keyboard operability for everything (WCAG 2.1.1).** Every interactive element reachable via Tab / Shift+Tab / Enter / Space / Arrow. Focus traps in modals work.

**H38. Visible focus rings (WCAG 2.4.7).** Never `outline: none` without a `:focus-visible` equivalent — ≥ 2 px, ≥ 3:1 contrast. Use a design system focus token if defined.

**H39. Pointer target size ≥ 24×24 (WCAG 2.5.8).** Every clickable element ≥ 24×24 CSS px. On touch, aim for 44×44 (Apple HIG) or 48×48 (Material 3).

**H40. Programmatically associated labels (WCAG 3.3.2).** Every input has a `<label htmlFor=id>` or wraps the input. `aria-label` only when the input is genuinely unlabeled visually.

**H41. Focus not obscured (WCAG 2.4.11).** Sticky headers/footers must not cover the focused element. Set `scroll-padding-top`/`-bottom` equal to sticky heights.

**H42. Accessible authentication (WCAG 3.3.8).** No cognitive puzzles for login. Allow paste in password fields. Pair OTP with `autocomplete="one-time-code"`.

### Mobile & touch

**H43. Thumb-zone primary CTAs.** On mobile, primary actions land in the bottom half. Avoid top corners for high-frequency actions.

**H44. Respect safe-area insets.** Use `env(safe-area-inset-*)` on fixed/sticky bottom bars and full-bleed headers.

**H45. No hover-only critical info.** Tooltips shown only on `:hover` are invisible on touch. Show critical content by default.

**H46. Drag has a tap alternative (WCAG 2.5.7).** Every drag-to-sort/delete/resize has a button or menu equivalent.

### Cognitive load & navigation

**H47. Navigation ≤ 5–9 items (Miller).** Top-level nav stays inside the magic number; push the rest into a menu, command palette, or settings.

**H48. Recognition over Recall (NN/g #6).** Surface selected filters, current page (active nav state), breadcrumbs, recent items. Never make the user remember a value from a previous screen.

### AI-product specifics (2025–2026)

**H49. Stream + Stop is a primary control.** AI responses stream as they generate; a visible Stop button at primary affordance weight while streaming aborts the underlying request, not just the UI. Tool calls / thinking / searching surface as labeled steps.

**H50. AI output is an editable draft.** Treat model responses as drafts users can edit, accept partially, regenerate, or reject. Cite sources inline with one-click open. Distinguish error categories (network, rate limit, content filter, model error) so each has its own recovery path.

## Handling New Patterns

When a heuristic needs a visual property that `docs/design.md` doesn't yet define (a new color role, a missing spacing step, an undocumented component variant), **never invent the value inline.** Instead:

- **If `docs/design.md` exists** — flag the gap as a **New Pattern** in your delivery notes (what property, what value the heuristic implies, and where it's needed), and recommend adding it to `docs/design.md` via the **Design System** skill, which owns editing the token catalogue and keeps `docs/design.html` in sync. Use the closest existing token in the meantime.
- **If `docs/design.md` doesn't exist** — follow the codebase's existing convention for that property (theme config, CSS custom property, the component library), note that you did, and recommend running the **Design System** skill so the project gains a documented source of truth.

## What "done" looks like

Generated or reviewed UI where the **pre-flight checklist** in workflow step 4 passes in full, every visual decision references a `docs/design.md` token (or a codebase convention when no design system exists), the primary action is the single visually dominant element (H1), microcopy is plain language, errors say what/why/how-to-fix (H32), the success moment is invested in (Peak-End Rule), and any token the design system doesn't yet have is flagged as a New Pattern to promote via the **Design System** skill rather than invented inline.

Recommended pairing: run this skill alongside a **Build Loop** — its review step (`/review`, and `/security-review` for sensitive surfaces) catches token drift and accessibility regressions that slip through — and feed any flagged New Patterns back into `docs/design.md` with the **Design System** skill before commit.
