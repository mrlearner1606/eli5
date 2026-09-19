---
name: eli5
description: Explain a topic like I'm a 5 year old. Use when the user types /eli5 <topic> or asks for a dead-simple explainer of how something works. Creates a themed HTML explainer page, adds it to index.html, and uses a branch + PR + merge workflow with mandatory post-merge verification.
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
5. Facts get verified online in the pipeline — write confidently about stable concepts, and keep time-sensitive claims (defaults, limits, pricing) hedged unless verified

**Style requirements (match the existing pages):**
- Dark theme with the palette defined in `index.html` (`--bg #0b0f14`, `--panel #131b26`, `--border #223040`, orange `#ff9900` accents, cyan `#37c8e8` secondary, 16px radius cards)
- Sticky top nav with section anchors, scroll-reveal animations, `prefers-reduced-motion` support
- Same fonts, badge/tag styling, and hover polish as `AWS/Gen AI/opensearch-vs-serverless.html`
- Responsive down to mobile

## Required workflow — do all steps

### 1. Create the explainer file
Write the HTML into the appropriate topic folder as described above.

### 2. Update `index.html` (required on every new topic)
Every new explainer MUST be linked from the landing page:
- Add a `<a class="card" href="<relative-path-URL-encoded>">…</a>` card inside the matching `.group` section
- Card must include: an emoji icon, `<h2>` title, a `<span class="tag">`, and a `<span class="card-path">` with the repo path
- URL-encode spaces in `href`, e.g. `AWS/Gen%20AI/…`
- Update the existing index from the current branch contents; do not reconstruct or overwrite unrelated cards
- If adding a new top-level category, create the collapsible `.group` and rely on the existing JS for counts/search

### 3. Verify the branch before PR
After writing:
- Re-fetch the new HTML file from the working branch
- Re-fetch `index.html` from the working branch
- Confirm the exact `href` and visible title are present
- Confirm the target file exists in the same branch
- Do not treat a write API response alone as proof that the index is correctly linked

### 4. Branch + PR + merge
For normal ELI5 repo changes:
- Create a dedicated branch from `main`
- Commit the explainer + index changes on that branch
- Open a PR targeting `main`
- Merge the PR (squash preferred for small changes)
- Never force-push, reset, or rewrite history

### 5. REQUIRED post-merge verification
After the merge:
- Re-fetch `index.html` from **`main`**
- Re-fetch the explainer file from **`main`**
- Verify the exact card `href` and title are present in the main-branch index
- Verify the target file exists on main
- Verify the PR is actually merged
- When possible, fetch the public GitHub Pages URL and verify the rendered page
- If live Pages cannot be fetched or may still be deploying/cached, state that limitation instead of claiming it is live

### 6. Reporting
Only report success after the post-merge checks. Include:
- file path
- exact index card title + href
- PR and merge status
- merge commit hash
- main-branch verification
- live Pages verification only when actually performed

## Notes

- GitHub Pages is served from `main` / root, but deployment/cache can lag behind the Git branch.
- Keep each explainer standalone; never make it depend on `index.html`.
