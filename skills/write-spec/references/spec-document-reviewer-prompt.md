# Spec Document Reviewer Prompt

Use this template when dispatching a separate spec reviewer pass.

Purpose: verify the spec is complete, consistent, and ready for implementation planning.

## Task

Review the spec document at `[SPEC_FILE_PATH]`.

## What to Check

| Category | What to Look For |
| --- | --- |
| Completeness | Missing sections, placeholders, unresolved requirements |
| Consistency | Conflicting requirements, scope drift, mismatched behavior |
| Clarity | Requirements ambiguous enough to cause the wrong implementation |
| Scope | Too broad for a single plan, or multiple independent subsystems |
| YAGNI | Unrequested features, over-engineering, speculative extras |

## Calibration

- Only flag issues that would cause real problems during implementation planning.
- Do not nitpick style unless it hides a real ambiguity.
- Prefer actionable findings over general commentary.

## Output

Return the smallest set of concrete fixes needed before planning can start.
