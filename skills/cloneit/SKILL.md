---
name: cloneit
description: Actively inspect and clone a live website or web app by using Computer Use to navigate, click, scroll, switch tabs, open menus, review auth and onboarding surfaces, inspect computed styles, capture evidence, and then recreate it locally in the current workspace. Use when Codex needs to closely reproduce a real product surface from a URL or visual reference, especially for interactive apps, dashboards, SaaS products, onboarding flows, login flows, or marketing pages where static screenshots are not enough.
---

# Cloneit

Rebuild the target from evidence, not memory. Operate the real interface, collect verifiable evidence, extract exact styles, and only then implement the local clone.

Spacing, padding, layout rhythm, typography, macro-composition, and exact CSS treatment are first-class requirements. If those are off, the clone is not done.

If the repo already has a frontend stack, stay inside it. If nothing is specified and there is no existing stack, default to React + Vite + Tailwind. Use Next.js + Tailwind when the target clearly benefits from app routing, server rendering, or content-heavy page structure. Do not default to plain HTML/CSS unless the user explicitly asks for it. If the cloned behavior implies persistence, user data, or server-side workflows, create the lightest backend needed to support those behaviors.

## Workflow

### 1. Create a run folder

Before inspecting the target, create a local run folder and keep findings there instead of in chat.

Use this layout:

```text
artifacts/cloneit/<site-slug-or-task-name>/
  screenshots/
  notes/
    source.md
    evidence.md
    structure.md
    styles.md
    interactions.md
    components.md
    assets.md
    rebuild-plan.md
    inspection-summary.md
    verification.md
```

### 2. Classify the job before inspecting

Record the target surface in `notes/source.md`:

- Marketing homepage
- Logged-in app shell
- Specific dashboard or project page
- Settings or onboarding flow
- Modal, drawer, menu, or wizard flow
- A small set of representative app screens

Also classify clone depth:

- Visual structure only
- Visual plus interaction fidelity
- Full workflow clone

If the user says "clone the app", prioritize the product surface over the marketing site. Enter the app and inspect the actual working interface.

If the user wants "this website, but for my app/product/company", write a short adaptation brief in `notes/rebuild-plan.md` before implementing.

Include:

- Source design cues to preserve
- Product content and behavior that must change
- Components that transfer directly
- Components that keep the same layout language but need content substitution
- Components that need redesign because the new app has different behavior

Adaptation does not reduce the inspection requirement. The goal is "the same design system and page structure, built for the new product".

### 3. Build evidence before code

Do not implement until the applicable inspection gates are complete.

Hard gates for every task:

- Full-page scroll completed on every in-scope surface
- At least one representative click-through in each major nav area
- At least one existing populated state inspected when populated states exist
- At least one menu, modal, drawer, or popover captured where present
- Desktop and mobile pass completed when responsiveness matters

Additional minimums by UI type:

- Marketing page:
  - Scroll to footer
  - Inspect header, hero, primary CTA, repeated sections, pricing or proof blocks, and footer
- App shell:
  - Open at least one existing item from each visible collection that matters
  - Inspect one menu or command surface
  - Inspect one detail view
- Chat, dashboard, or data-heavy app:
  - Inspect one populated state
  - Inspect one empty or sparse state if accessible
  - Inspect one action flow such as filter, create, assign, edit, or send

If a target area is inaccessible, blocked, or unsafe to exercise, record that explicitly. Missing evidence is not permission to guess.

### 4. Maintain an evidence ledger

For every major claim that will drive implementation, add an entry to `notes/evidence.md`.

Use this shape:

```markdown
## Hero headline width
- Surface:
- Action taken:
- Observed result:
- Evidence file:
- Status: observed | inferred
```

The evidence ledger is the source of truth. Notes may summarize, but implementation should be based on evidence.

### 5. Inspect the interface actively

Do not stop after reading the initial page. Use Computer Use to operate the interface.

At minimum, perform the relevant subset of these actions:

- Scroll from top to bottom and back through the main surface
- Click each primary nav item, tab, segmented control, and mode switch once
- Open at least one representative menu, popover, drawer, or modal in each major area
- Trigger hover, focus, selected, active, and disabled states when they exist
- Exercise one representative create, edit, assign, or filter flow when it is safe to do so
- Review login, signup, onboarding, and logged-out/logged-in entry points when they are in scope and safe to inspect
- Inspect empty, loading, success, and error states if accessible
- Check desktop and mobile widths when responsive behavior matters

### 6. Capture notes and unknowns

Write findings into these files as you discover them:

- `notes/source.md`: URL, capture date, browser/app, auth requirements, target surface, clone depth, blockers
- `notes/structure.md`: screen map, section order, layout, hierarchy, repeated patterns
- `notes/styles.md`: colors, type scale, spacing, borders, shadows, radii, breakpoints
- `notes/interactions.md`: trigger, precondition, result, animation, and evidence for each interaction
- `notes/components.md`: reusable components, variants, props/content model, and shared styles
- `notes/assets.md`: logos, icons, imagery, videos, gradients, fonts, and external assets
- `notes/rebuild-plan.md`: target stack, file plan, adaptation brief, milestones, open questions
- `notes/verification.md`: source vs local differences, severity, and next fixes

Add an `Unknowns` section to `notes/source.md` before implementation:

- What was not accessible
- What is still inferred
- What risk that creates for fidelity

Save screenshot or visual evidence into `screenshots/` with descriptive names for each important state when the environment allows it.

### 7. Do a browser-first style inspection

Prefer the live browser over terminal fetching for exact styles.

Use browser devtools first. Inspect computed or authored styles for the most visible areas before lower-priority sections:

- Header
- Hero
- Primary CTA
- First product frame or first major content block

Capture exact values when possible:

- Font family, size, weight, line height, and letter spacing
- Text and background colors
- Gaps, padding, margins, widths, heights, and max widths
- Border, radius, outline, and shadow
- Grid and flex rules
- Animation duration, easing, delay, and transform behavior

For above-the-fold sections, capture exact numeric values when possible for:

- Nav height
- Hero container width
- Headline max width
- Headline font size and line height
- Hero-to-subcopy spacing
- CTA position and offset
- Primary mockup width and height
- Section top and bottom padding
- Distance between hero copy and mockup

If a value is inferred instead of observed, mark it as `inferred` in `notes/styles.md` and `notes/evidence.md`.

### 8. Use fallback CSS inspection only when needed

If devtools inspection is blocked or incomplete, use browser-native fallbacks before terminal fetching:

- Devtools Sources or Network panels for loaded CSS
- Browser console queries such as `getComputedStyle(...)`
- Rendered DOM inspection in the live browser

If that still is not enough, use terminal-based fallbacks to supplement the browser pass:

- Page HTML via `curl`
- Referenced CSS files and downloaded stylesheets
- Static assets such as fonts, logos, and images
- Public page metadata such as title, description, and Open Graph assets

If public, unauthenticated browser inspection still is not enough and the environment supports it, use Playwright or another browser automation path to inspect the rendered page. Do not use Playwright as the first choice for authenticated or sensitive flows when the live browser session already has the page loaded.

If stylesheets are public, inspect them for:

- CSS variables and design tokens
- Typography scales
- Spacing scales
- Breakpoints
- Motion values
- Shared component rules

If screenshot capture fails in the environment, keep going and record the blocker. Use a combination of Computer Use observations, live browser inspection, HTML/CSS fetches, and written notes instead.

### 9. Run a fidelity pass for small details

After the structural pass, do a second pass focused on the details that make clones feel authentic instead of generic.

Lock down the most visible 20% first: header, hero, primary CTA, and first product frame or first major content block.

Inspect and record:

- Exact text content, casing, punctuation, and line breaks
- Font family, fallback stack, size, weight, line height, and letter spacing
- Spacing, padding, gap, margin, and container width
- Border width, border color, radius, and divider opacity
- Background treatments, gradients, blur, noise, glow, and shadows
- Icon size, icon stroke or fill style, and icon alignment
- Button height, chip height, input height, and control density
- Alignment details such as baseline alignment, vertical centering, and text wrapping

If the source is restrained, keep the clone restrained. Do not add extra cards, wrappers, shadows, gradients, panels, or decorative elements just to fill space.

### 10. Stop if evidence is missing

Before implementing a major area, check whether you have enough evidence.

If you are missing evidence for:

- layout structure
- populated state
- interaction behavior
- exact styling for a highly visible area

then do not code that area yet. Continue inspection first.

### 11. Write an inspection summary before coding

Before implementation, update `notes/inspection-summary.md` with:

- In scope
- Inspected
- Not yet inspected
- Unknowns
- Ready to build: yes or no

If `Ready to build` is not clearly `yes`, continue inspection.

### 12. Build from the inventory

Implement in this order:

1. App shell and layout primitives
2. Global design tokens
3. Reusable components
4. Screen or section composition
5. Interactive states and motion
6. Responsive adjustments
7. Asset replacement or extraction

Do not aim for pixel perfection before the structural pass is complete. Get hierarchy and spacing right first, then tighten styles, then interactions, then finish with the small-detail fidelity pass.

In adaptation mode, change the content and product-specific behavior while keeping the original spacing, layout language, typography rhythm, component density, page flow, and macro-composition as intact as possible.

### 13. Verify against the source

Run the local app and compare it to the source. Revisit the source with Computer Use for mismatches that matter.

Verify:

- Above-the-fold layout
- Navigation and state changes
- Representative clicked flows
- Scroll behavior
- Responsive layout
- Visual rhythm, spacing, and text ramps

Do a viewport-matched comparison before finalizing:

- Use the same viewport width for source and clone
- Compare the same scroll position
- Check header height, headline wrap, CTA placement, mockup scale, and negative space
- Fix silhouette-level mismatches before polishing deeper sections

Update `notes/verification.md` with:

- What now matches
- What still differs
- Whether the difference is intentional, inferred, or blocked

## File Templates

### `notes/source.md`

```markdown
# Source

- URL:
- Captured:
- Browser/app:
- Auth requirements:
- Target surface:
- Clone depth:
- Tested states:
- Blocked areas:

## Unknowns
- Not accessible:
- Still inferred:
- Fidelity risk:
```

### `notes/evidence.md`

```markdown
# Evidence

## Claim
- Surface:
- Action taken:
- Observed result:
- Evidence file:
- Status: observed | inferred
```

### `notes/structure.md`

```markdown
# Structure

- Screen map or page outline:
- Layout primitives:
- Reusable components:
- Responsive notes:
```

### `notes/styles.md`

```markdown
# Styles

## Tokens
- Colors:
- Typography:
- Spacing:
- Radius:
- Shadows:
- Breakpoints:

## Sections or screens
### Header
- Observed:
- Inferred:
```

### `notes/interactions.md`

```markdown
# Interactions

## Mobile menu
- Trigger:
- Preconditions:
- Result:
- Animation:
- Evidence:
- Notes:
```

### `notes/inspection-summary.md`

```markdown
# Inspection Summary

- In scope:
- Inspected:
- Not yet inspected:
- Unknowns:
- Ready to build: yes | no
```

### `notes/rebuild-plan.md`

```markdown
# Rebuild Plan

- Target stack:
- Files to create/update:
- Milestones:
- Open questions:

## Adaptation Brief
- Preserve:
- Change:
- Direct transfers:
- Content substitutions:
- Redesigns:
```

### `notes/verification.md`

```markdown
# Verification

- Compared states:
- Matching areas:
- Differences:
- Severity:
- Next fixes:
```

## Rules

- No code before evidence. If the applicable gates are not complete, continue inspection.
- Interact with the product. Do not stop at the first visible state.
- Maintain an evidence ledger and label details as observed or inferred.
- Use live browser inspection and devtools first for CSS and computed styles.
- Use terminal fetches only as supporting evidence, not the primary source of truth for rendered styling.
- Use Playwright only as a later fallback for public, unauthenticated inspection when the live browser path is insufficient.
- Put spacing, padding, gaps, sizing, alignment, typography, and exact CSS treatment at the forefront of the work.
- Preserve macro-composition, especially above the fold: header height, headline width and wrap, CTA placement, mockup scale, alignment, and negative space.
- Treat interactions as first-class requirements and record blockers explicitly when a state cannot be safely exercised.
- Be true to the source design. Do not add generic filler UI such as extra cards, wrappers, or decorative blocks that are not present in the original.
- In adaptation mode, keep the source's spacing, structure, page flow, and interaction language while swapping in the new product's story and behavior.
- Reuse components when patterns repeat, mark uncertain details as inferred, and note legal or ethical boundaries when relevant.
- Default to React + Vite + Tailwind when no stack is specified and no existing app is present. Use Next.js + Tailwind when the target clearly calls for it.

## Example Triggers

- "Clone this marketing page into the current React app."
- "Open this SaaS app, click through the main screens, and rebuild it locally."
- "Use Computer Use to inspect this dashboard and clone the actual product surface, not just the homepage."
- "Study this onboarding flow, capture each step and state, and recreate it in Next.js."
- "Make my app's marketing site as if Linear's website had been designed for my product."
