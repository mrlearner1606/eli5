# eli5

## all-in-one

```yaml
name: all-in-one
description: End-to-end ELI5 explainer pipeline. Use when the user types /all-in-one <topic> or asks for a new explainer topic. Calls the eli5 skill to write the content, the frontend-design skill to apply the site theme, updates index.html with a card for the new page, and commits + pushes everything to the GitHub repo so GitHub Pages picks it up.
```

Run the complete ELI5 explainer pipeline for one topic, from blank page to pushed commit.

**Topic:** `$ARGUMENTS`

### Pipeline — execute every step in order

**Step 1 — Write the content (call the `eli5` skill)**

Follow `.claude/skills/eli5/SKILL.md` for the full content contract. In short:
- Produce a single self-contained HTML file (inline CSS/JS, zero external requests).
- Place it in a topic folder matching the subject, e.g. `AWS/Gen AI/opensearch-vs-serverless.html`.
- Structure: ELI5 one-liner → why it exists → concrete analogy → side-by-side table → gotchas → TL;DR.

**Step 2 — Verify facts online (required — never skip)**

Built-in knowledge can be stale; service names, defaults, limits, and pricing drift. Before finalizing the draft:
- Use web search against official sources first (for example `docs.aws.amazon.com`, AWS News Blog, service FAQs/what's-new posts), then reputable secondary sources.
- Verify anything time-sensitive: product names & tiers, default values, pricing shape, GA vs preview status, and any “newer option” claims.
- If a fact can't be verified, soften it (“historically…” / “check current docs”) rather than stating it as fixed.
- Never cite unverified numbers with false precision.

**Step 3 — Apply the design system (call the `frontend-design` skill)**

Follow `.claude/skills/frontend-design/SKILL.md` so the new page matches the site:
- Dark palette (`#0b0f14` / `#131b26` panels, orange `#ff9900` + cyan `#37c8e8` accents, 16px radius).
- Sticky anchor nav, scroll-reveal with `prefers-reduced-motion` support, hover-lift cards.
- Responsive to ~360px, visible focus states, semantic landmarks.

**Step 4 — Update `index.html` (required — never skip)**

A page that isn't linked never appears on GitHub Pages:
- Add `<a class="card" href="<URL-encoded-relative-path>">…</a>` inside the matching `.group` section.
- Card contents: emoji icon tile, `<h2>` title with a topic `<span class="tag">`, `<span class="card-path">` with the repo path.
- For a new top-level category, add a new collapsible `.group-title.toggle` section; existing JavaScript wires collapse, counts, and search automatically.
- URL-encode spaces in `href` (for example `AWS/Gen%20AI/…`).

**Step 5 — Verify**

- Confirm markup balance (matching `div`/`span` tags).
- Confirm links resolve to real files.
- Confirm the page renders correctly in preview.

**Step 6 — Merge to the GitHub repo (required — never skip)**

```bash
git add <new-explainer-file> index.html
git commit -m "Add ELI5 explainer: <topic>"
git push origin main
```

- Stage only files belonging to this request; leave unrelated user changes alone.
- Never force-push, reset, or rewrite history.
- GitHub Pages serves from `main` / root at `https://mrlearner1606.github.io/eli5/`.

### Output summary

Finish by reporting: the new file's path, the index card added, the commit hash, and the live URL once pushed.

---

## eli5

```yaml
name: eli5
description: Explain a topic like I'm a 5 year old. Use when the user types /eli5 <topic> or asks for a dead-simple explainer of how something works. Creates a themed HTML explainer page, adds it to the index.html landing page, and commits + pushes everything to the GitHub repo.
```

Explain like I'm someone who knows nothing about this topic, using an HTML page with big ideas, vivid analogies, and few words.

**Topic:** `$ARGUMENTS`

### What to produce

A single, self-contained HTML file (no external dependencies — inline CSS/JS, works offline and on GitHub Pages).

**Location:** put it in a topic folder that matches the subject, e.g. `AWS/Gen AI/opensearch-vs-serverless.html`, `Kubernetes/pods-vs-deployments.html`. Create the folder if it doesn't exist.

**Content requirements:**
1. Lead with a one-line ELI5 framing.
2. Build from “why does this exist” → concrete analogy → side-by-side comparison.
3. Include a comparison table if the topic compares things, and a “gotchas / fine print” section.
4. End with a short TL;DR list.
5. Verify facts online in the `all-in-one` pipeline; write confidently about stable concepts and hedge time-sensitive claims unless verified.

**Style requirements:**
- Dark theme with palette defined in `index.html`: `--bg #0b0f14`, `--panel #131b26`, `--border #223040`, `--orange #ff9900`, `--cyan #37c8e8`, `--radius 16px`.
- Sticky top nav with section anchors, scroll-reveal animations, `prefers-reduced-motion` support.
- Same fonts, badge/tag styling, and hover polish as the existing explainer pages.
- Responsive down to mobile.

### Required workflow

**1. Create the explainer file**

Write the HTML into the appropriate topic folder.

**2. Update `index.html` (required on every new topic)**

Every new explainer MUST be linked from the landing page or it will never appear on GitHub Pages:
- Add an `<a class="card" href="<relative-path-URL-encoded>">…</a>` card inside the matching `.group` section.
- Create a collapsible `.group-title.toggle` section if the topic introduces a new top-level category.
- Card must include an emoji icon, `<h2>` title, and `<span class="card-path">` with the repo path.
- Check the result in preview before pushing.

**3. Merge to the GitHub repo (required on every change)**

```bash
git add <new-explainer-file> index.html
git commit -m "Add ELI5 explainer: <topic>"
git push origin main
```

- Stage only files belonging to this request.
- Never force-push, reset, or rewrite history.

### Notes

- GitHub Pages is served from `main` / root at `https://mrlearner1606.github.io/eli5/`.
- Keep each explainer standalone; never make it depend on `index.html`.

---

## frontend-design

```yaml
name: frontend-design
description: Apply the ELI5 repo's visual design system when creating or editing any HTML page. Use for theming, layout, motion, and responsive polish so new pages match the existing explainer site.
```

Apply this repo's design system so every page looks like part of one site.

### Theme (dark, AWS-inspired)

- Backgrounds: `--bg #0b0f14`, `--bg-soft #10161f`, `--panel #131b26`, `--panel-2 #18212e`.
- Borders: `--border #223040`; text: `--text #e8eef4`, `--muted #98a6b5`, `--faint #6b7a89`.
- Accents: orange `--orange #ff9900`, cyan `--cyan #37c8e8`.
- Radius: `--radius 16px`.
- Cards use subtle `linear-gradient(180deg, panel → bg-soft)`.
- Body background uses layered radial gradients over `--bg`.
- Font stack: `ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, …`.
- Headline gradient: orange → cyan via `background-clip: text`.

### Layout & components

- Max-width container (`880px` for index, `1080px` for explainers), generous vertical rhythm.
- Cards: flex rows with emoji icon tile, title, path/description, hover lift (`translateY(-2px)`) and orange border.
- Section headers: uppercase, letter-spaced, cyan, with trailing hairline (`::after`).
- Tags/badges: pill-shaped orange or cyan soft backgrounds.
- Tables: dark panels, uppercase headers, hairline row dividers.
- Sticky top nav with anchor links and backdrop blur.

### Motion & accessibility

- Scroll-reveal: `.reveal` + IntersectionObserver, disabled under `prefers-reduced-motion`.
- Transitions: `0.15s ease` on borders/transform/color only.
- Visible `:focus-visible` outlines (cyan), semantic landmarks, `aria-expanded` on collapsible headers, `aria-live` for dynamic result boxes.

### Hard rules

- Self-contained files only: inline CSS/JS, zero external requests (works offline + GitHub Pages).
- Responsive down to ~360px; stack grids and reduce padding under 560px.
- Vanilla CSS only; no external frameworks.
