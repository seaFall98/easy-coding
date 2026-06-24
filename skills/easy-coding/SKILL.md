---
name: easy-coding
description: Use when routing complex feature delivery, roadmap-backed work, or /easy-coding light/standard/heavy requests across setup, grill, spec, plan, implementation, verification, acceptance, finish, or goal-mode execution.
---

# Easy Coding

Easy Coding is a lightweight delivery router. It chooses the smallest workflow that can reach real acceptance, then keeps moving until a true human gate appears.

## Core Rules

- Tiers are `light`, `standard`, and `heavy`. Accept legacy aliases `a-light`, `aa-standard`, and `aaa-heavy`, but use the short names in owner-facing examples.
- Keep the main path practical. Do not add process just to look disciplined; do not drop useful checks just to look fast.
- Use four normal owner-facing gates: grill, post-grill goal-mode opt-in, manual acceptance, and finish confirmation.
- After grill resolves the product decisions, stop at the post-grill owner gate unless the owner has already explicitly requested goal-mode pipeline execution. Tell the owner the grill is complete and ask them to explicitly request goal mode to complete the pipeline.
- Once the owner explicitly requests goal-mode pipeline execution, spec, plan, implementation, verification, review-fix, and docs/status updates are agent-owned.
- The post-grill goal-mode opt-in is an entry gate only. Do not add owner gates inside downstream pipeline skills such as `write-spec` or `goal-plan`; they are internal pipeline steps once goal mode is authorized.
- At every internal stage boundary, run a pipeline checkpoint: name the stage just completed, consult the Agent-Owned Pipeline or live plan for the next required step, update visible plan/status when available, then continue. Do not rely on memory of the pipeline order.
- Review-fix is mandatory once after implementation is complete and before manual acceptance. It must be a visible plan/status item, must use `review-fix`, and must write/update `REVIEW.md`. Do not run review after every subtask by default.
- Review-fix must include an independent native subagent review before manual acceptance. Optional external tools such as OpenCodeReview are supporting evidence only. If subagent tooling is unavailable, stop at a blocker instead of downgrading to main-agent-only review.
- After review-fix passes and affected checks are rerun, create a local checkpoint commit for the accepted candidate when repository rules allow commits. If commits are not allowed, explicitly say why and provide the exact uncommitted state. Do not leave a large completed batch only in the working tree before finish.
- Internal checkpoints are resume markers, not stopping points. Do not final-answer after only drafting a spec, writing a plan, passing a build, updating docs, or finishing one phase.
- Keep `Resolved Decisions` or the project’s live control document current when meaningful decisions, phase status, verification results, or stop conditions change.
- If a repo has not adopted the Easy Coding docs model, route to `setup-easy-coding`. If no accepted roadmap exists, route to `to-roadmap` after intake.
- If roadmap items look stale, audit them before building. Classify each relevant item as keep, already done, rewrite, future, or drop.
- Use goal mode only when the owner explicitly asks for `/goal`, `goal mode`, `create a goal`, `创建 goal`, `完成Pipeline`, or equivalent. If requested and tooling exists, create the goal yourself. If requested but tooling is unavailable, stop at a blocker. If not requested, stop at the post-grill owner gate and ask for explicit goal-mode authorization.
- Before ending a turn, run the stop-condition check: final answer only for a blocker, the post-grill goal-mode opt-in gate, another real human gate, acceptance handoff, or finish report.

## Tier Router

| Tier | Use for | Expected weight |
| --- | --- | --- |
| `light` | Small bounded bugfix or local enhancement | Minimal notes, targeted verification, compact handoff |
| `standard` | One feature crossing a few files or one surface | Brief spec/plan, targeted tests/builds, docs only where decisions changed |
| `heavy` | Multi-surface, data-model, roadmap, UI-quality, migration, or Docker/local-stack work | Grill/stale audit, spec, plan, phased implementation, stronger verification, compact acceptance handoff |

Default down, not up. Escalate when a wrong assumption would be expensive, when multiple surfaces must close the loop, or when the owner explicitly asks for `heavy`.

## Stage Router

- Unclear requirements, boundaries, terminology, or mature solution choices: use `grill-with-docs`.
- Accepted grill result needing a source of truth: use `write-spec`.
- Accepted spec needing execution structure: use `goal-plan` or a project implementation plan.
- Feature or bug implementation with behavior risk: use `tdd` where tests can pin the contract.
- Non-trivial bug, regression, failed acceptance, or unknown failure mode: use `diagnosing-bugs`.
- UI quality work: use the project’s frontend/design skill before changing visuals.
- Implementation complete and ready for pre-acceptance review: use `review-fix`.
- Completion after owner acceptance: use `doc-sync`, then `finish`.

When a supporting skill fits, use it; do not reimplement it inside Easy Coding.

## Agent-Owned Pipeline

Run this pipeline only after the owner explicitly requests goal-mode pipeline execution. If grill just completed and goal mode was not explicitly requested, stop and tell the owner: "grill is complete; please explicitly request goal mode to complete the pipeline." After goal mode is authorized, do not stop at spec or plan checkpoints unless a real blocker appears; `write-spec` and `goal-plan` are internal pipeline steps.

1. If goal tooling exists, create a concise goal covering the whole pipeline: objective, branch, spec path, plan path, current phase, stop conditions, verification expectations, and manual acceptance boundary.
2. Draft or update the spec from confirmed decisions, repo context, accepted research, and explicit assumptions.
3. Create or update the implementation plan. Split by behavior and dependency, not by file list.
4. Implement in the active feature branch, respecting project branch/push/PR permissions.
5. Verify real behavior, persistence, reload/reopen behavior, and integration paths relevant to the tier.
6. Use `review-fix`: run the required independent subagent review of the current pre-checkpoint diff or candidate range, write/update `REVIEW.md`, triage findings, and fix clear issues before acceptance handoff.
7. Rerun the checks affected by review fixes. Old verification is not proof after review-fix changes.
8. Commit the reviewed acceptance candidate locally when commits are permitted, so finish/PR work has a stable, searchable checkpoint. Keep docs in the same commit when they describe the same completed work unless the project separates source and docs.
9. Hand off for manual acceptance with only delta information: what changed, what passed, what was not run, commit/checkpoint state, how to accept, and remaining risk.

After each numbered step, run the pipeline checkpoint before deciding what to do next. If the next step is review-fix, do review-fix; do not skip from verification to acceptance handoff.

Large work may take multiple internal phases. Phase size is not a blocker; only missing authority, missing product decisions, or external failure states are blockers.

## Acceptance Handoff Gate

Before asking the owner to manually accept, all boxes must be true:

- Implementation is complete for the accepted scope, or unfinished pieces are explicitly out of scope/follow-up.
- Relevant verification has passed or skipped checks are named with reasons.
- The diff has been reviewed by an independent subagent for false completion, accidental UI/copy changes, security/privacy leaks, migration hazards, stale docs, and obvious code-quality issues, and the subagent/tool mode used is recorded.
- Clear review findings have been fixed and the affected checks rerun.
- Current docs/status files match reality.
- A local checkpoint commit exists for the acceptance candidate when commits are permitted; otherwise the handoff states why no commit exists.

If any box is false, keep working. Do not present the work as ready for acceptance.

## Verification Policy

- Prefer meaningful targeted checks over ritual full-suite runs.
- Full suites are useful only when they are reliable, current, and proportionate to the change.
- If the owner or project docs identify old/V1 failures as obsolete, do not treat them as current gates. Run current targeted tests/builds instead, and record obsolete failures only if encountered.
- This is not permission to skip verification. Every handoff must show the relevant checks that did run, and any skipped checks with reasons.
- For `heavy`, try the documented local stack or Docker command when the project provides one and the change affects runtime integration. If it is not run, say why.

## Progressive References

Keep this file short. Load extra references only when their context pointer fires:

- `references/checklists.md`: load before review-fix and acceptance handoff; also load for `heavy`, stale-roadmap work, security/data consistency risk, uncertain verification strategy, UI quality acceptance, local-stack/Docker handoff, or compact handoff formatting.
- `references/review-tools.md`: load when preparing `review-fix`, choosing optional OpenCodeReview support, or recording a review blocker.
- `references/skill-map.md`: load when choosing among Easy Coding supporting skills.
- `references/publishing.md`: load only when publishing, installing, or syncing the skill itself.

## Finish

When the owner confirms finish:

- Finish is administrative closeout for an already accepted checkpoint. It is not another implementation, review, or verification phase.
- Sync current docs and roadmap status without duplicating content already available in those docs.
- Use the repository's finish workflow for branch, PR, merge, cleanup, and final state.
- Do not run new review or general regression tests during finish. Manual acceptance already validated the checkpoint.
- During finish, integrate the latest base branch locally before PR. If that merge is clean, continue. If conflicts require code resolution, resolve only when the owner approves returning to implementation, then rerun affected checks and get manual acceptance again before PR.
- Do not push, create a PR, merge, deploy, or delete important content unless the project/user permission boundary allows it.

## Output Style

Be concise and decisive. Tell the owner the current stage, confirmed direction, next action, needed owner action if any, and supporting skill being used.
