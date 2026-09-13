---
name: all-in-one
description: End-to-end ELI5 explainer pipeline. Use when the user types /all-in-one <topic> or asks for a new explainer topic. Calls the eli5 skill to write the content, the frontend-design skill to apply the site theme, updates index.html with a card for the new page, and commits + pushes everything to the GitHub repo so GitHub Pages picks it up.
---

# all-in-one

Run the complete ELI5 explainer pipeline for one topic, from blank page to pushed commit.

Topic: $ARGUMENTS

## Pipeline — execute every step in order

### Step 1 — Write the content (call the `eli5` skill)
Follow `.claude/skills/eli5/SKILL.md` for the full content contract. In short:
- Produce a single self-contained HTML file (inline CSS/JS, zero external requests)
- Place it in a topic folder matching the subject, e.g. `AWS/Gen AI/opensearch-vs-serverless.html`
- Structure: ELI5 one-liner → why it exists → concrete analogy → side-by-side table → gotchas → TL;DR
- Verify facts against official docs when unsure

### Step 2 — Apply the design system (call the `frontend-design` skill)
Follow `.claude/skills/frontend-design/SKILL.md` so the new page matches the site:
- Dark palette (`#0b0f14` / `#131b26` panels, orange `#ff9900` + cyan `#37c8e8` accents, 16px radius)
- Sticky anchor nav, scroll-reveal with `prefers-reduced-motion` support, hover-lift cards
- Responsive to ~360px, visible focus states, semantic landmarks

### Step 3 — Update `index.html` (required — never skip)
A page that isn't linked never appears on GitHub Pages:
- Add `<a class="card" href="<URL-encoded-relative-path>">…</a>` inside the matching `.group` section
- Card contents: emoji icon tile, `<h2>` title with a topic `<span class="tag">`, `<span class="card-path">` with the repo path
- New top-level category? Add a new `.group` with a `.group-title.toggle` collapsible header — the script wires collapse, counts, and search automatically (no JS edits needed)
- URL-encode spaces in `href` (e.g. `AWS/Gen%20AI/…`)

### Step 4 — Verify
- Confirm markup balance (matching div/span tags), links resolve to real files, and the page renders correctly in the preview

### Step 5 — Merge to the GitHub repo (required — never skip)
```bash
git add <new-explainer-file> index.html
git commit -m "Add ELI5 explainer: <topic>"
git push origin main
```
- Stage only files belonging to this request; leave unrelated user changes untracked
- Never force-push, reset, or rewrite history
- Pages serves from `main` / root → live at `https://mrlearner1606.github.io/eli5/` within a minute or two of the push

## Output summary

Finish by reporting: the new file's path, the index card added, the commit hash, and the live URL once pushed.
