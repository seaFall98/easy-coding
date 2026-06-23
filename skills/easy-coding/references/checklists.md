# Easy Coding Checklists

Load this reference only when the main skill points here: heavy work, stale roadmap items, review-fix risk, uncertain verification, UI quality acceptance, local-stack/Docker handoff, or final acceptance formatting.

## Stale Roadmap Audit

Before implementing roadmap-derived work, classify each relevant item:

| Status | Meaning | Action |
| --- | --- | --- |
| `keep` | Still accurate and valuable | Implement or verify it |
| `already done` | Current product already satisfies it | Record evidence; do not rebuild it |
| `rewrite` | Intent is valid but old wording/design is wrong | Update spec before implementing |
| `future` | Useful but outside current batch | Move to follow-up planning |
| `drop` | No longer useful | Explain briefly and exclude |

This audit is lightweight. It prevents blind V1 adaptation; it is not a new owner gate unless a product decision is genuinely missing.

## False-Completion Scan

Before acceptance handoff, check the changed surfaces for these traps:

- Placeholder or hardcoded data presented as done.
- A route returning HTTP 200 without real persistence or real read path.
- UI state that works until reload/reopen, then disappears.
- Admin and public flows using different semantics for the same domain object.
- Security/privacy fields leaked through public APIs.
- Docs or live plan still saying `next`, `pending`, or an old phase after completion.
- Review comments answered with explanation but no code or test when code was needed.
- Acceptance handoff prepared without review-fix and the following local checkpoint commit even though repository rules allow commits.
- Review-fix changes made after verification without rerunning the affected checks.

Fix findings in scope. If a finding is real but out of scope, name it as follow-up rather than hiding it.

## Verification Matrix

| Tier | Minimum useful verification |
| --- | --- |
| `light` | Targeted test, lint, type-check, or manual reproduction for the changed behavior |
| `standard` | Targeted backend/frontend checks plus the relevant build/type-check for touched surfaces |
| `heavy` | Targeted tests per changed surface, relevant builds, persistence/reload checks, and documented local-stack/Docker attempt when runtime integration is affected |

Full test suites are optional unless the project requires them or they are known reliable. If a full suite is dominated by documented obsolete/V1 failures, prefer current targeted checks and record the obsolete suite state only if encountered. Do not use obsolete failures as a reason to skip current tests.

For PR review fixes:

- Security, privacy, permissions, token handling, data consistency, or migration issues need a regression test or a strong targeted verification.
- Logic bugs need the smallest test that fails without the fix.
- Minor config/copy/doc changes need the relevant parser/build/check, not a ceremonial full suite.

## Review-Fix Loop

Before manual acceptance handoff, and again after PR review comments:

1. Inspect the actual diff, not just the remembered task.
2. Choose and record the review mode:
   - The main agent owns the review-fix outcome: inspect the diff, spawn/request the subagent review, triage findings, apply fixes, rerun checks, and record what happened.
   - Load `review-tools.md` before preparing the required subagent review, running optional OpenCodeReview, or recording a review blocker.
   - A subagent review is mandatory before manual acceptance. Main-agent checklist-only review is not an acceptable Easy Coding review-fix result.
   - The default review target is the current pre-checkpoint diff, including staged, unstaged, and untracked in-scope files. Review-fix happens before the checkpoint commit.
   - The subagent must provide its own reviewed judgment; raw tool output alone is not enough.
   - OpenCodeReview may support the review when selected, but it does not replace the subagent reviewer.
   - If subagent tooling is unavailable, stop at a blocker and explain what is missing. Do not downgrade to main-agent-only review.
   - Use PR/cloud reviewers only when the owner explicitly requests them, the repository workflow requires them, or a PR workflow is already active; they are still supplemental to the subagent review unless the owner changes this policy.
3. Review changed files and nearby contracts for:
   - false completion and placeholders
   - accidental public UI/copy/layout changes
   - security/privacy or secret leakage
   - migration/data-loss/idempotency hazards
   - stale docs/status
   - missing or weak tests for changed behavior
   - obvious code-quality and maintainability issues
4. Classify findings as must-fix, follow-up, or non-issue.
5. Fix must-fix items in scope.
6. Rerun checks affected by those fixes.
7. Record the review mode, fixes, verification result, and remaining follow-ups in the live plan/status doc.

Do not ask the owner to remind you to review. In Easy Coding, review-fix is agent-owned.

## Checkpoint Commit Gate

Before finish/PR work starts:

- `git status --short` should be clean.
- `git log --oneline -3` should show the acceptance-candidate commit.
- If the repository allows commits and the completed work is still uncommitted, commit it before finish.
- If unrelated owner changes are present, leave them alone and ask before touching them.

## UI Quality Acceptance

For public/admin UI work, include acceptance beyond "component exists":

- Real data path and empty/loading/error states.
- Visual hierarchy and spacing consistent with the approved product direction.
- Practical interaction details: disabled states, validation feedback, reload survival, and mobile where relevant.
- No GPT-like filler copy. If explanatory text does not help a real user act, remove it.

Use the project’s frontend/design skill when available.

## Compact Handoff Template

Keep handoff delta-only. Do not repeat roadmap/current-direction content the owner can already read in docs.

```
Changed:
- ...

Verified:
- command/check -> result

Not run:
- check -> reason

Known unrelated/obsolete failures:
- ...

Manual acceptance:
- ...

Branch/PR/docs:
- ...

Next:
- ...
```

Omit empty sections except `Verified` and `Not run`. If nothing was skipped, write `Not run: none`.
