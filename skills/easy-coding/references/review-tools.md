# Review Tools

Load this reference when preparing, running, or recording Easy Coding review-fix.

## Default Policy

Review-fix has two mandatory roles:

1. An independent subagent reviewer.
2. The main agent as integrator, triager, fixer, and recorder.

CodeRabbit is the default tool the subagent should use for code review when available/authenticated. OpenCodeReview, PR reviewers, and main-agent checklist review are supporting signals. They do not replace the subagent reviewer.

Non-negotiables:

- Before manual acceptance, spawn/request a subagent review of the actual diff.
- Ask the subagent to use CodeRabbit local CLI when it is available/authenticated and suitable for the diff.
- Give the subagent the accepted scope, changed files, branch/base, relevant docs, verification already run, and project completion rules.
- Ask the subagent for CodeRabbit findings plus its own reviewed judgment, ordered by severity, with file/line references where possible.
- The main agent must inspect the subagent findings, classify them, fix must-fix items, rerun affected checks, and record the review result.
- If subagent tooling is unavailable, stop at a blocker. Do not present the work as ready for manual acceptance with main-agent-only review.
- Never claim a subagent, CodeRabbit, or OpenCodeReview review ran unless it actually ran and its output was inspected.

For `light` work, the subagent prompt can be compact. The subagent requirement still exists.

## Subagent Review

Minimum prompt shape:

```text
Review this diff for Easy Coding review-fix. Do not edit files.

Scope: ...
Repo/path: ...
Base/head or committed/uncommitted state: ...
Changed surfaces: ...
Verification already run: ...
Project completion rules: ...

Use CodeRabbit local CLI if available/authenticated and suitable for the diff. Include the exact CodeRabbit command/result in your final note. If CodeRabbit cannot run, state why and still perform your own subagent review.

Focus on correctness, false completion, accidental UI/copy changes, security/privacy, migration/data-loss/idempotency, stale docs, and missing verification.
Return findings first, ordered by severity, with file/line references where possible. If there are no issues, say so clearly and note residual risk.
```

The required output is the subagent's reviewed judgment. Raw CodeRabbit output alone is not enough.

## CodeRabbit Default Subagent Tool

Prefer CodeRabbit as the local external review tool inside the required subagent review. It can review committed and uncommitted local diffs without requiring a PR.

When the `coderabbit:code-review` skill is available and CodeRabbit is selected, load that skill for current command guidance before running the CLI.

Preflight:

```bash
coderabbit --version
coderabbit auth status --agent
git status --short
git diff --stat
```

If CodeRabbit was explicitly requested or the subagent decides to run it for review-fix, follow the `coderabbit:code-review` skill's setup and failure-handling rules, including `coderabbit auth login --agent` when authentication is missing. If CodeRabbit has not yet been selected and preflight shows it is unavailable/unauthenticated, or if the Git state is unsuitable for the target diff, record why and continue with the required subagent review. Do not claim a fallback review came from CodeRabbit.

Common local scopes:

```bash
coderabbit review --agent
coderabbit review --agent -t uncommitted
coderabbit review --agent -t committed
coderabbit review --agent --base main
coderabbit review --agent --base-commit <sha>
```

If the repo has review context such as `AGENTS.md`, `.coderabbit.yaml`, or an accepted implementation plan, pass the relevant file(s) with CodeRabbit's context option when the CodeRabbit skill documents it.

## OpenCodeReview Optional Support

OpenCodeReview is optional. Use it for explicit owner requests, small targeted diffs, or fallback/supplemental cases where its runtime cost is acceptable.

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

If OCR is slow, noisy, misconfigured, rate-limited, or previews an unexpectedly broad scope, stop using it for that run and record why. The required subagent+CodeRabbit-default review path still remains.

## Findings

Classify all reviewer findings before fixing:

- `must-fix`: correctness, security, privacy, migration/data-loss, permission, persistence, or clear regression risk.
- `follow-up`: valid but outside the accepted batch or too risky for the current change window.
- `non-issue`: false positive, pre-existing issue not introduced by the diff, or already covered by tests/contracts.

After fixing must-fix findings, rerun affected verification. Old checks do not prove review-fix changes.

## Handoff Record

Record review mode in the live plan/status document and acceptance handoff:

```text
Review mode: required subagent review (<agent id/name>) using CodeRabbit via `...` / CodeRabbit unavailable because ...
Subagent result: no must-fix findings / fixed N must-fix findings / deferred N follow-ups
Optional extra tools: OpenCodeReview via `...` / none
Main-agent final pass: completed
Verification after review-fix: ...
```
