---
name: finish
description: "Complete the development closure workflow for a feature branch. Use when the user says they are done, ready to wrap up, ready to open a PR, ready to merge, finish this feature, close out this branch, or similar."
---

# Finish

Complete the standard GitHub PR-based closure workflow for an already accepted feature branch.

Finish is administrative closeout: sync docs/status, push the accepted branch, and create or report the PR. It is not another implementation, review, or verification phase.

## Prerequisites

Before starting, verify:

- The owner has confirmed manual acceptance.
- All intended changes are committed in a local checkpoint commit that can be inspected later.
- The working tree is clean.
- The `gh` CLI is authenticated.
- The current branch is a feature branch, not `main` or `master`.
- The target base branch is known. Use `main` by default unless the repo or owner says otherwise.

If there is uncommitted in-scope accepted work, stop and report that finish cannot continue from a moving target. The accepted checkpoint should already be committed before finish.

If any other prerequisite fails, report what is missing and stop.

Do not start a new review, run regression tests, or make code fixes in finish. Manual acceptance already validated the checkpoint. If finish reveals any need for code changes, stop and ask the owner how to proceed.

## Workflow

### 1. Identify branch and repo

```bash
git branch --show-current
git remote get-url origin
git status --short
```

Stop if already on `main` or `master`.

If `git status --short` is not clean:

- Stop and report the dirty files.
- Do not review, fix, commit, stash, discard, or silently carry uncommitted changes into PR creation.

### 2. Sync docs

Use the repository's documentation model to record the accepted/PR-ready state before opening the PR. If the project uses easy-coding default docs, update:

- `docs/easy-roadmap.md`
- `docs/current-work.md`
- active `docs/batch/batchN-feature-name/IMPLEMENTATION_PLAN.md`

If the project adopted easy-coding with project-specific roadmap/status names, update the mapped files instead of creating duplicate defaults.

Otherwise, look for agent instructions, docs indexes, roadmaps, status files, ADRs, and context files. Update only existing relevant docs unless the owner asked to create a new doc.

If docs live inside the same source repo and require a commit, a docs-only finish commit is allowed when it records acceptance or PR state. Do not include code changes in that commit.

### 3. Optional remote refresh

Fetch remote refs only to avoid duplicate PRs and stale branch assumptions:

```bash
git fetch origin --prune
```

Do not switch to the base branch, pull the base branch, merge base into the feature branch, rebase, or resolve conflicts during finish. Let the PR surface whether the branch is mergeable.

### 4. Final branch check

Do not rerun review or regression tests in finish. Only confirm the branch is still the accepted clean checkpoint.

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

Stop and ask the owner what to do if the divergence is not obviously safe. Do not force-push by default.

### 6. Build PR title and body

Use real branch commits and the verification/manual acceptance data already produced before finish.

PR body should include:

- Summary
- Key changes
- Verification already run before finish
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

## Edge Cases

| Situation | Action |
| --- | --- |
| Already on main/master | Stop; nothing to close |
| Uncommitted in-scope accepted work | Stop; accepted work must already be checkpoint-committed before finish |
| Uncommitted unrelated or ambiguous changes | Stop and ask whether to clean them up before finish |
| Branch already pushed | Continue to PR creation |
| PR already exists | Report existing URL |
| `gh` unauthenticated | Ask owner to run `gh auth login` |
| Base branch moved | Still create/report PR unless the owner asked to pre-integrate; let PR show mergeability |
| Feature conflicts with base | Report the PR conflict state; do not resolve in finish mode |
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
