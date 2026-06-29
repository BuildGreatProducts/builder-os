---
name: design-system
description: |
  Translates an image (or a set of image references — screenshots,
  mockups, Figma URLs, live websites) into two mirrored design-system
  artifacts: `docs/design.md` (YAML tokens + prose, following Google's open
  [design.md](https://github.com/google-labs-code/design.md) format, for the
  coding agent) and `docs/design.html` (a self-contained, token-driven style
  guide rendering every token and component live, for the human to read).
  Reads the imagery, asks targeted clarifying questions, derives the design
  tokens (colors, typography, spacing, rounded, components), and writes both
  files. Fully standalone — requires no other document or skill. Use when the
  founder says "create a design system", "design from image",
  "translate image to design", "create design.md", "image to design
  system", "extract design tokens", or shares an image with no other
  clear intent.
license: MIT
metadata:
  author: BuilderOS
  version: "1.1"
  compatibility: Requires file system access to write the docs/ directory. Optional Figma MCP for Figma URLs.
---

# Design System — Image to design.md + design.html

This skill takes an image (or a set of image references) and translates them into a design system captured as **two mirrored files**:

- **`docs/design.md`** — a YAML token block in [Google's open design.md format](https://github.com/google-labs-code/design.md) that gives a coding agent exact implementation values, plus prose rationale explaining the *why*. This is the **source of truth**.
- **`docs/design.html`** — a self-contained, human-readable style guide that renders every token and component live in a browser, styled directly from the same token values. This is the **mirror** the human reads.

Same design system, two audiences: the agent reads the `.md`, the human opens the `.html`. They must always be written and updated together so they never drift.

## When to Use This

- Founder shares a screenshot, mockup, Figma file, inspiration board, or live website and wants it captured as a structured design system
- Founder needs design tokens a coding agent can implement without guessing
- Founder wants a precise token spec to supplement an existing product vision or PRD
- Fully standalone — does not require any other document

## Modes

**No image provided yet:** Ask for one (or more) before doing anything else. Don't draft a design.md from imagination.

**`docs/design.md` already exists:** Read it (and `docs/design.html` if present) and ask what they want to do — refine specific tokens or sections, replace it with a fresh analysis from new imagery, or merge the new analysis into the existing tokens. Confirm before destructive overwrites. Whatever changes, regenerate `docs/design.html` so it stays in sync with the `.md`.

**Partial conversation:** If the session is interrupted mid-flow, note where you left off and resume from that step. Don't restart.

-----

## Voice

You are a senior design director with strong taste. You're observant — you describe what you actually see in the imagery, not what you assume. You're decisive — when the founder is uncertain, recommend a direction with a one-line rationale. You're systematic — you treat design as a coordinated system of tokens and rules, not just a vibe.

Don't flatter weak references. If the imagery is conflicting, contradictory, or thin, say so and ask which direction to anchor on.

-----

## Step 0: Image Intake

Open with:

> **"Share the image (or images) you want me to translate. I can work with screenshots, mockups, Figma URLs, live websites, or a mix. If you have multiple, tell me which is the primary anchor and which are inspiration references."**

Accept any of these inputs:

- **Local image paths** (PNG / JPG / WebP / screenshots) — read with the `Read` tool. The `Read` tool renders image content visually for analysis.
- **Figma URLs** (`figma.com/design/...`, `figma.com/board/...`, `figma.com/make/...`) — use the Figma MCP tools (`get_design_context`, `get_screenshot`, `get_metadata`). Extract `fileKey` and `nodeId` from the URL per the Figma server's URL parsing rules.
- **Live website URLs** — note that you cannot screenshot arbitrary URLs without browser tooling; ask the founder to paste a screenshot, or use `WebFetch` to read content/styles only as a supplementary signal (not the primary visual source).
- **A combination** of the above.

If only one image is provided, treat it as the primary anchor. If multiple, confirm which is the anchor and which are references for mood/inspiration.

If the founder provides no image after one prompt, offer a fallback: "I can draft a starter design.md from a text description of the brand and we'll refine from there — but the result will be weaker than working from imagery. Want to proceed that way, or grab a reference first?"

-----

## Step 1: Image Analysis

Read every image carefully before asking any questions. Don't generalize — describe what you actually see.

For each image, extract and note:

- **Colors** — Approximate hex values for backgrounds, surfaces, primary text, secondary text, accents, borders, and any semantic states (success / warning / error / info) you can spot. Note dominant vs. accent. Light or dark mode? Any obvious contrast pairs?
- **Typography** — Typeface character (geometric sans, humanist sans, transitional serif, slab, display, mono, etc.). Visible hierarchy levels. Approximate sizes and weights. Letter-spacing tendencies (tight display, neutral body). Any uppercase / smallcaps usage.
- **Spacing & density** — Tight, comfortable, or generous? Consistent rhythm or improvised? Any visible scale (e.g., 4 / 8 / 16 / 24 / 32)?
- **Shapes** — Corner radius philosophy (sharp 0px, slight 4px, rounded 8–12px, very rounded 16–24px, fully rounded). Does it vary by component class (e.g., chips fully rounded, cards slight)?
- **Elevation** — Soft shadows, hard shadows, borders only, both, or completely flat? Any layering?
- **Components** — What atoms are visible (buttons, inputs, chips, cards, nav, tables, modals, toasts)? What variants and states?
- **Mood** — Two or three concrete adjectives. "Editorial and minimal," "playful and dense," "industrial and high-contrast," "warm and approachable," "futuristic and monochrome."

Then summarize what you saw to the founder in 5–8 tight bullets. Be specific. Mirror back the imagery's actual character. If two references conflict, name the conflict.

-----

## Step 2: Context Questions

Ask questions one at a time. Offer 3 tailored suggestions for each (drawn from your Step 1 analysis). Carry every answer forward as context for later suggestions. If `docs/VISION.md` or `docs/product-vision.md` exists (created by the Product Planner skill), read it and skip questions already covered there — acknowledge what's known instead of re-asking.

1. **What is this design for?** — Product name, what it does, who uses it. One sentence. (Skip if `docs/VISION.md` already answers this.)
2. **Emotional tone** — Three adjectives describing how the product should feel. Suggest from the mood you observed.
3. **Audience and context of use** — Who looks at this, on what device, in what mode (focused work / casual browse / repeated daily use)?
4. **Color role assignments** — From the colors you spotted, which is `primary` (most-used brand surface), which is `accent` (interactive emphasis), which carries semantic meaning? Light mode, dark mode, or both? Suggest a mapping.
5. **Typography decisions** — Confirm typeface choice. What's the type scale (display / h1 / h2 / h3 / body / caption / mono)? Any anti-pattern fonts to avoid (e.g., "never serif")?
6. **Spacing density** — Tight, comfortable, or generous? Suggest based on observed density.
7. **Shape language** — Sharp, soft, fully rounded, or mixed? What does that signal about the brand?
8. **Elevation philosophy** — Shadows, borders, both, or flat? Recommend based on what you saw.
9. **Component priorities** — Which components matter most for the MVP? Cap at 6–10. Variants and states (hover / active / disabled / pressed) count as separate entries.
10. **Anti-patterns** — Three things this design must never become. Critical — these become the Don'ts section and protect the system over time.

If an answer is vague, push back gently with a recommendation rather than another open-ended question.

-----

## Step 3: Token Derivation

Synthesize the YAML token block. Follow the schema below precisely — it's what the design.md spec validates against.

### Token block shape

```yaml
version: alpha
name: <product-or-design-system-name>
description: <one-sentence description>

colors:
  <semantic-token>: "#RRGGBB"

typography:
  <scale-token>:
    fontFamily: <family>
    fontSize: <px | rem | em>
    fontWeight: <number, e.g. 400, 600, 700>
    lineHeight: <unitless multiplier or dimension>
    letterSpacing: <dimension, optional>
    fontFeature: <string, optional>
    fontVariation: <string, optional>

rounded:
  <scale>: <dimension>

spacing:
  <scale>: <dimension or unitless number>

components:
  <component-name>:
    backgroundColor: "{colors.<token>}"
    textColor: "{colors.<token>}"
    typography: "{typography.<token>}"
    rounded: "{rounded.<token>}"
    padding: <dimension or token reference>
    size: <dimension, optional>
    height: <dimension, optional>
    width: <dimension, optional>
```

### Rules

- **Hex colors** are quoted strings prefixed with `#` (sRGB). Example: `"#1A1C1E"`.
- **Dimensions** use `px`, `em`, or `rem`. Letter-spacing may use a negative em (e.g., `-0.02em`).
- **Component property values** should reference tokens with `{path.to.token}` syntax wherever a token exists. Inline literal dimensions only when no matching token applies.
- **Variants** (hover, active, disabled, pressed, focus) are separate component entries with a related key — `button-primary` and `button-primary-hover`, not nested children.
- **Semantic color names** beat appearance-based names. Use `primary`, `on-primary`, `surface`, `on-surface`, `accent`, `error`, `success`, `warning`, `info` — not `blue`, `red`, `lightGray`.
- **Valid component property names** (per spec): `backgroundColor`, `textColor`, `typography`, `rounded`, `padding`, `size`, `height`, `width`. Unknown properties are accepted by parsers but trigger warnings — avoid them unless deliberate.
- **No duplicate `##` headings** in the prose (the spec rejects files with duplicates).

-----

## Step 4: Prose Drafting

Draft prose for the eight canonical sections, in this exact order. Each section should be tight (3–8 sentences). Don't pad. Don't restate the YAML — explain the *why* behind it so a coding agent can make sound choices in cases the tokens don't cover.

1. **Overview** — Product, audience, emotional response, and one or two anti-patterns. The brand-and-style north star.
2. **Colors** — Palette intent. What `primary`, `accent`, `surface`, and semantic colors do, and why those specific values. Note contrast considerations (WCAG AA at minimum for text).
3. **Typography** — Typeface choice and its character. The type scale's intent — what each level is for. Any pairing logic.
4. **Layout** — Spacing scale, grid model (if any), density philosophy. Margin / gutter / container approach.
5. **Elevation & Depth** — Shadow scale or border-and-contrast strategy. Why this choice for this brand.
6. **Shapes** — Corner radius philosophy. When sharp vs. rounded, and what each signals.
7. **Components** — How buttons, inputs, chips, cards behave. Variant rules and state behavior. Reference the YAML tokens by name.
8. **Do's and Don'ts** — 4–6 do's and 4–6 don'ts. Specific and enforceable. Drawn from the anti-patterns and aesthetic intent.

-----

## Step 5: Confirm and Write

Before writing, show the founder a brief outline:

- The YAML token names you've picked (color tokens, type scale levels, rounded scale, spacing scale, component list)
- A one-line summary of each prose section

Ask for any last edits. Then write to `docs/design.md`. Create the `docs/` directory if it doesn't exist. This is the source of truth — once it's written and verified, Step 6 generates the `design.html` mirror from it.

### File format

```markdown
---
version: alpha
name: <Name>
description: <One-sentence description>
colors:
  ...
typography:
  ...
rounded:
  ...
spacing:
  ...
components:
  ...
---

# <Name> Design System

## Overview
...

## Colors
...

## Typography
...

## Layout
...

## Elevation & Depth
...

## Shapes
...

## Components
...

## Do's and Don'ts
...
```

After writing, verify the write succeeded before confirming. If the write fails, surface a clear, user-friendly message based on the cause:

- **Permission denied** → "I couldn't save `docs/design.md` because the directory isn't writable. Check folder permissions and try again."
- **No space left on device (ENOSPC)** → "The disk is full — free up space and I'll retry the save."
- **Existing file conflict** (read-only or unexpected contents) → "A `docs/design.md` already exists and I can't overwrite it. Want me to save under a different name or overwrite?"
- **Any other error** → Report the error message verbatim and ask how to proceed.

Only confirm "saved" after the write is verified successful, then proceed to Step 6 to build the matching `design.html`.

-----

## Step 6: Build the design.html mirror

`docs/design.html` is the human-readable twin of `docs/design.md`. Same system, two audiences: the `.md` gives the coding agent exact tokens and rationale; the `.html` lets a person *see* the system in a browser — every token and component rendered live in its real styling. They are mirrors and must never drift: **`design.md` is the source of truth; `design.html` is generated from it.** Build the HTML from the token values you just wrote, not from a fresh interpretation of the imagery.

Build a single self-contained `.html` file to these requirements:

- **Self-contained and dependency-free.** One file that opens directly in any browser — all CSS inline in a `<style>` block, no build step, no frameworks, no external JS. Web fonts may load via a `<link>` to Google Fonts (or a CDN) when the typeface needs it; otherwise use the system font stack.
- **Token-driven.** Declare every token from the YAML as a CSS custom property in `:root` (e.g. `--color-primary`, `--type-h1-size`, `--rounded-md`, `--space-4`). Every swatch, specimen, and component styles itself from those variables — so the page renders the exact same values that live in the `.md`, and changing a variable updates everything. Don't hardcode values that exist as tokens.
- **Mirror the md's structure and order**, so a reader can hold the two side by side.
- If the system defines **both light and dark modes**, include a small theme toggle (a few lines of vanilla JS flipping a `data-theme` attribute on `<html>`) and define both token sets. Otherwise render the single mode.

Sections, in this order:

1. **Header** — design system name, the one-line description, and a note that this file is the human-readable mirror of `docs/design.md`.
2. **Colors** — a swatch for every color token: the color block, the token name, the hex value, and a line of text in the paired `on-` color to show the combination. Group semantic states (success / warning / error / info) together.
3. **Typography** — render each type-scale level as a live specimen at its real family / size / weight / line-height / letter-spacing, with the token name and its spec listed beside it.
4. **Spacing** — a visual bar or box for each spacing step, labeled with the token and value, so the rhythm is visible.
5. **Radius** — a sample box rendered at each `rounded` value, labeled.
6. **Elevation & Depth** — a card for each elevation level (its shadow or border strategy), labeled.
7. **Components** — every component from the `components:` block, built out and rendered live, including each variant and state (default / hover / active / disabled / focus). Group related entries together (e.g. all button variants). Where a state like hover can't be triggered statically, render a labeled copy already in that state so it's visible without interaction.
8. **Do's and Don'ts** — the same do's and don'ts as the md, laid out as two readable columns (✓ do / ✗ don't), with a small visual example where it helps.

Keep the page's own chrome (layout, labels, section headers) clean and neutral — this is a reference style guide, not a marketing page or the product UI. The tokens and components should be what stands out.

Write to `docs/design.html`. Verify the write succeeded using the same error handling as the `.md` write (permission denied, no space, existing-file conflict, other — report and ask how to proceed). The `.md` and `.html` are always written together — never leave one updated and the other stale.

-----

## Step 7: Handoff

After writing both files, say:

> "Your design system is captured in two mirrored files:
> - **`docs/design.md`** — YAML tokens + prose for any coding agent to implement from (the source of truth).
> - **`docs/design.html`** — a human-readable style guide; open it in a browser to see every token and component rendered live.
>
> They're the same system — one for the agent, one for you. When tokens change, both update together."

Then suggest the natural next step based on project state:

- If `docs/prd.md` exists → "Want me to update the PRD's Design System section so it references these tokens?"
- If `docs/product-vision.md` exists but `docs/prd.md` does not → "Run the **Product Planner** skill to generate the PRD — it'll consume these tokens directly."
- If neither exists → "Run the **Product Planner** skill if you want to wrap this design into a full product vision and PRD."

If the Product Planner skill isn't installed, mention that it's part of BuilderOS: https://github.com/BuildGreatProducts/builder-os.

-----

## Editing the design system

`docs/design.md` is canonical; `docs/design.html` is its rendered mirror. **Any change to one must be reflected in the other in the same edit** — never let them drift. Make the change in the `.md` first, then update the matching part of the `.html` (or regenerate it). If the founder hand-edits the `.html`, fold the change back into the `.md` tokens too.

If the founder wants to refine after the files exist:

- **Change a single token** — Update the YAML and any prose that references the old value, then update the corresponding CSS custom property and any affected component in `design.html`. Keep YAML, prose, and HTML in sync.
- **Reanalyze with a new image** — Read the new image, summarize what changed, and ask whether to replace the existing tokens or merge specific ones. Regenerate `design.html` from the resulting tokens.
- **Rewrite a prose section** — Update only that section in the `.md`. Leave the YAML and HTML intact unless the founder also wants tokens changed.
- **Add a component** — Append a new entry under `components:`, add a paragraph in the Components prose section, and render the new component (with its variants/states) in the Components section of `design.html`.

In the `.md`, always preserve canonical section order and never create duplicate `##` headings — the design.md spec rejects files with duplicates.
