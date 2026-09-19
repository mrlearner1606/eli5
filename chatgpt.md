# ChatGPT ELI5 GitHub Workflow

## Purpose

This file is the source of truth for how ChatGPT should handle requests to create new ELI5 explainers in this repository.

The repository is:

- **Owner:** `mrlearner1606`
- **Repo:** `eli5`
- **Default branch:** `main`
- **Landing page:** `index.html`

## Trigger

When the user says:

> **explain <<topic>> check github repo chatgpt.md file**

ChatGPT should treat this as a complete instruction to perform the workflow below.

The user should **not** need to repeat these instructions for every topic.

## Required behavior

For every triggered request:

1. Read this `chatgpt.md` file from the current `main` branch before doing the work.
2. Determine an appropriate HTML filename and location based on the topic.
3. Create a single, self-contained HTML explainer using the repository's ELI5 style.
4. Explain the topic in simple English using the ELI5 method:
   - one-line ELI5 explanation
   - why it exists
   - concrete analogy
   - how it works
   - important concepts / features
   - comparison table when useful
   - gotchas / exam traps / fine print when relevant
   - concise TL;DR
5. For technical, cloud, AWS, software, or other current topics, verify important facts against current official documentation or authoritative sources before finalizing.
6. Use the repository's existing visual design:
   - dark background
   - orange and cyan accents
   - rounded cards
   - sticky navigation
   - responsive layout
   - reduced-motion support
   - accessible focus states
   - no external runtime dependencies
7. Put the HTML file in the most appropriate existing topic folder. Prefer an existing folder when one clearly matches.
8. Update `index.html` so the new HTML page appears in the appropriate group.
9. The index card must contain:
   - emoji/icon
   - visible topic title
   - topic tag
   - repository-relative path
   - URL-encoded spaces in the `href`
10. Do **not** overwrite unrelated index cards or content.
11. Use a dedicated Git branch for the change.
12. Verify the new HTML file and `index.html` on that branch before opening the PR.
13. Open a pull request targeting `main`.
14. Merge the PR into `main`, preferably using **squash merge** for a small explainer change.
15. After merging, re-fetch both the HTML file and `index.html` directly from **`main`**.
16. Confirm the exact index card title and exact `href` exist on `main`.
17. Confirm the target HTML file exists on `main`.
18. Confirm the pull request is actually merged.
19. When GitHub Pages can be directly fetched, verify the rendered page. If it cannot be fetched because of deployment/cache limitations, explicitly state that live rendering could not be verified.
20. Never claim that a change is complete merely because a GitHub write API returned success.

## File naming

Use readable lowercase filenames with hyphens.

Examples:

- `AWS/Gen AI/sagemaker-data-wrangler.html`
- `AWS/Gen AI/amazon-bedrock-knowledge-bases.html`
- `AWS/Cloud/lambda-concurrency.html`
- `Kubernetes/pods-vs-deployments.html`

Keep the filename descriptive and stable.

## Choosing the index group

When `index.html` already has a matching category/group:

- Add the new card there.

When no appropriate group exists:

- Create a new collapsible group using the same structure and styling as the existing groups.
- Make sure the existing search/count JavaScript continues to work.

Do not create a duplicate group merely because the wording of the topic differs slightly from an existing group.

## GitHub workflow

Use this logical sequence:

```text
main
  ↓
create topic branch
  ↓
create HTML + update index.html
  ↓
verify branch contents
  ↓
open PR → main
  ↓
squash merge
  ↓
re-fetch main
  ↓
verify HTML + index link + PR status
```

Never force-push, reset, rewrite history, or delete unrelated user changes.

## Verification checklist

Before saying "done", verify all of these:

- [ ] `chatgpt.md` was read from current `main`
- [ ] HTML file exists on the working branch
- [ ] HTML file is self-contained
- [ ] HTML file follows ELI5 structure
- [ ] Important current facts were verified where necessary
- [ ] `index.html` was updated on the working branch
- [ ] Exact target `href` exists in `index.html`
- [ ] Visible card title matches the topic
- [ ] Pull request targets `main`
- [ ] Pull request was merged
- [ ] HTML file exists on `main`
- [ ] Exact target `href` exists in `main/index.html`
- [ ] Rendered GitHub Pages page verified when technically possible

## User interaction rule

The user's only required input for this workflow should be:

> `explain <<topic>> check github repo chatgpt.md file`

Do not ask the user to repeat the repository, branch, ELI5 format, HTML requirement, index update, PR/merge process, or verification steps when those instructions are already present in this file.

Ask a clarification only when the topic itself is genuinely ambiguous and choosing the wrong interpretation would materially change the page.

## Final response

After successful completion, report:

- HTML file path
- index card title and href
- PR number and merged status
- merge commit SHA
- confirmation that `main` was re-checked
- GitHub Pages URL only when its rendered page was actually verified

Keep the final report concise.
