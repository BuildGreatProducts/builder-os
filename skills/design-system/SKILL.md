---
name: design-system
description: |
  Translates an image (or a set of image references — screenshots,
  mockups, Figma URLs, live websites) into a high-quality `docs/design.md`
  file following Google's open
  [design.md](https://github.com/google-labs-code/design.md) format.
  Reads the imagery, asks targeted clarifying questions, derives YAML
  design tokens (colors, typography, spacing, rounded, components),
  and writes prose rationale that any AI coding agent can consume.
  Fully standalone — requires no other document or skill. Use when the
  founder says "create a design system", "design from image",
  "translate image to design", "create design.md", "image to design
  system", "extract design tokens", or shares an image with no other
  clear intent.
license: MIT
metadata:
  author: BuilderOS
  version: "1.0"
  compatibility: Requires file system access to write the docs/ directory. Optional Figma MCP for Figma URLs.
---

# Design — Image to design.md Translation

This skill takes an image (or a set of image references) and translates them into a high-quality `design.md` file in [Google's open design.md format](https://github.com/google-labs-code/design.md). The output is `docs/design.md` — a YAML token block that gives a coding agent exact implementation values, plus prose rationale that explains the *why* behind those tokens.

## When to Use This

- Founder shares a screenshot, mockup, Figma file, inspiration board, or live website and wants it captured as a structured design system
- Founder needs design tokens a coding agent can implement without guessing
- Founder wants a precise token spec to supplement an existing product vision or PRD
- Fully standalone — does not require any other document

## Modes

**No image provided yet:** Ask for one (or more) before doing anything else. Don't draft a design.md from imagination.

**`docs/design.md` already exists:** Read it and ask what they want to do — refine specific tokens or sections, replace it with a fresh analysis from new imagery, or merge the new analysis into the existing tokens. Confirm before destructive overwrites.

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

Ask for any last edits. Then write to `docs/design.md`. Create the `docs/` directory if it doesn't exist.

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

Only confirm "saved" after the write is verified successful.

-----

## Step 6: Handoff

After writing `docs/design.md`, say:

> "Your design system is captured at `docs/design.md`. The YAML front matter gives any coding agent exact tokens to implement. The prose explains the *why* so they can make informed choices when the tokens don't cover an edge case."

Then suggest the natural next step based on project state:

- If `docs/prd.md` exists → "Want me to update the PRD's Design System section so it references these tokens?"
- If `docs/product-vision.md` exists but `docs/prd.md` does not → "Run the **Product Planner** skill to generate the PRD — it'll consume these tokens directly."
- If neither exists → "Run the **Product Planner** skill if you want to wrap this design into a full product vision and PRD."

If the Product Planner skill isn't installed, mention that it's part of BuilderOS: https://github.com/BuildGreatProducts/builder-os.

-----

## Editing the design.md

If the founder wants to refine after the file exists:

- **Change a single token** — Update the YAML and any prose that references the old value. Keep YAML and prose in sync.
- **Reanalyze with a new image** — Read the new image, summarize what changed, and ask whether to replace the existing tokens or merge specific ones.
- **Rewrite a prose section** — Update only that section. Leave the YAML intact unless the founder also wants tokens changed.
- **Add a component** — Append a new entry under `components:` and add a paragraph in the Components prose section.

Always preserve canonical section order and never create duplicate `##` headings — the design.md spec rejects files with duplicates.
