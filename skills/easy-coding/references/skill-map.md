# Skill Map

## Routing

- `grill-with-docs`: unclear requirements, contested terminology, risky domain decisions, or cases where mature solutions may already exist.
- `write-spec`: grill is complete and a spec draft needs to be synthesized from the confirmed decisions.
- `goal-plan`: grill is complete and the accepted spec or roadmap needs a goal-mode execution plan.
- `diagnose`: reproducible bug, regression, manual acceptance failure, or unknown failure mode.
- `tdd`: behavior is best driven by tests and the implementation can be safely grown from them.
- `doc-sync`: run inside finish after owner confirms the feature can close.
- `finish`: owner confirms finish and the branch is ready for closure, merge, or cleanup.

## Owner Gates

- Grill: owner answers only the questions needed to remove guessing.
- Manual acceptance: owner checks delivered behavior.
- Finish confirmation: owner confirms closeout.

Everything between grill and manual acceptance is agent-owned unless a blocking decision appears.

## Tier Hints

- `a-light`: small fix, copy, style, or contained behavior change.
- `aa-standard`: one feature or one workflow with a clear spec and plan.
- `aaa-heavy`: multi-stage feature, cross-system work, migration, middleware, or complex UI/state.

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
