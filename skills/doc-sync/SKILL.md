---
name: doc-sync
description: Keep project documentation synchronized after decisions, acceptance, merges, or feature closeout. Use when a feature is accepted, a roadmap/spec/plan changed, a branch is finished, or the user asks to sync docs.
---

# Doc Sync

Keep the project's active documentation aligned with the actual work state.

## When to Run

Run after:

- the owner confirms an important decision
- a spec, plan, or roadmap changes
- manual acceptance passes
- a feature branch is ready to finish
- a PR merges or the branch closes
- the user explicitly asks to sync docs

## Discover the Documentation Model

Before editing, inspect the repository's own instructions and docs index:

1. `AGENTS.md`, `CLAUDE.md`, or equivalent agent instructions
2. `docs/README.md` or docs index
3. `docs/easy-roadmap.md`, or roadmap files such as `ROADMAP.md`, `docs/roadmap*.md`, or project-specific equivalents named by the project
4. `docs/current-work.md`, or active direction/status files such as `CURRENT_DIRECTION.md`, `STATUS.md`, or active batch docs named by the project
5. `docs/batch/batchN-feature-name/IMPLEMENTATION_PLAN.md` for the active feature
6. `CONTEXT.md` and `docs/adr/` when domain language or architecture decisions changed

Do not assume any project-specific layout. Use the files that actually exist.

## What to Sync

- Current phase, active branch, active feature, or completed work status
- Confirmed workspace root, source git root, docs root, and active batch path
- Roadmap task state when acceptance or merge changes it
- Spec/plan changes when grill decisions alter scope
- Active batch `IMPLEMENTATION_PLAN.md` phase status, verification notes, review-fix notes, and acceptance notes
- ADRs only for hard-to-reverse, surprising, trade-off decisions
- Context glossary only for stable domain terms, not implementation notes
- Follow-up work when a decision is intentionally deferred
- If both easy-coding default docs and project-specific docs exist, prefer the authoritative files named by project instructions and record any mapping instead of updating duplicates blindly.

## Editing Rules

- Prefer minimal edits over broad rewrites.
- Preserve the project's existing documentation style.
- Do not create new documentation systems unless the project has none and the user needs one.
- Do not mark work complete unless real acceptance or merge state supports it.
- Keep implementation facts in docs, not memory, when the repository has a docs system.

## Output

Report only:

- files updated
- what changed in each file
- skipped docs, if any, and why
