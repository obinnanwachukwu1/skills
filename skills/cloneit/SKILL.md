---
name: cloneit
description: Actively inspect and clone a live website or web app by using Computer Use to navigate, click, scroll, open menus, switch tabs, test states, capture structure/styles/interactions, and then recreate it locally in the current workspace. Use when Codex needs to closely reproduce a real product surface from a URL or visual reference, especially for interactive apps, dashboards, SaaS products, onboarding flows, or marketing pages where static screenshots are not enough.
---

# Cloneit

Rebuild the target from evidence, not memory. Use Computer Use to operate the real interface, write findings into workspace artifacts, extract exact styles when needed, and only then implement the local clone.

If the repo already has a frontend stack, stay inside it. If the repo is empty, create the lightest viable local app for the requested fidelity.

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

### 3. Run a mandatory interaction pass before coding

Do not stop after reading the initial page. Use Computer Use to actively operate the interface.

At minimum, perform the relevant subset of these actions:

- Scroll from top to bottom and back through the main surface.
- Click each primary nav item, tab, or mode switch once.
- Open at least one representative menu, popover, drawer, or modal in each major area.
- Trigger hover, focus, selected, active, and disabled states when they exist.
- Exercise one representative create, edit, or filter flow when it is safe to do so.
- Inspect empty, loading, success, and error states if accessible.
- Check desktop and mobile widths when responsive behavior matters.

If a target area is inaccessible, blocked, or unsafe to exercise, record that explicitly in `notes/source.md` and `notes/interactions.md`.

### 4. Capture before implementing

Use Computer Use with a browser to inspect:

- Initial load state
- Global navigation
- Major screens or sections
- Menus, dialogs, drawers, tabs, filters, and forms
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
- `notes/rebuild-plan.md`: target stack, file plan, milestones, and open questions
- `notes/verification.md`: source vs local differences, severity, and next fixes

Save screenshot or visual evidence into `screenshots/` with descriptive names for each important state when the environment allows it.

### 6. Use direct inspection for exact values

Computer Use is for operation and discovery. Exact reconstruction usually requires direct inspection.

Use browser devtools when exact styles matter. Capture exact values when possible:

- Font family, size, weight, line height, and letter spacing
- Text and background colors
- Gaps, padding, margins, widths, heights, and max widths
- Border, radius, outline, and shadow
- Grid and flex rules
- Animation duration, easing, delay, and transform behavior

If a value is inferred instead of observed, mark it as `inferred` in `notes/styles.md`.

### 7. Use terminal-based fallbacks when the browser view is not enough

If Computer Use cannot reliably expose a detail, fetch supporting evidence from the terminal.

Allowed fallback sources:

- Page HTML via `curl`
- Referenced CSS files
- Static assets such as fonts, logos, and images
- Public page metadata such as title, description, and Open Graph assets

Use these fallbacks to supplement the visual pass, not replace it. Do not reduce the workflow to "take screenshots and guess".

If screenshot capture fails in the environment, keep going and record the blocker. Use a combination of Computer Use observations, HTML/CSS fetches, and written notes instead.

### 8. Record interactions as requirements

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

Treat hover states, open/close states, sticky behavior, carousels, tables, filters, forms, and responsive menus as part of the clone, not polish.

### 9. Build from the inventory

Implement in this order:

1. App shell and layout primitives
2. Global design tokens
3. Reusable components
4. Screen or section composition
5. Interactive states and motion
6. Responsive adjustments
7. Asset replacement or extraction

Do not aim for pixel perfection before the structural pass is complete. Get hierarchy and spacing right first, then tighten styles, then interactions.

### 10. Verify against the source

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
- Interact with the product. Do not stop at the first visible state.
- If the target is an app, enter the app and exercise its core surfaces.
- Keep a written record of every non-trivial finding in the workspace.
- Prefer exact observed values over approximation.
- Use terminal fallbacks for HTML, CSS, and assets when Computer Use alone is insufficient.
- Treat interactions as first-class requirements.
- Preserve the target information architecture even if the implementation stack changes.
- Reuse components when multiple screens or sections share the same pattern.
- If a detail is uncertain, mark it as inferred instead of pretending it is exact.
- If a state cannot be safely exercised, record the blocker explicitly.
- Note legal or ethical boundaries if the user asks to copy proprietary assets or code verbatim.

## Example Triggers

- "Clone this marketing page into the current React app."
- "Open this SaaS app, click through the main screens, and rebuild it locally."
- "Use Computer Use to inspect this dashboard and clone the actual product surface, not just the homepage."
- "Study this onboarding flow, capture each step and state, and recreate it in Next.js."
