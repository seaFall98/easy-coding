---
name: review-fix
description: Use when Easy Coding implementation is complete and ready for mandatory pre-acceptance review-fix, especially when a batch needs subagent code review, REVIEW.md, or final triage before manual acceptance.
---

# Review Fix

Run the Easy Coding review-fix gate once after implementation is complete and before manual acceptance. Do not use it after manual acceptance or during finish.

## Inputs

Collect a review packet before spawning the reviewer:

- accepted scope and non-goals
- repo path, branch, and base
- changed-file summary from `git status --short` and `git diff --stat`
- review target: pre-checkpoint working diff, staged diff, untracked files, or candidate commit/range
- relevant spec/plan/docs paths
- verification already run
- project-specific constraints

For untracked in-scope files, either use `git add -N` before diffing or include line-numbered file content in the packet.

## Reviewer

Use an independent native subagent. The subagent must not edit files.

Ask the subagent to load:

- this skill's review rules from `SKILL.md`
- `references/code-review-skill/upstream-skill.md`
- only the relevant language/framework guides under `references/code-review-skill/reference/`

For ZBlog-like Java/Spring + Vue/TypeScript work, the usual guides are:

- `reference/java.md`
- `reference/vue.md`
- `reference/typescript.md`
- `reference/security-review-guide.md`
- `reference/code-quality-universal.md`

Optional external tools such as OpenCodeReview are supplemental only. They never replace the native subagent review.

## Output

Write or update the batch review document, normally:

```text
docs/batch/<batch-name>/REVIEW.md
```

If the project has a different docs model, use its mapped review file.

The review document must include:

- review target and reviewer mode
- reviewed scope
- summary decision: no must-fix / must-fix found / blocked
- findings grouped as `must-fix`, `follow-up`, and `non-issue`
- file/line references for actionable findings where possible
- verification already run and verification needed after fixes

Keep the review document concise. Do not paste full diffs or long generated logs.

## Triage And Fix

The main agent owns integration:

1. Read the whole `REVIEW.md`.
2. Classify each finding:
   - `must-fix`: correctness, security, privacy, permissions, persistence, migration/data-loss, or clear regression risk introduced by the change.
   - `follow-up`: valid but outside the accepted batch or too risky for the current change window.
   - `non-issue`: false positive, pre-existing issue, or already covered by the accepted contract.
3. Fix all in-scope `must-fix` findings.
4. Rerun checks affected by those fixes.
5. Update `REVIEW.md` and the live plan/status doc with the final review-fix result.
6. Create the local checkpoint commit when repository rules allow commits.

Do not ask for manual acceptance until the review document exists, must-fix findings are resolved or explicitly blocked, affected checks have been rerun, and the checkpoint commit exists where allowed.

## Prompt Template

```text
You are the independent Easy Coding review-fix reviewer. Do not edit files.

Load review guidance:
- <path-to-review-fix>/SKILL.md
- <path-to-review-fix>/references/code-review-skill/upstream-skill.md
- relevant guides: <list>

Review target:
- Repo:
- Branch/base:
- Diff/range:
- Changed surfaces:
- Accepted scope:
- Non-goals:
- Verification already run:
- Project constraints:

Focus on actionable issues introduced by this change: correctness, false completion, security/privacy, persistence, migration/data-loss/idempotency, accidental UI/copy changes, maintainability, and stale docs.

Return concise Markdown suitable for docs/batch/.../REVIEW.md. Group findings as must-fix, follow-up, and non-issue. If there are no actionable issues, say that directly and note residual risks.
```
