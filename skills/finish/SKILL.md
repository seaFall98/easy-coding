---
name: finish
description: "Complete the development closure workflow for a feature branch. Use when the user says they are done, ready to wrap up, ready to open a PR, ready to merge, finish this feature, close out this branch, or similar."
---

# Finish

Complete the standard GitHub PR-based closure workflow for a feature branch.

## Prerequisites

Before starting, verify:

- The owner has confirmed manual acceptance.
- All intended changes are committed in a local checkpoint commit that can be inspected later.
- The working tree is clean.
- The `gh` CLI is authenticated.
- The current branch is a feature branch, not `main` or `master`.
- The target base branch is known. Use `main` by default unless the repo or owner says otherwise.

If the only missing prerequisite is uncommitted in-scope accepted work, do not continue to PR steps. First inspect the diff, ensure docs/status are current, run or confirm relevant verification, and create the missing local checkpoint commit if repository rules allow it. If the repo/user has not allowed commits, stop and ask.

If any other prerequisite fails, report what is missing and stop.

## Workflow

### 1. Identify branch and repo

```bash
git branch --show-current
git remote get-url origin
git status --short
```

Stop if already on `main` or `master`.

If `git status --short` is not clean:

- Separate in-scope accepted work from unrelated owner changes.
- For in-scope accepted work, finish the review-fix/docs/verification loop and commit it before proceeding.
- For unrelated or ambiguous changes, stop and ask the owner how to handle them.
- Do not stash, discard, or silently carry uncommitted changes into PR creation.

### 2. Fetch and sync the base branch

```bash
git fetch origin --prune
git switch <base-branch>
git pull --ff-only origin <base-branch>
```

If the base branch cannot fast-forward, stop and report the divergence. Do not force reset or rebase without explicit approval.

### 3. Integrate the latest base branch locally

Return to the feature branch and integrate the current base branch before pushing or creating a PR:

```bash
git switch <feature-branch>
git merge --no-edit origin/<base-branch>
```

Use merge by default because it does not rewrite branch history and is safer for shared feature branches. If the repository requires linear history, ask the owner before rebasing. Never force-push after a rebase unless the owner explicitly approves `--force-with-lease`.

If conflicts occur:

- Resolve them locally on the feature branch.
- Keep conflict resolution in the feature branch commit history.
- Re-run relevant verification after the conflict is resolved.
- Stop and ask for help if the conflict requires product or architecture decisions.

Do not open a PR while the feature branch is behind the base branch or has unresolved conflicts.

### 4. Re-run relevant verification

Integration can break previously passing work. Re-run the checks that cover the changed area, or clearly report why a check is skipped.

Never reuse old verification results as proof after merging in new base-branch changes.

If integration requires fixes, make them on the feature branch and commit them before pushing. The working tree must be clean before PR creation.

Before pushing, run a final `git status --short` and `git log --oneline -3` check so the PR is anchored to committed, reviewable work.

### 5. Push the feature branch

```bash
git push origin <feature-branch>
```

If push is rejected because the remote feature branch advanced, fetch and inspect the divergence. Do not force-push by default.

```bash
git fetch origin <feature-branch>
git log --oneline --left-right HEAD...origin/<feature-branch>
```

Merge the remote feature branch locally or ask the owner what to do if the divergence is not obviously safe.

### 6. Build PR title and body

Use real branch commits and real verification data.

PR body should include:

- Summary
- Key changes
- Verification actually run
- Skipped verification and why
- Manual acceptance note

Never fabricate test results.

### 7. Create or report PR

```bash
gh pr create --base <base-branch> --head <feature-branch> --title "<title>" --body "<body>"
```

If a PR already exists, report its URL instead of creating a duplicate.

### 8. Wait for owner merge

Tell the owner to review and merge on GitHub, then report back.

Do not merge via CLI unless the owner explicitly asks for it.

### 9. Clean up after merge

After owner confirms merge:

```bash
git switch <base-branch>
git pull --ff-only origin <base-branch>
git branch -d <feature-branch>
```

Ask before deleting the remote branch.

### 10. Sync docs

Use the repository's documentation model. If the project uses easy-coding default docs, update:

- `docs/easy-roadmap.md`
- `docs/current-work.md`
- active `docs/batch/batchN-feature-name/IMPLEMENTATION_PLAN.md`

If the project adopted easy-coding with project-specific roadmap/status names, update the mapped files instead of creating duplicate defaults.

Otherwise, look for agent instructions, docs indexes, roadmaps, status files, ADRs, and context files. Update only existing relevant docs unless the owner asked to create a new doc.

## Edge Cases

| Situation | Action |
| --- | --- |
| Already on main/master | Stop; nothing to close |
| Uncommitted in-scope accepted work | Review, verify, update docs if needed, commit locally, then continue |
| Uncommitted unrelated or ambiguous changes | Stop and ask whether to keep, commit separately, stash, or ignore |
| Branch already pushed | Continue to PR creation |
| PR already exists | Report existing URL |
| `gh` unauthenticated | Ask owner to run `gh auth login` |
| Base branch cannot fast-forward | Stop and report conflict/divergence |
| Feature conflicts with base | Resolve locally on feature branch, re-run verification, then push |
| Push rejected because remote branch advanced | Fetch, inspect divergence, and do not force-push by default |
| Linear history required | Ask before rebase; never force-push without explicit approval |
| No docs system | Skip docs sync |

## Completion Output

Report:

- PR URL or merge state
- branch cleanup result
- docs updated
- skipped steps and why
- remaining follow-up work
