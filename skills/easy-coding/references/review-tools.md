# Review Tools

Load this reference when choosing, installing, checking, running, or recording a code-review tool for Easy Coding review-fix.

## Default Policy

OpenCodeReview is the preferred default local CLI reviewer for Easy Coding.

- Treat OpenCodeReview as a recommended external prerequisite, similar to Matt Pocock skills.
- For automatic pipeline review-fix, invoke the `ocr` CLI directly. Do not depend on the owner manually typing a slash command or `@Open Code Review`.
- Install the OpenCodeReview skill for agent guidance. The Codex plugin is optional and useful for manual invocation, but it is not required for Easy Coding's automatic review-fix path.
- If the OpenCodeReview skill is available in the skill list, load it before running OCR so tool-specific guidance is current.
- Do not treat it as a hard runtime dependency. Review-fix must still complete when OCR is unavailable.
- Never claim OpenCodeReview ran unless the command actually ran and its output was inspected.
- Always keep the main-agent checklist review as the final safety pass.

Preferred review chain:

1. Run OpenCodeReview when installed, configured, and safe for the current diff.
2. Use an independent subagent review when OCR is unavailable, the work is heavy/risky, or an extra pass is useful.
3. Finish with the main-agent checklist review.
4. Use PR/cloud reviewers such as CodeRabbit only when the owner explicitly asks, the repo workflow requires it, or a PR workflow is already active.

## OpenCodeReview Setup

Install the OpenCodeReview skill separately from Easy Coding so agents can load its usage guidance:

```bash
npx skills add alibaba/open-code-review --skill open-code-review
```

Install the OCR CLI because Easy Coding's automatic pipeline invokes `ocr` directly:

```bash
npm install -g @alibaba-group/open-code-review
ocr config provider
ocr config model
ocr llm test
```

Optional Codex plugin installation provides a manual `@Open Code Review ...` entrypoint. It is not the automatic pipeline path:

```bash
codex plugin marketplace add alibaba/open-code-review
```

OCR requires its own LLM configuration. Environment variables or `~/.opencodereview/config.json` may supply the model endpoint and token.

## Preflight

Before running OCR, check:

- `ocr` is on PATH.
- `ocr llm test` succeeds, unless the owner explicitly accepts an offline/preflight-only run.
- The review scope is explicit enough to avoid scanning unrelated generated files, local secrets, or env files.
- The repo has a clean enough Git state to distinguish in-scope work from unrelated owner changes.

Useful preflight commands:

```bash
ocr version
ocr review --preview
ocr review --commit <sha> --preview
ocr review --from <base> --to <head> --preview
```

## Review Scope

Prefer precise scopes over bare workspace review.

- For one committed acceptance candidate: `ocr review --audience agent --commit <sha> --background "<context>"`
- For a feature branch: `ocr review --audience agent --from <base> --to HEAD --background "<context>"`
- For current uncommitted work: `ocr review --audience agent --background "<context>"`, only after checking untracked files and secrets.

Use `--format json` when the output needs parsing or when a report will be archived.

## Findings

Classify OCR findings before fixing:

- `must-fix`: correctness, security, privacy, migration/data-loss, permission, persistence, or clear regression risk.
- `follow-up`: valid but outside the accepted batch or too risky for the current change window.
- `non-issue`: false positive, pre-existing issue not introduced by the diff, or already covered by tests/contracts.

After fixing must-fix findings, rerun affected verification. Old checks do not prove review-fix changes.

## Fallbacks

If OCR cannot run, record the reason and continue with the fallback chain:

1. Independent subagent review when available.
2. Main-agent checklist review.
3. Affected verification rerun after any fixes.

Do not block review-fix merely because OCR is missing, misconfigured, rate-limited, or unsuitable for the current scope. Do not hide the downgrade.

## Handoff Record

Record review mode in the live plan/status document and acceptance handoff:

```text
Review mode: OpenCodeReview via `ocr review --audience agent --from main --to HEAD --background "..."`
OCR result: no must-fix findings / fixed N must-fix findings / unavailable because ...
Fallback: independent subagent review used / main-agent checklist only because ...
Verification after review-fix: ...
```
