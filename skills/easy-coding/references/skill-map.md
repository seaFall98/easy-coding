# Skill Map

## Routing

- `grill-with-docs`: unclear requirements, contested terminology, risky domain decisions, or cases where mature solutions may already exist.
- `write-spec`: grill is complete and a spec draft needs to be synthesized from the confirmed decisions.
- `goal-plan`: grill is complete and the accepted spec or roadmap needs a goal-mode execution plan.
- goal-mode start: only after explicit owner wording such as `/goal`, `goal mode`, `create a goal`, `创建 goal`, or `完成Pipeline`; after opt-in, the agent writes the goal prompt and starts the goal instead of asking the owner to do it manually. Example: `/easy-coding heavy 创建 goal完成Pipeline`.
- `diagnose`: reproducible bug, regression, manual acceptance failure, or unknown failure mode.
- `tdd`: behavior is best driven by tests and the implementation can be safely grown from them.
- `review-fix`: implementation is complete and needs the mandatory pre-acceptance subagent review plus `REVIEW.md`.
- `doc-sync`: run inside finish after owner confirms the feature can close.
- `finish`: owner confirms finish and the branch is ready for closure, merge, or cleanup.

## Owner Gates

- Grill: owner answers only the questions needed to remove guessing.
- Manual acceptance: owner checks delivered behavior.
- Finish confirmation: owner confirms closeout.

Everything between grill and manual acceptance is agent-owned unless a blocking decision appears.

Internal checkpoints are not owner gates. Spec written, plan written, phase complete, build passed, or docs updated are not reasons to final-answer and wait. Report them in commentary and continue until a real human gate, blocker, or acceptance handoff.

## Tier Hints

- `light`: small fix, copy, style, or contained behavior change.
- `standard`: one feature or one workflow with a clear spec and plan.
- `heavy`: multi-stage feature, cross-system work, migration, middleware, or complex UI/state.
- Legacy aliases `a-light`, `aa-standard`, and `aaa-heavy` are accepted for compatibility, but owner-facing examples should use `light`, `standard`, and `heavy`.

## When to Grill

Grill before planning when the work touches solved domains such as auth, mail, notifications, personal center, media playback, rich text, charts, payments, uploads/COS, scheduling, MQ, Redis, or any area where an official or mature open-source solution is likely better than a custom guess.

## TDD Fit

Use `tdd` for testable behavior, not only backend work.

Good fits:

- backend APIs and domain logic
- permissions and ownership checks
- state machines
- data migration behavior
- async delivery and retry behavior
- frontend hooks, reducers, state transitions, validators, routing guards, and data transforms

Poor fits:

- pure visual polish
- layout taste
- animation feel
- subjective UI aesthetics

For frontend work, test logic and user-observable behavior. Do not force TDD onto visual design polish.
