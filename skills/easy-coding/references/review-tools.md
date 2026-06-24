# Review Tools

Load this reference when preparing, running, or recording Easy Coding review-fix.

## Default Policy

Review-fix has two mandatory roles:

1. An independent subagent reviewer.
2. The main agent as integrator, triager, fixer, verifier, and recorder.

The default review path is the `review-fix` skill: a native subagent reviews the current pre-checkpoint diff or candidate range using the vendored code-review-skill guidance, then writes or updates `REVIEW.md`. External tools are optional supporting evidence. They do not replace the subagent reviewer.

Non-negotiables:

- Before manual acceptance, run `review-fix` once for the completed implementation. Do not review after every subtask by default.
- Review happens before the checkpoint commit. After fixing findings and rerunning affected checks, create the checkpoint commit.
- Give the subagent the accepted scope, changed files, branch/base, relevant docs, verification already run, and project completion rules.
- Ask the subagent for its own reviewed judgment, grouped into `must-fix`, `follow-up`, and `non-issue`, with file/line references where possible.
- The main agent must inspect the subagent findings, classify them, fix must-fix items, rerun affected checks, and record the review result.
- If subagent tooling is unavailable, stop at a blocker. Do not present the work as ready for manual acceptance with main-agent-only review.
- Never claim a subagent or OpenCodeReview review ran unless it actually ran and its output was inspected.

For `light` work, the subagent prompt can be compact. The subagent requirement still exists.

## Review Scope

Default to the pre-checkpoint working tree because it lets review fixes land before the acceptance-candidate commit:

```bash
git status --short
git diff --stat
git diff
git diff --cached
git ls-files --others --exclude-standard
```

Untracked in-scope files must be reviewable by content, not only by filename. Either mark them with `git add -N` before producing the diff, or include line-numbered file reads in the subagent prompt.

If the work is already committed for a project-specific reason, ask the subagent to review the candidate commit or branch range instead, and record that exception.

## Subagent Review

Minimum prompt shape:

```text
Review this diff for Easy Coding review-fix. Do not edit files.

Load and follow the `review-fix` skill. Use the vendored code-review-skill references relevant to this stack.

Scope: ...
Repo/path: ...
Review target: current pre-checkpoint diff / commit <sha> / range <base>..<head>
Changed surfaces: ...
Verification already run: ...
Project completion rules: ...

Review staged, unstaged, and untracked in-scope files. Focus on correctness, false completion, accidental UI/copy changes, security/privacy, migration/data-loss/idempotency, stale docs, and missing verification.
Return concise Markdown suitable for `docs/batch/.../REVIEW.md`. Group findings as `must-fix`, `follow-up`, and `non-issue`. If there are no issues, say so clearly and note residual risk.
```

The required output is the subagent's reviewed judgment. Raw external-tool output alone is not enough.

## Review Document

Write or update the active batch review file, usually `docs/batch/<batch-name>/REVIEW.md`.

Record:

- review target and reviewer mode
- reviewed scope
- summary decision
- findings grouped as `must-fix`, `follow-up`, and `non-issue`
- verification already run and verification needed after fixes
- final main-agent triage/fix result

## Optional OpenCodeReview Support

OpenCodeReview is optional. Use it only for explicit owner requests, small targeted diffs, or supplemental cases where its runtime cost is acceptable.

Install/setup when the owner chooses this path:

```bash
npx skills add alibaba/open-code-review --skill open-code-review
npm install -g @alibaba-group/open-code-review
ocr config provider
ocr config model
ocr llm test
```

Before running OCR, check:

- `ocr` is on PATH.
- `ocr llm test` succeeds, unless the owner explicitly accepts an offline/preflight-only run.
- `ocr review --preview` shows a small enough scope to justify the runtime.
- The repo Git state is clean enough to distinguish in-scope work from unrelated owner changes.

Useful scopes:

```bash
ocr review --preview
ocr review --commit <sha> --preview
ocr review --from <base> --to <head> --preview
ocr review --audience agent --commit <sha> --background "<context>"
ocr review --audience agent --from <base> --to HEAD --background "<context>"
```

If OCR is slow, noisy, misconfigured, rate-limited, or previews an unexpectedly broad scope, stop using it for that run and record why. The required subagent review still remains.

## Findings

Classify all reviewer findings before fixing:

- `must-fix`: correctness, security, privacy, migration/data-loss, permission, persistence, or clear regression risk.
- `follow-up`: valid but outside the accepted batch or too risky for the current change window.
- `non-issue`: false positive, pre-existing issue not introduced by the diff, or already covered by tests/contracts.

After fixing must-fix findings, rerun affected verification. Old checks do not prove review-fix changes.

## Handoff Record

Record review mode in the live plan/status document and acceptance handoff:

```text
Review mode: required subagent review (<agent id/name>)
Review target: pre-checkpoint working diff / commit <sha> / range <base>..<head>
Subagent result: no must-fix findings / fixed N must-fix findings / deferred N follow-ups
Optional extra tools: OpenCodeReview via `...` / none
Verification after review-fix: ...
Checkpoint commit after review-fix: <sha> / not created because ...
```
