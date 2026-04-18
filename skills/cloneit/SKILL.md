---
name: cloneit
description: Actively inspect and clone a live website or web app by using Computer Use to navigate, click, scroll, switch tabs, open menus, review auth and onboarding surfaces, test states, capture structure/styles/interactions, and then recreate it locally in the current workspace. Use when Codex needs to closely reproduce a real product surface from a URL or visual reference, especially for interactive apps, dashboards, SaaS products, onboarding flows, login flows, or marketing pages where static screenshots are not enough.
---

# Cloneit

Rebuild the target from evidence, not memory. Use Computer Use to operate the real interface, write findings into workspace artifacts, extract exact styles when needed, and only then implement the local clone.

Spacing, padding, layout rhythm, typography, and exact CSS treatment are first-class requirements. In adaptation mode, preserve the source design language so the result feels like "this same website, but for my app", not a loose inspiration or generic redesign.

If the repo already has a frontend stack, stay inside it. If nothing is specified and there is no existing stack, default to React + Vite + Tailwind. Use Next.js + Tailwind when the target clearly benefits from app routing, server rendering, or content-heavy page structure. Do not default to plain HTML/CSS unless the user explicitly asks for it. If the cloned behavior implies persistence, user data, or server-side workflows, create the lightest backend needed to support those behaviors.

## Workflow

### 1. Create a run folder in the workspace

Before inspecting the target, create a local run folder and keep findings there instead of in chat.

Use this exact layout:

```text
artifacts/cloneit/<site-slug-or-task-name>/
  screenshots/
  notes/
    source.md
    structure.md
    styles.md
    interactions.md
    components.md
    assets.md
    rebuild-plan.md
    verification.md
```

If the folders or files do not exist, create them first.

### 2. Decide what surface is actually being cloned

Do not assume the public homepage is the target.

Determine which of these is in scope:

- Marketing homepage
- Logged-in app shell
- Specific dashboard or project page
- Settings or onboarding flow
- Modal, drawer, menu, or wizard flow
- A small set of representative app screens

If the user says "clone the app", prioritize the product surface over the marketing site. Enter the app and inspect the actual working interface.

### 2.5. Write an adaptation brief when content is changing

If the user wants "this website, but for my app/product/company", write a short adaptation brief in `notes/rebuild-plan.md` before implementing.

Include:

- Source design cues to preserve exactly or as closely as possible
- Product content and behavior that must change
- Components that transfer directly
- Components that keep the same layout language but need content substitution
- Components that need redesign because the new app has different behavior

Preserve as much of the following as possible:

- Spacing scale
- Padding and gap relationships
- Typography scale and tone
- Grid, alignment, and container widths
- Section rhythm and page flow
- Button, chip, input, and card density
- Navigation pattern and information hierarchy
- Motion language and interaction feel

Aim for "the same design system and page structure, built for the new product".

### 3. Run a mandatory interaction pass before coding

Do not stop after reading the initial page. Use Computer Use to actively operate the interface.

At minimum, perform the relevant subset of these actions:

- Scroll from top to bottom and back through the main surface.
- Click each primary nav item, tab, segmented control, and mode switch once.
- Open at least one representative menu, popover, drawer, or modal in each major area.
- Trigger hover, focus, selected, active, and disabled states when they exist.
- Exercise one representative create, edit, or filter flow when it is safe to do so.
- Review login, signup, onboarding, and logged-out/logged-in entry points when they are in scope and safe to inspect.
- Inspect empty, loading, success, and error states if accessible.
- Check desktop and mobile widths when responsive behavior matters.

If a target area is inaccessible, blocked, or unsafe to exercise, record that explicitly in `notes/source.md` and `notes/interactions.md`.

### 4. Capture before implementing

Use Computer Use with a browser to inspect:

- Initial load state
- Global navigation
- Major screens or sections
- Menus, dialogs, drawers, tabs, filters, and forms
- Login, signup, onboarding, and account-entry surfaces when relevant
- Scroll behavior and sticky regions
- Repeated UI patterns
- Responsive behavior at a minimum of desktop and mobile widths

Do not start coding until the main structure, major styles, and critical interactions are written down.

### 5. Persist findings into the notes files

Write findings into these files as you discover them:

- `notes/source.md`: URL, capture date, browser/app, auth requirements, blockers, tested surfaces
- `notes/structure.md`: screen map, section order, layout, hierarchy, repeated patterns
- `notes/styles.md`: colors, type scale, spacing, borders, shadows, radii, breakpoints
- `notes/interactions.md`: trigger, precondition, result, animation, and evidence for each interaction
- `notes/components.md`: reusable components, variants, props/content model, and shared styles
- `notes/assets.md`: logos, icons, imagery, videos, gradients, fonts, and external assets
- `notes/rebuild-plan.md`: target stack, file plan, milestones, open questions, and adaptation brief when relevant
- `notes/verification.md`: source vs local differences, severity, and next fixes

Save screenshot or visual evidence into `screenshots/` with descriptive names for each important state when the environment allows it.

### 6. Run a fidelity pass for small details

After the first structural pass, do a second pass focused on the details that make clones feel authentic instead of generic. Prioritize spacing and CSS fidelity above visual improvisation. If spacing, padding, line height, gaps, widths, alignment, or text treatment are off, the clone is not done.

Inspect and record:

- Exact text content, casing, punctuation, and line breaks
- Font family, fallback stack, size, weight, line height, and letter spacing
- Spacing, padding, gap, margin, and container width
- Border width, border color, radius, and divider opacity
- Background treatments, gradients, blur, noise, glow, and shadows
- Icon size, icon stroke/fill style, and icon alignment
- Button height, chip height, input height, and control density
- Alignment details such as baseline alignment, vertical centering, and text wrapping

If the source is restrained, keep the clone restrained. Do not add extra cards, wrappers, shadows, gradients, panels, or decorative elements just to fill space.

### 7. Use direct inspection for exact values

Computer Use is for operation and discovery. Exact reconstruction usually requires direct inspection.

Use browser devtools when exact styles matter. Capture exact values when possible:

- Font family, size, weight, line height, and letter spacing
- Text and background colors
- Gaps, padding, margins, widths, heights, and max widths
- Border, radius, outline, and shadow
- Grid and flex rules
- Animation duration, easing, delay, and transform behavior

If a value is inferred instead of observed, mark it as `inferred` in `notes/styles.md`.

### 8. Use terminal-based fallbacks when the browser view is not enough

If Computer Use cannot reliably expose a detail, fetch supporting evidence from the terminal.

Allowed fallback sources:

- Page HTML via `curl`
- Referenced CSS files and downloaded stylesheets
- Static assets such as fonts, logos, and images
- Public page metadata such as title, description, and Open Graph assets

Use these fallbacks to supplement the visual pass, not replace it.

If stylesheets are public, inspect them for:

- CSS variables and design tokens
- Typography scales
- Spacing scales
- Breakpoints
- Motion values
- Shared component rules

If screenshot capture fails in the environment, keep going and record the blocker. Use a combination of Computer Use observations, HTML/CSS fetches, and written notes instead.

### 9. Record interactions as requirements

For each meaningful interaction, write a block in `notes/interactions.md` using this shape:

```markdown
## Filter dropdown
- Trigger:
- Preconditions:
- Result:
- Animation:
- Evidence:
- Notes:
```

Treat hover states, open/close states, sticky behavior, tables, filters, forms, and responsive menus as part of the clone, not polish.

### 10. Build from the inventory

Implement in this order:

1. App shell and layout primitives
2. Global design tokens
3. Reusable components
4. Screen or section composition
5. Interactive states and motion
6. Responsive adjustments
7. Asset replacement or extraction

Do not aim for pixel perfection before the structural pass is complete. Get hierarchy and spacing right first, then tighten styles, then interactions, then finish with the small-detail fidelity pass. In adaptation mode, change the content and product-specific behavior while keeping the original spacing, layout language, typography rhythm, and component density as intact as possible.

### 11. Verify against the source

Run the local app and compare it to the source. Revisit the source with Computer Use for mismatches that matter.

Verify:

- Above-the-fold layout
- Navigation and state changes
- Representative clicked flows
- Scroll behavior
- Responsive layout
- Visual rhythm, spacing, and text ramps

Update `notes/verification.md` with:

- What now matches
- What still differs
- Whether the difference is intentional, inferred, or blocked

## File Templates

Use these starter templates when creating the notes.

### `notes/source.md`

```markdown
# Source

- URL:
- Captured:
- Browser/app:
- Auth requirements:
- Target surface:
- Tested states:
- Blocked areas:
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

### `notes/components.md`

```markdown
# Components

## Component name
- Variants:
- Props/content model:
- Shared styles:
- Used in:
```

### `notes/assets.md`

```markdown
# Assets

- Asset:
- Type:
- Source:
- Usage:
- Replacement plan:
```

### `notes/rebuild-plan.md`

```markdown
# Rebuild Plan

- Target stack:
- Files to create/update:
- Milestones:
- Open questions:
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

- Capture first, implement second.
- Interact with the product. Do not stop at the first visible state; click through tabs, menus, representative flows, and relevant auth/onboarding surfaces when safe.
- Put spacing, padding, gaps, sizing, alignment, typography, and exact CSS treatment at the forefront of the work.
- Use terminal fallbacks and inspect public stylesheets when Computer Use alone is insufficient.
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
