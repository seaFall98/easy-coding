---
name: finish
description: "Complete the development closure workflow for a feature branch. Use when the user says they are done, ready to wrap up, ready to open a PR, ready to merge, finish this feature, close out this branch, or similar."
---

# Finish

Complete the standard GitHub PR-based closure workflow for a feature branch.

## Prerequisites

Before starting, verify:

- The owner has confirmed manual acceptance.
- All intended changes are committed.
- The working tree is clean.
- The `gh` CLI is authenticated.
- The current branch is a feature branch, not `main` or `master`.

If any prerequisite fails, report what is missing and stop.

## Workflow

### 1. Identify branch and repo

```bash
git branch --show-current
git remote get-url origin
```

Stop if already on `main` or `master`.

### 2. Sync local main

```bash
git checkout main
git pull origin main
```

If pull fails because histories diverged, stop and ask the owner to resolve it. Do not force reset or rebase without explicit approval.

### 3. Push the feature branch

```bash
git checkout <feature-branch>
git push origin <feature-branch>
```

### 4. Build PR title and body

Use real branch commits and real verification data.

PR body should include:

- Summary
- Key changes
- Verification actually run
- Skipped verification and why
- Manual acceptance note

Never fabricate test results.

### 5. Create or report PR

```bash
gh pr create --base main --head <feature-branch> --title "<title>" --body "<body>"
```

If a PR already exists, report its URL instead of creating a duplicate.

### 6. Wait for owner merge

Tell the owner to review and merge on GitHub, then report back.

Do not merge via CLI unless the owner explicitly asks for it.

### 7. Clean up after merge

After owner confirms merge:

```bash
git checkout main
git pull origin main
git branch -d <feature-branch>
```

Ask before deleting the remote branch.

### 8. Sync docs

Use the repository's documentation model. If the project uses easy-coding docs, update:

- `docs/easy-roadmap.md`
- `docs/current-work.md`
- active `docs/batch/batchN-feature-name/IMPLEMENTATION_PLAN.md`

Otherwise, look for agent instructions, docs indexes, roadmaps, status files, ADRs, and context files. Update only existing relevant docs unless the owner asked to create a new doc.

## Edge Cases

| Situation | Action |
| --- | --- |
| Already on main/master | Stop; nothing to close |
| Uncommitted changes | Ask whether to commit or stash |
| Branch already pushed | Continue to PR creation |
| PR already exists | Report existing URL |
| `gh` unauthenticated | Ask owner to run `gh auth login` |
| Pull fails | Stop and report conflict/divergence |
| No docs system | Skip docs sync |

## Completion Output

Report:

- PR URL or merge state
- branch cleanup result
- docs updated
- skipped steps and why
- remaining follow-up work
