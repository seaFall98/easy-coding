---
name: setup-easy-coding
description: Initialize an existing project for the easy-coding workflow. Use when adopting easy-coding in a repo, creating the standard docs model, setting up roadmap/current-work/batch conventions, or wiring Matt Pocock skills setup into the project.
---

# Setup Easy Coding

Initialize a project so `$easy-coding` knows where to put and find workflow documents.

## Workflow

1. Inspect existing project instructions:
   - `AGENTS.md`
   - `CLAUDE.md`
   - `docs/README.md`
   - existing roadmap/current status files
2. Route to `setup-matt-pocock-skills` if the project has not already configured issue tracker, triage labels, and domain docs.
3. Create missing easy-coding docs:
   - `docs/easy-roadmap.md`
   - `docs/current-work.md`
   - `docs/batch/`
4. Do not migrate old docs unless the user explicitly asks.
5. If legacy docs exist, report the suggested mapping instead of rewriting them by default.

## Standard Docs

Read `references/docs-model.md` before creating or updating files.

## Migration Policy

Default mode is additive:

- create missing docs
- preserve existing docs
- report suggested renames or migrations

Explicit migration mode may map:

- `V2_ROADMAP_V2.md` or similar -> `docs/easy-roadmap.md`
- `CURRENT_DIRECTION.md` or similar -> `docs/current-work.md`

Do not delete legacy files unless the user explicitly asks.

## Output

Report:

- files created
- existing files reused
- suggested migrations
- whether `setup-matt-pocock-skills` was run or should be run next
