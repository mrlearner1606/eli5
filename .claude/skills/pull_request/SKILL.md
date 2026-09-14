---
name: pull_request
description: Standalone GitHub flow — create a branch, make changes, push, open a pull request to main, and merge it. Use when the user types /pull_request <what to change> or asks to ship a change through a proper branch → PR → merge workflow. This skill is independent and does not call any other skill.
---

# pull_request

Ship a change to `main` the right way: dedicated branch → commit → push → pull request → merge.

Task: $ARGUMENTS

## Workflow — execute every step in order

### Step 1 — Sync and branch
Start from a clean, up-to-date `main`:
```bash
git status                      # must be clean or only unrelated user changes
git fetch origin
git pull --ff-only origin main
git checkout -b <branch-name>   # short, kebab-case, descriptive: e.g. add-pull-request-skill
```
- If the working tree has unrelated user changes, leave them untouched — never stage, commit, or discard files that don't belong to this task.
- Never branch off a stale or detached state.

### Step 2 — Make the changes
Implement exactly what was asked for the task in $ARGUMENTS:
- Keep the change focused; do not reformat, rename, or "improve" anything unrelated.
- Verify the result (typecheck/tests/preview render) before committing.

### Step 3 — Commit on the branch
```bash
git add <only-the-files-from-this-task>
git commit -m "<concise message describing the why>"
```
- Stage only files that belong to this request.
- Never force-push, reset, rebase, or rewrite history.

### Step 4 — Push and open the PR
```bash
git push -u origin <branch-name>
gh pr create --base main --head <branch-name> \
  --title "<short PR title>" \
  --body "What changed, why, and how it was verified."
```

### Step 5 — Merge the PR (required — never skip)
```bash
gh pr merge --squash --delete-branch
```
- Squash keeps `main` linear for content-site repos; use `--merge` instead if preserving individual commits matters.
- If merging is blocked (branch protection, failing checks), DO NOT force it — report the PR URL and what's blocking so the user can decide.

### Step 6 — Sync back to main
```bash
git checkout main
git pull --ff-only origin main
```
This guarantees local `main` matches what was just merged and GitHub Pages / CI picks it up.

## Output summary
Finish by reporting: the branch name, the commit hash, the PR number + URL, the merge state, and the final `main` commit hash.
