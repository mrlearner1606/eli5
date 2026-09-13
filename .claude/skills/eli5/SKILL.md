---
name: eli5
description: Explain a topic like I'm a 5 year old. Use when the user types /eli5 <topic> or asks for a dead-simple explainer of how something works. Creates a themed HTML explainer page, adds it to the index.html landing page, and commits + pushes everything to the GitHub repo.
---

# eli5

Explain like I'm someone who knows nothing about this topic, using an HTML page with big ideas, vivid analogies, and few words.

Topic: $ARGUMENTS

## What to produce

A single, self-contained HTML file (no external dependencies — inline CSS/JS, works offline and on GitHub Pages).

**Location:** put it in a topic folder that matches the subject, e.g. `AWS/Gen AI/opensearch-vs-serverless.html`, `Kubernetes/pods-vs-deployments.html`. Create the folder if it doesn't exist.

**Content requirements:**
1. Lead with a one-line ELI5 framing (e.g. "like renting a kitchen vs hailing a ride")
2. Build from "why does this exist" → concrete analogy → side-by-side comparison
3. Include a comparison table if the topic compares things, and a "gotchas / fine print" section
4. End with a short TL;DR list
5. Facts get verified online in step 2 of the `all-in-one` pipeline — write confidently about stable concepts, and keep time-sensitive claims (defaults, limits, pricing) hedged unless verified

**Style requirements (match the existing pages):**
- Dark theme with the palette defined in `index.html` (`--bg #0b0f14`, `--panel #131b26`, `--border #223040`, orange `#ff9900` accents, cyan `#37c8e8` secondary, 16px radius cards)
- Sticky top nav with section anchors, scroll-reveal animations, `prefers-reduced-motion` support
- Same fonts, badge/tag styling, and hover polish as `AWS/Gen AI/opensearch-vs-serverless.html`
- Responsive down to mobile

## Required workflow — do all three steps

### 1. Create the explainer file
Write the HTML into the appropriate topic folder as described above.

### 2. Update `index.html` (required on every new topic)
Every new explainer MUST be linked from the landing page or it will never appear on GitHub Pages:
- Add a `<a class="card" href="<relative-path-URL-encoded>">…</a>` card inside the matching `.group` section (create the section, with a collapsible `.group-title.toggle` header, if the topic introduces a new top-level category — the script wires up collapse, counts, and search automatically)
- Card must include: an emoji icon, `<h2>` title, and a `<span class="card-path">` with the repo path (URL-encode spaces in `href`, e.g. `AWS/Gen%20AI/...`)
- Check the result in the preview before pushing

### 3. Merge to the GitHub repo (required on every change)
Commit and push so GitHub Pages picks it up:
```bash
git add <new-explainer-file> index.html
git commit -m "Add ELI5 explainer: <topic>"
git push origin main
```
Stage only files belonging to this request (leave unrelated user changes alone). Never force-push, reset, or rewrite history.

## Notes

- GitHub Pages is served from `main` / root, so anything pushed is live at `https://mrlearner1606.github.io/eli5/` within a minute or two.
- Keep each explainer standalone; never make it depend on `index.html`.
