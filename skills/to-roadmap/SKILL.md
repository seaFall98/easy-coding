---
name: to-roadmap
description: Create or migrate an easy-coding roadmap after owner grilling. Use when a new project needs docs/easy-roadmap.md, an existing project has legacy roadmap/PRD/planning docs to normalize, or setup-easy-coding finds no accepted roadmap source.
---

# To Roadmap

Create `easy-roadmap.md` only after owner intent and project layout are clear.

Do not invent a roadmap from thin air. Grill first.

## Inputs

Accept any of:

- owner's project vision
- existing roadmap
- PRD or product plan
- technical plan
- current codebase analysis
- research notes

## Required Intake

Before writing, confirm:

- workspace root
- source git root
- docs root
- target product/project
- roadmap time horizon
- first useful feature phase
- known non-goals
- release or acceptance style

If these are unknown, ask one blocking question at a time.

## Output Path

Default path:

```text
<docs-root>/easy-roadmap.md
```

If owner gives another path, use it.

## Roadmap Shape

Use this structure:

```markdown
# Easy Roadmap

## Product Direction

## Current Architecture / Starting Point

## Phase Plan

### Phase N: ...
- Goal:
- Scope:
- Non-goals:
- Depends on:
- Acceptance signal:
- Follow-ups:

## Backlog

## Deferred Decisions
```

## Migration Mode

When migrating existing docs:

- preserve the original file
- extract durable roadmap decisions
- avoid copying stale implementation details
- record source files used
- flag conflicts instead of silently resolving them

## Handoff

After writing or updating the roadmap:

- update or request update of `current-work.md`
- recommend the first batch directory only when the owner is ready to start a feature
- route feature execution back to `easy-coding`
