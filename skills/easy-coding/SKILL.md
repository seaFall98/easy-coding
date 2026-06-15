---
name: easy-coding
description: Lightweight AI workflow router for complex feature delivery. Use when orchestrating multi-step web or full-stack feature work, deciding whether to setup-easy-coding, to-roadmap, grill, write-spec, goal-plan, implement autonomously, diagnose, accept, or finish, and when routing to supporting skills such as setup-easy-coding, to-roadmap, grill-with-docs, write-spec, goal-plan, diagnose, tdd, doc-sync, and finish.
---

# Easy Coding

Use this skill as the entry point for complex feature delivery.

It does not replace specialized skills. It decides the current stage, the right complexity tier, and the next supporting skill or action.

## Core Rules

- Treat `a-light`, `aa-standard`, and `aaa-heavy` as workflow tiers, not separate product modes.
- Default to the smallest viable workflow that can still reach real acceptance.
- Keep `Resolved Decisions` current. Record the current stage, confirmed choices, and next action after each meaningful response.
- If a repo has not adopted the easy-coding docs model, route to `setup-easy-coding` before feature work.
- If no accepted roadmap exists, route to `to-roadmap` after project intake grill.
- Owner-facing flow has only three normal gates: grill, manual acceptance, and finish confirmation.
- After grill is resolved, spec draft, plan, implement, verify, and review-fix are agent-owned unless a blocking product decision appears.
- Use `grill-with-docs` first when requirements, boundaries, terminology, or proven solutions are still unclear.
- Route to `write-spec` after grill and before planning.
- Use `goal-plan` after a spec is accepted and the work needs a goal-mode execution plan.
- Use `diagnose` only for non-trivial bugs, regressions, acceptance failures, or unknown failure modes.
- Use `tdd` when the work is safest to drive by tests and behavior contracts.
- Run docs sync as part of finish, not as a separate owner-facing phase.
- Use `finish` when the branch is ready to close out.

## Owner Flow

1. **Grill**
   - Clarify the request, constraints, non-goals, and owner decisions.
   - For project setup or roadmap creation, first confirm workspace root, source git root, docs root, and collaboration model.
   - Research mature or official solutions when the domain is risky or already solved well elsewhere.
   - Stop grilling when the agent can write a coherent spec and execute without guessing.

2. **Manual Acceptance**
   - Owner verifies the delivered behavior with real data, real pages, and real navigation.
   - If acceptance fails or a bug appears, route to `diagnose` or a bounded review-fix loop.

3. **Finish Confirmation**
   - Owner confirms the feature can be closed out.
   - Finish includes docs sync, final branch/PR cleanup, and final state recording.

## Agent-Owned Internal Pipeline

After grill is complete, proceed autonomously unless a human gate is hit.

1. **Spec Draft**
   - Use `write-spec` to synthesize the spec from grill decisions, repo context, accepted research, and explicit assumptions.
   - Put the spec in `docs/batch/batchN-feature-name/SPEC.md` when the project uses the easy-coding docs model.
   - If the spec exposes a missing product decision, return to grill with the smallest blocking question.
   - If no blocking questions remain, treat the spec as the accepted execution source and continue.

2. **Plan**
   - Convert the accepted spec into an executable goal-mode plan.
   - Put the plan in `docs/batch/batchN-feature-name/IMPLEMENTATION_PLAN.md` when the project uses the easy-coding docs model.
   - Treat `IMPLEMENTATION_PLAN.md` as the live control document, including phase status and final acceptance notes.
   - Split by behavior and dependency, not by file list.
   - Add review gates for heavy work.

3. **Implement**
   - Work in one feature branch.
   - Use subagents only where they can make a bounded contribution.
   - Keep changes aligned with the confirmed scope.

4. **Verify**
   - Run targeted checks for the affected area.
   - Verify real behavior, not placeholders.

5. **Review-fix**
   - Review the diff or subagent output.
   - Fix clear issues before asking the owner to accept.

6. **Acceptance Handoff**
   - Tell the owner exactly what changed, what passed, what was not verified, and how to manually accept.

## Finish

When the owner confirms finish:

- Run docs sync for current direction, roadmap, accepted deviations, and relevant task docs.
- Close out the branch or PR using the repository's finish workflow.
- Record the final state and any remaining follow-up work.

## Escalation

- Prefer the named supporting skill when one fits.
- Ask the smallest blocking question when a decision is truly missing.
- Do not carry old stage assumptions forward if the current `Resolved Decisions` says otherwise.
- Do not ask the owner to approve routine spec, plan, implementation, or review-fix steps after grill is complete.

## Output Style

Be concise and decisive. Tell the user:
- current stage
- what is confirmed
- what the agent will do next
- what the owner needs to do, if anything
- which supporting skill is being used
