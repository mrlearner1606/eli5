---
name: frontend-design
description: Apply the ELI5 repo's visual design system when creating or editing any HTML page. Use for theming, layout, motion, and responsive polish so new pages match the existing explainer site.
---

# frontend-design

Apply this repo's design system so every page looks like part of one site.

## Theme (dark, AWS-inspired)

- Palette (defined identically in `index.html` and every explainer):
  - Backgrounds: `--bg #0b0f14`, `--bg-soft #10161f`, `--panel #131b26`, `--panel-2 #18212e`
  - Borders: `--border #223040`; Text: `--text #e8eef4`, `--muted #98a6b5`, `--faint #6b7a89`
  - Accents: orange `--orange #ff9900` (primary), cyan `--cyan #37c8e8` (secondary)
  - Radius: `--radius 16px`; cards use subtle `linear-gradient(180deg, panel → bg-soft)`
- Body background: layered radial gradients (orange top-right, cyan top-left) over `--bg`
- Font stack: `ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, …`
- Gradient headline text via `background-clip: text` (orange → cyan)

## Layout & components

- Max-width container (`880px` for index, `1080px` for explainers), generous vertical rhythm
- Cards: flex rows with emoji icon tile, title, path/description, hover lift (`translateY(-2px)` + orange border)
- Section headers: uppercase, letter-spaced, cyan, with trailing hairline (`::after`)
- Tags/badges: pill-shaped, orange or cyan soft backgrounds
- Tables: dark panels, uppercase headers, hairline row dividers
- Sticky top nav with anchor links and backdrop blur

## Motion & accessibility

- Scroll-reveal: `.reveal` + IntersectionObserver, disabled under `prefers-reduced-motion`
- Transitions: 0.15s ease on borders/transform/color only — no gratuitous animation
- Visible `:focus-visible` outlines (cyan), semantic landmarks, `aria-expanded` on collapsible headers, `aria-live` for dynamic result boxes

## Hard rules

- Self-contained files only: inline CSS/JS, zero external requests (works offline + GitHub Pages)
- Responsive down to ~360px (stack grids, reduce padding under 560px)
- Vanilla CSS only; no external frameworks
