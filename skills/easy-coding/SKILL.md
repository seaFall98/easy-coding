---
name: easy-coding
description: Lightweight AI workflow router for complex feature delivery. Use when orchestrating multi-step web or full-stack feature work, deciding whether to setup-easy-coding, to-roadmap, grill, write-spec, goal-plan, implement autonomously, diagnose, accept, or finish, and when routing to supporting skills such as setup-easy-coding, to-roadmap, grill-with-docs, write-spec, goal-plan, diagnose, tdd, doc-sync, and finish.
---

# Easy Coding

Use this skill as the entry point for complex feature delivery.

It does not replace specialized skills. It decides the current stage, the right complexity tier, and the next supporting skill or action.

## Core Rules

- Treat `light`, `standard`, and `heavy` as workflow tiers, not separate product modes. Accept legacy aliases `a-light`, `aa-standard`, and `aaa-heavy`, but prefer the short tier names in examples and owner-facing docs.
- Default to the smallest viable workflow that can still reach real acceptance.
- Keep `Resolved Decisions` current. Record the current stage, confirmed choices, and next action after each meaningful response.
- If a repo has not adopted the easy-coding docs model, route to `setup-easy-coding` before feature work.
- If no accepted roadmap exists, route to `to-roadmap` after project intake grill.
- Owner-facing flow has only three normal gates: grill, manual acceptance, and finish confirmation.
- After grill is resolved, spec draft, plan, implement, verify, and review-fix are agent-owned unless a blocking product decision appears.
- Internal checkpoints are not owner gates. Do not stop, final-answer, or wait for the owner after writing a spec, writing a plan, completing an implementation phase, passing a compile/build, or updating docs. Report those checkpoints in commentary, update the live plan/status, and continue the agent-owned pipeline until a real human gate or acceptance handoff is reached.
- Use `grill-with-docs` first when requirements, boundaries, terminology, or proven solutions are still unclear.
- Route to `write-spec` after grill and before planning.
- Use `goal-plan` after a spec is accepted and the work needs a goal-mode execution plan.
- Use goal-mode only when the owner explicitly asks for `/goal`, `goal mode`, `create a goal`, `创建 goal`, `完成Pipeline`, or an equivalent explicit goal request. Do not infer goal-tool authorization from ordinary feature work.
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

Important continuation rule:

- Treat every internal phase/checkpoint as a resume marker, not a stopping point.
- Use commentary for checkpoint updates such as "Phase A passed; continuing Phase B".
- Do not end the turn with a final answer merely because a phase completed or verification passed.
- A final answer is appropriate only for a real human gate, a blocker, or the final acceptance handoff.
- If the work is large, continue phase by phase; task size alone is not a blocker.

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

2.5. **Goal Prompt and Goal-Mode Start**
   - If and only if the owner explicitly requested goal-mode execution, write a concise goal prompt after the spec and plan are clean. Example owner wording: `/easy-coding heavy 创建 goal完成Pipeline`.
   - The goal prompt should include objective, branch, spec path, plan path, current phase, stop conditions, verification expectations, and manual acceptance boundary.
   - Then start the goal with the available goal tool and continue the complete Agent-Owned Internal Pipeline under that goal: spec draft -> plan -> implement -> verify -> review-fix -> acceptance handoff.
   - If goal tooling is unavailable, unavailable in the current mode, or not explicitly authorized by the owner, continue the same agent-owned pipeline without goal mode. Do not stop merely to ask the owner to invoke `/goal`.
   - A skill instruction alone is not user authorization to create a goal; the owner request must be explicit in the conversation or command.

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
   - This is the first normal stopping point after grill unless a blocker or human gate was hit earlier.

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
- Do not convert an internal checkpoint into an owner handoff. If no human gate is hit, keep working.
- Do not create a goal unless goal-mode execution was explicitly requested by the owner; if it was requested, create the goal after spec/plan instead of asking the owner to trigger it manually. `创建 goal完成Pipeline` means the goal objective is the whole Agent-Owned Internal Pipeline, not merely planning or starting a goal.

## Output Style

Be concise and decisive. Tell the user:
- current stage
- what is confirmed
- what the agent will do next
- what the owner needs to do, if anything
- which supporting skill is being used
