---
name: goal-plan
description: Convert an accepted complex spec, roadmap, PRD, or feature plan into a concrete Codex goal-mode implementation plan. Use when the user wants to split a large spec into goal-mode phases, decide one goal vs multiple goals, define review gates, assign subagent work, prevent false completion, or produce an IMPLEMENTATION_PLAN.md-style execution plan.
---

# Goal Plan

Use this skill to turn an accepted feature spec into an execution plan that Codex can run in goal mode.

Do not use it for discovery or product debate. If the spec is still missing a blocking decision, ask the smallest possible question and stop before decomposition.

## Inputs

Read the spec/roadmap/PRD first. If needed, inspect only the repo files that affect execution order, risk, or verification.

Acceptable inputs:
- accepted spec
- roadmap section
- PRD
- batch plan
- high-level implementation plan

Pause when:
- the outcome is unclear
- acceptance criteria are missing for user-facing work
- the spec still contains unresolved product choices
- required reference material is unavailable

## Output Contract

Produce a goal-ready plan. If the project uses easy-coding docs, write to `docs/batch/batchN-feature-name/IMPLEMENTATION_PLAN.md`. If the user asks for a different file, use that path. Otherwise output the plan in chat.

If the owner explicitly requested goal-mode execution, also produce a concise goal prompt after the plan is clean. Explicit requests include `/goal`, `goal mode`, `create a goal`, `创建 goal`, `完成Pipeline`, or shorthand such as `/easy-coding heavy 创建 goal完成Pipeline`. The prompt should be ready to pass to the available goal tool and should include objective, spec path, plan path, branch, current phase, stop conditions, verification expectations, and manual acceptance boundary. Its objective is the complete Agent-Owned Internal Pipeline, not merely planning or goal creation. If goal tooling is unavailable or not explicitly authorized, continue the normal agent-owned pipeline without stopping for owner approval.

The plan should include:

```markdown
# Implementation Plan

## Recommended Goal Shape
- Objective:
- Mode: one goal with N phases / multiple goals
- Why this shape:
- Stop conditions:

## Execution Rules
- Scope:
- Non-goals:
- Required references:
- Human gates:

## Phase Plan
### Phase A: ...
- Purpose:
- Depends on:
- Main-agent tasks:
- Subagent tasks:
- Definition of done:
- Local verification:
- Phase review:
- Commit/checkpoint:

## Delegation Rules
- What can go to subagents:
- What must stay with main agent:
- Review prompts:

## Verification Matrix
- Backend:
- Frontend:
- Admin:
- Data/migration:
- Security:
- Manual acceptance:

## False-Completion Guards
- ...

## Resume Notes
- Current stage:
- Next action:
- Known risks:
```

Keep it executable. Every phase must produce an observable outcome and a concrete check.

## Goal Shape

Default to **one goal with staged phases** when:
- one product objective drives the work
- later phases depend on earlier foundations
- context continuity matters
- end-to-end acceptance is required
- rollback would apply to the whole feature

Use **multiple goals** only when:
- deliverables can ship independently
- acceptance must happen separately
- rollback boundaries differ
- separate agents can work without shared state
- one goal would become too large to resume reliably

Never split only because the plan is long. Split only when product scope, acceptance, or rollback boundaries are real.

## Phase Rules

Prefer 3-7 phases. Each phase should be reviewable in one sitting.

A good phase usually has:
- one primary outcome
- clear dependency boundaries
- one owner for integration
- local verification that can run immediately
- a short phase review before moving on

Typical order:
1. foundations and contracts
2. data model and migrations
3. backend behavior
4. frontend/admin wiring
5. async jobs or integrations
6. polish and acceptance

Change the order only when the spec demands it, and say why.

Split a phase when it:
- has more than 8 substantial tasks
- crosses unrelated subsystems
- requires separate acceptance
- would be hard to review in one sitting

## Delegation Rules

A subtask is delegation-ready only when it has:
- a clear file/module or investigation scope
- one behavioral outcome
- explicit inputs
- expected output format
- local checks
- no open product decision

Do not delegate unresolved architecture choices as implementation tasks. Delegate them as investigation or review tasks instead.

The main agent must always:
- integrate subagent output
- resolve conflicts
- run verification
- update the plan if reality differs
- own final quality

## Review Gates

For complex specs, include a pre-implementation review after spec/plan and before coding:
- review spec and plan together
- find missing scenarios
- find phase-order risks
- find false-completion traps
- find missing tests or acceptance checks
- find places where official or open-source solutions should be used

After each implementation phase, include a lighter review:
- compare delivered behavior to the spec
- run targeted checks
- inspect the diff for scope creep
- decide whether to continue, patch, or ask the owner

## Verification Matrix

Use targeted checks per phase and final end-to-end checks before acceptance.

Include checks relevant to the affected area:
- backend: unit, integration, API tests
- frontend: unit tests, build, browser smoke, screenshots when visual
- admin: type-check, build, route/API behavior
- data: migration, rollback, compatibility, real persistence
- async: outbox, MQ, retry, idempotency
- cache: invalidation and source-of-truth checks
- security: auth, ownership, privacy, rate limit
- docs: current direction, roadmap, accepted deviations

Never mark complete from:
- route exists
- HTTP 200 only
- envelope shape only
- placeholder data
- empty arrays or nulls
- type-check only
- static screenshots without real data

## Human Gates

Record explicit human gates for:
- unresolved product decisions
- non-trivial public UI change, especially when project instructions require explicit approval for layout, copy, visual hierarchy, or interaction deltas
- destructive migration or delete
- external service, cost, or config
- push, PR, merge, deploy
- final manual acceptance

Do not plan to bypass these gates.

## Commit and Resume Strategy

For staged goal-mode work, recommend commits at stable checkpoints:
- after a phase passes verification
- after a review-fix batch passes tests
- before a risky migration or large refactor if useful

Include resume notes that stay short:
- current phase
- completed phases
- next command or check
- known risks
- files likely touched next

When writing `IMPLEMENTATION_PLAN.md`, keep it as the live control document. Update it with phase status, verification results, review-fix notes, and final acceptance notes instead of creating a separate acceptance document by default.

## Anti-Patterns

Avoid:
- file-by-file task lists without behavior
- phases named only by layer when behavior spans layers
- delegating unclear product decisions
- saving all verification for the end
- claiming complete before manual acceptance when user-facing
- using goal mode for independent deliverables that need separate acceptance
- making the plan so long that it becomes the work

## Response Style

Be decisive. If the spec is executable, output the plan. If blocked, ask the smallest number of questions.

When uncertain, prefer one goal with phases, clear review gates, and resume notes.

