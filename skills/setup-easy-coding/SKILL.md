---
name: setup-easy-coding
description: Initialize an existing project for the easy-coding workflow. Use when adopting easy-coding in a repo, creating the standard docs model, setting up roadmap/current-work/batch conventions, or wiring Matt Pocock skills setup into the project.
---

# Setup Easy Coding

Initialize a project so `$easy-coding` knows where source code lives, where workflow docs live, and how feature batches should be organized.

## Workflow

1. Run a Project Intake Grill before creating files.
2. Confirm:
   - workspace root
   - source git root
   - docs root
   - roadmap source or roadmap creation path
   - branch/PR/acceptance workflow
3. Inspect existing project instructions:
   - `AGENTS.md`
   - `CLAUDE.md`
   - `docs/README.md`
   - existing roadmap/current status files
4. Check whether Matt Pocock skills are installed. If missing, stop and tell the owner to install them; do not create duplicate local copies.
5. Route to `setup-matt-pocock-skills` if the project has not already configured issue tracker, triage labels, and domain docs.
6. Create missing easy-coding docs only when no authoritative project-specific equivalents exist:
   - `docs/easy-roadmap.md`
   - `docs/current-work.md`
   - `docs/batch/`
7. If existing agent instructions or docs indexes name project-specific roadmap/current-work files, reuse those files by default and record the mapping. Do not create duplicate `easy-roadmap.md` or `current-work.md` unless the owner explicitly approves canonical migration.
8. Do not migrate old docs unless the user explicitly asks.
9. If no roadmap source exists, route to `to-roadmap` after intake grill.
10. If legacy docs exist, report the suggested mapping instead of rewriting them by default.

## Matt Skills Dependency

`easy-coding` depends on Matt Pocock skills by name, including `grill-with-docs`, `diagnose`, `tdd`, `to-prd`, `to-issues`, `triage`, and related skills.

Do not install duplicate copies from this repository. The Matt snapshot in `vendor/` is for reference only.

## Defaults

Default to an outer workspace docs model when the project appears to have a workspace root containing one or more source repos:

```text
workspace-root/
  docs/
  source-repo/
```

In this model:

- docs root: `workspace-root/docs`
- source git root: `workspace-root/source-repo`

If the current directory is a single source repo with no obvious outer workspace, default to `source-repo/docs`.

Owner confirmation overrides all defaults.

## Standard Docs

Read `references/docs-model.md` before creating or updating files.

The docs model defines default names, not mandatory names. Project-specific files named by `AGENTS.md`, `CLAUDE.md`, or `docs/README.md` remain authoritative unless the owner chooses migration.

## Migration Policy

Default mode is additive:

- create missing docs
- preserve existing docs
- reuse existing authoritative roadmap/status files when present
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
- confirmed workspace/source/docs roots
- whether `setup-matt-pocock-skills` was run or should be run next
