---
name: design-better
description: Use when the user is building or refactoring UI in their product codebase and wants designer-quality frontend code — not just functional code. Triggers on phrases like "design better", "make this UI feel more designed", "elevate the design quality", "build this with design best practices", "follow UX heuristics", "make this feel professional", "apply design polish", "review this for craft", or any request to apply UX/UI craft heuristics to code generation or review. Pairs with `docs/design.md` when it exists — the design system file owns **style** (colors, typography, spacing tokens, components), this skill owns **craft** (hierarchy, interaction, accessibility, motion, polish). Reference `docs/design.md` tokens by name for every visual decision; apply the 50-item heuristics catalogue and the Laws of UX catalogue below for every craft decision. When `docs/design.md` is absent, fall back to the codebase's existing design conventions and recommend the **Design System** skill. When a heuristic implies a token the design system doesn't have, flag it as a New Pattern to add via the **Design System** skill — never invent a value inline. Fully standalone.
license: MIT
metadata:
  author: BuilderOS
  version: "1.1"
  compatibility: Reads `docs/design.md` when present; otherwise falls back to the codebase's existing design tokens/conventions. No required documents or other skills.
---

# Design Better

A craft layer for any coding agent generating or reviewing UI. Style is owned by `docs/design.md`; this skill owns the timeless UX/UI craft heuristics — drawn from Nielsen Norman Group, the [Laws of UX](https://lawsofux.com/), Shneiderman, Refactoring UI, WCAG 2.2, Don Norman, Apple HIG, Material 3, and 2025–2026 AI-product patterns — that separate "shipped" from "designed."

> **Boundary.** When a heuristic implies a visual property, reference a `docs/design.md` token by name (`{colors.primary}`, `{rounded.md}`). If `docs/design.md` doesn't have the token, flag it as a New Pattern to add via the **Design System** skill — never inline hex, font names, or arbitrary px. If `docs/design.md` doesn't exist at all, follow the codebase's existing convention for that property (theme config, CSS custom properties, component library) and note it.

## Inputs

- **`docs/design.md`** — the style layer (recommended, not required). When present, reference its tokens by name for every visual decision. When absent, this skill still applies: fall back to the codebase's existing design conventions (theme config, CSS custom properties, existing components) as the style source, and recommend running the **Design System** skill to establish a single token source of truth (part of BuilderOS: https://github.com/BuildGreatProducts/builder-os).
- **The thing being built or refactored** — a brief, a file, or uncommitted diff.
- **`docs/product-vision.md`** — optional. Its strategy and brand inform microcopy tone and help weight which heuristic categories dominate.

## Workflow

1. **Establish the style source.** If `docs/design.md` exists, parse its token catalogue (colors, typography, rounded, spacing, components) and the markdown rules (elevation, shape, Do's and Don'ts). If it doesn't, identify the codebase's existing design conventions — theme config (e.g. Tailwind/`tokens`), CSS custom properties, the component library in use — and treat those as the style source, then recommend the **Design System** skill so future work has a documented source of truth.

2. **Confirm scope and weight.** Restate the surface (signup / dashboard / chat / settings / detail / marketing) and which heuristic categories will dominate — a signup leans on Forms + States + Accessibility + Microcopy; a data dashboard leans on Hierarchy + IA + Cognitive Load; an AI chat leans on the AI-product subset.

3. **Generate or review using both layers.** The style source (`docs/design.md` tokens, or the codebase conventions) for every visual decision; the heuristics catalogue and the Laws of UX for every craft decision. Cite heuristics by number and laws by name in reviews (*"H22 — disabled instead of erroring after submit"*; *"Hick's Law — too many options in one step"*).

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

5. **Deliver.** Note which heuristic categories and Laws of UX dominated and surface any trade-off where a heuristic or law was knowingly skipped.

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

## The Laws of UX

The 50 heuristics above are the actionable checklist; the Laws of UX below are the psychological principles behind them — drawn from [lawsofux.com](https://lawsofux.com/). Apply them where relevant, especially the ones with no direct heuristic. Cite them by name in reviews (*"Hick's Law — too many options in one step"*). Where a heuristic already operationalizes a law, the `↔ H#` cross-reference points to it — treat the two as one rule seen from two angles: the law is the *why*, the heuristic is the *check*.

### Perception & grouping — how users visually parse a layout

**Law of Proximity.** Objects placed near each other are perceived as a group. Stack related fields and controls with a small `gap`; separate unrelated groups with a larger spacing token. ↔ H3

**Law of Common Region.** Elements inside a shared, clearly bounded area read as one group. When proximity isn't enough, bound related items with a card, divider, or tinted surface using elevation/surface tokens. ↔ H4

**Law of Similarity.** Elements that share visual characteristics are perceived as related. Give same-function elements the same treatment (all primary actions one button token, all links one style) and make genuinely different things look different. ↔ H16, H19

**Law of Uniform Connectedness.** Visually connected elements read as more related than merely nearby ones. Connect related controls with a shared container, background, or connecting line (a segmented control, a labelled field group) rather than relying on proximity alone.

**Law of Prägnanz.** People read ambiguous or complex forms as the simplest interpretation possible. Favor simple, regular shapes and clean layouts; reduce visual complexity so the underlying structure is obvious at a glance.

**Aesthetic-Usability Effect.** Users perceive aesthetically pleasing design as more usable and forgive minor issues. Invest in visual craft (spacing, type, polish per the heuristics) — but never let surface polish mask a real usability problem that testing surfaces.

### Choice & cognitive load — reduce the effort to decide and act

**Hick's Law.** Decision time grows with the number and complexity of choices. Reduce options per step, use progressive disclosure, pre-select a safe default, and break long flows into steps. ↔ H47

**Choice Overload.** Too many options overwhelm and stall users. Curate and rank choices, offer a recommended default, and filter or collapse the long tail instead of showing everything at once.

**Miller's Law.** Working memory holds ~7±2 items. Chunk content and navigation into small groups; keep top-level nav inside the magic number and push the rest into a menu or command palette. ↔ H47

**Chunking.** Information is easier to process when grouped into meaningful units. Format long strings (phone numbers, card numbers, IDs) and group form fields, nav, and content into digestible sets. ↔ H3

**Cognitive Load.** Every extra element, label, and step consumes mental resources. Cut extraneous load — offload memory to the interface (show, don't make users recall), remove decorative noise, and keep each screen to one primary job.

**Working Memory.** Users can't reliably hold values from a previous screen. Carry context forward for them — surface selected filters, prior inputs, and the current step — never require re-entering or remembering across screens. ↔ H48

**Selective Attention.** Users focus on the subset of the screen tied to their goal and filter out the rest (banner-blindness). Put critical info and actions in the goal path; don't hide them in ad-like regions or easily dismissed chrome.

**Occam's Razor.** Among equivalent solutions, the one with the fewest elements wins. Remove any element, option, or step that doesn't earn its place; the best interface is the simplest one that still does the job.

**Tesler's Law (Conservation of Complexity).** Every system has irreducible complexity — the only question is who absorbs it. Push it into the system, not the user: smart defaults, inference, and sensible automation over extra fields and choices. ↔ H27

**Cognitive Bias.** Users' judgments are shaped by systematic biases (anchoring, framing, defaults). Design around them honestly to help users decide well — never to manipulate. No dark patterns, forced continuity, or misleading framing.

**Pareto Principle.** Roughly 80% of use comes from 20% of features. Identify the high-frequency paths and make them fast and obvious; don't let rarely-used features crowd the common ones.

### Memory & attention — what users remember and are drawn to

**Von Restorff Effect (Isolation Effect).** Among similar items, the one that differs is remembered. Make the single primary action visually distinct; use emphasis sparingly so it keeps its meaning. ↔ H1

**Serial Position Effect.** People best remember the first and last items in a series. Place the most important nav items and actions at the start and end of a list, menu, or bar; let the least important sit in the middle.

**Zeigarnik Effect.** Uncompleted tasks are remembered better and create a pull to finish. Use progress indicators, checklists, and "X of Y complete" cues to nudge completion of onboarding, setup, and multi-step flows.

**Goal-Gradient Effect.** Motivation increases as a goal gets closer. Show progress and make the remaining steps feel small — progress bars, step counters, and a pre-filled first step so momentum starts above zero.

**Peak-End Rule.** People judge an experience by its most intense moment and its ending. Invest in the peak (the magic moment) and the end (success, first-win, sign-off states) — the success moment deserves real design, not a bare toast.

### Expectation & behavior — match how users actually behave

**Jakob's Law.** Users spend most of their time on other products and expect yours to work the same way. Follow established conventions and platform patterns for common UI (nav, forms, icons, gestures); reserve novelty for where it genuinely pays off. ↔ H20

**Mental Model.** Users act on what they believe your system is and how it works. Match their expected model — familiar labels, metaphors, and flows — and close the gap between their model and yours instead of forcing them to learn a new one. ↔ H48

**Paradox of the Active User.** Users skip manuals and start immediately, even when reading would be faster. Make the happy path self-evident, put guidance inline and in context (not in docs), and design so users succeed without instructions.

**Postel's Law.** Be liberal in what you accept, conservative in what you produce. Accept messy, varied input (trim whitespace, parse date and phone formats, stay case-insensitive) and respond with clear, consistent, well-formed output. ↔ H26, H27

**Parkinson's Law.** A task expands to fill the time available. Shorten time-to-complete with autofill, smart defaults, and sensible constraints so users finish faster than they otherwise would. ↔ H27

**Flow.** Users perform best when fully immersed in a task. Protect focus — minimize interruptions, avoid gratuitous modals and confirmations mid-task, preserve state, and keep the path to done unbroken.

### Performance & responsiveness — respect the user's time

**Doherty Threshold.** Productivity soars when system and user respond to each other in under 400 ms. Confirm actions fast — optimistic UI, skeletons, and immediate press feedback — so users never wait on the interface. ↔ H17

**Fitts's Law.** Time to hit a target depends on its distance and size. Make frequent and primary targets large and close to where the user already is; keep tap targets generous and place mobile primary actions in the thumb zone. ↔ H39, H43

## Handling New Patterns

When a heuristic needs a visual property that `docs/design.md` doesn't yet define (a new color role, a missing spacing step, an undocumented component variant), **never invent the value inline.** Instead:

- **If `docs/design.md` exists** — flag the gap as a **New Pattern** in your delivery notes (what property, what value the heuristic implies, and where it's needed), and recommend adding it to `docs/design.md` via the **Design System** skill, which owns editing the token catalogue and keeps `docs/design.html` in sync. Use the closest existing token in the meantime.
- **If `docs/design.md` doesn't exist** — follow the codebase's existing convention for that property (theme config, CSS custom property, the component library), note that you did, and recommend running the **Design System** skill so the project gains a documented source of truth.

## What "done" looks like

Generated or reviewed UI where the **pre-flight checklist** in workflow step 4 passes in full, every visual decision references a `docs/design.md` token (or a codebase convention when no design system exists), the primary action is the single visually dominant element (H1), microcopy is plain language, errors say what/why/how-to-fix (H32), the success moment is invested in (Peak-End Rule), and any token the design system doesn't yet have is flagged as a New Pattern to promote via the **Design System** skill rather than invented inline.

Recommended pairing: run this skill alongside a **Build Loop** — its review step (`/review`, and `/security-review` for sensitive surfaces) catches token drift and accessibility regressions that slip through — and feed any flagged New Patterns back into `docs/design.md` with the **Design System** skill before commit.
