---
name: all-in-one
description: End-to-end ELI5 explainer pipeline. Use when the user types /all-in-one <topic> or asks for a new explainer topic. Calls the eli5 skill to write the content, the frontend-design skill to apply the site theme, updates index.html with a card for the new page, then pushes the change directly to main and performs post-push verification before reporting success.
---

# all-in-one

Run the complete ELI5 explainer pipeline for one topic, from blank page to verified push on `main`.

Topic: $ARGUMENTS

## Pipeline — execute every step in order

### Step 1 — Write the content (call the `eli5` skill)
Follow `.claude/skills/eli5/SKILL.md` for the full content contract. In short:
- Produce a single self-contained HTML file (inline CSS/JS, zero external requests)
- Place it in a topic folder matching the subject, e.g. `AWS/Gen AI/opensearch-vs-serverless.html`
- Structure: ELI5 one-liner → why it exists → concrete analogy → side-by-side table → gotchas → TL;DR

### Step 2 — Verify facts online (required — never skip)
Built-in knowledge can be stale; service names, defaults, limits, and pricing drift. Before finalizing the draft:
- Use web search against **official sources first** (e.g. `docs.aws.amazon.com`, AWS News Blog, service FAQs/what's-new posts), then reputable secondary sources
- Verify anything time-sensitive: product names & tiers, default values (chunk sizes, timeouts, token limits), pricing shape (per-what billing, minimums), GA vs preview status, and any "newer option" claims
- If a fact can't be verified, soften it ("historically…", "check current docs") rather than stating it as fixed
- Never cite unverified numbers with false precision

### Step 3 — Apply the design system (call the `frontend-design` skill)
Follow `.claude/skills/frontend-design/SKILL.md` so the new page matches the site:
- Dark palette (`#0b0f14` / `#131b26` panels, orange `#ff9900` + cyan `#37c8e8` accents, 16px radius)
- Sticky anchor nav, scroll-reveal with `prefers-reduced-motion` support, hover-lift cards
- Responsive to ~360px, visible focus states, semantic landmarks

### Step 4 — Update `index.html` (required — never skip)
A page that isn't linked never appears on GitHub Pages:
- Add `<a class="card" href="<URL-encoded-relative-path>">…</a>` inside the matching `.group` section
- Card contents: emoji icon tile, `<h2>` title with a topic `<span class="tag">`, `<span class="card-path">` with the repo path
- New top-level category? Add a new `.group` with a `.group-title.toggle` collapsible header — the script wires collapse, counts, and search automatically (no JS edits needed)
- URL-encode spaces in `href` (e.g. `AWS/Gen%20AI/…`)
- Prefer updating the existing index content from the current `main` rather than rebuilding `index.html` from memory

### Step 5 — Verify BEFORE pushing
- Re-read the **actual files** after writes
- Confirm the new HTML path exists
- Confirm `index.html` contains the exact target `href` and visible card title
- Check basic markup integrity (matching major containers / obvious malformed HTML)
- Do not report the page as "linked" based only on the write response; re-read the resulting content

### Step 6 — Push directly to `main` (required)
Commit and push straight to `main`. No feature branch and no pull request.
1. Make sure the local checkout is up to date with `origin/main`
2. Stage only the explainer and `index.html` changes for this request; leave unrelated changes alone
3. Commit with a clear message describing the new explainer and the index link
4. Push to `main`
5. Record the resulting `main` commit SHA

- Never force-push, reset, or rewrite history on `main`
- If the push is rejected because `main` moved, fetch and rebase onto the new `main`, then push again. Never force

### Step 7 — REQUIRED POST-PUSH VERIFICATION
This step exists specifically to prevent "the push says it worked, but the page doesn't show it" mistakes.
- Re-fetch `index.html` **from `origin/main`**
- Re-fetch the new explainer **from `origin/main`**
- Confirm the exact card `href` and visible title are present on `main`
- Confirm the linked target exists on `main`
- Confirm the `main` head SHA matches the commit that was pushed
- If a public GitHub Pages URL is known and fetchable, verify the **rendered/live page** too
- If the live page cannot be fetched because of cache/deployment limitations, say so explicitly; do **not** claim live-page verification
- Only after these checks may you report the task as complete

## Output summary

Finish by reporting:
- new file path
- exact index card title + href
- commit SHA pushed to `main`
- main-branch verification result
- live URL only when actually verified; otherwise say `main` is verified but live Pages rendering could not be directly verified
