# Easy Coding Docs Model

## Root Docs

```text
docs/
  easy-roadmap.md
  current-work.md
  batch/
```

## `docs/easy-roadmap.md`

Long-term product and implementation roadmap.

Track:

- phases
- feature batches
- completion status
- planned follow-ups
- deferred decisions

## `docs/current-work.md`

Current execution state.

Track:

- active feature/batch
- active branch
- current workflow stage
- owner gates still pending
- next action

## Batch Directory

Create a batch directory when a feature starts, not during setup.

```text
docs/batch/batchN-feature-name/
  SPEC.md
  IMPLEMENTATION_PLAN.md
  REVIEW.md
  RETROSPECTIVE.md
  handoff.md
  research.md
```

Required for standard features:

- `SPEC.md`
- `IMPLEMENTATION_PLAN.md`

Create only when useful:

- `REVIEW.md` for subagent review, stage review, or final code review
- `RETROSPECTIVE.md` for high-value lessons
- `handoff.md` when context transfer is needed
- `research.md` when external/open-source research materially shaped the feature

## Acceptance

Do not create `ACCEPTANCE.md` by default.

Record acceptance status inside `IMPLEMENTATION_PLAN.md`, because the plan is the feature's live control document.
