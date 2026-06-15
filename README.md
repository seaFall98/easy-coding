# easy-coding

`easy-coding` is a lightweight AI-driven feature delivery workflow.

It provides one entry skill, `$easy-coding`, that routes a complex web/full-stack feature through the right workflow stage and supporting skills: setup, roadmap, grill, spec draft, goal-mode planning, implementation, verification, review-fix, manual acceptance, and finish.

## Positioning

This project blends Matt Pocock skills with practical AI coding workflow patterns from real feature development. It is not a fork of Matt Pocock skills and does not promise upstream sync.

Default installation includes only easy-coding core skills. A local Matt Pocock skills snapshot is kept under `vendor/` for attribution and reference, but it is not installed by default to avoid duplicate skill names.

## Install

Prerequisites:

- `npx skills` available in your agent environment
- Matt Pocock skills installed separately
- `git` for branch workflows
- authenticated `gh` CLI when using `$finish` with GitHub PRs

Install Matt Pocock skills first, then install easy-coding core skills:

```bash
npx skills add mattpocock/skills --all
npx skills add seaFall98/easy-coding --all
```

Do not install this repository with `--full-depth` unless you intentionally want to inspect or install the vendor snapshot. Default installation detects only the 7 core easy-coding skills.

For local testing from this checkout:

```bash
npx skills add /path/to/easy-coding --all
```

## Main Entry

Use:

```text
$easy-coding a-light ...
$easy-coding aa-standard ...
$easy-coding aaa-heavy ...
```

If no tier is supplied, `$easy-coding` chooses the smallest viable workflow.

## Docs Model

For projects adopting the full workflow, run `$setup-easy-coding` first. It creates or completes:

```text
docs/
  easy-roadmap.md
  current-work.md
  batch/
```

Each feature creates its own `docs/batch/batchN-feature-name/` directory with `SPEC.md` and `IMPLEMENTATION_PLAN.md` as the required live documents.

## Roadmap Creation

`setup-easy-coding` does not invent a roadmap. For existing projects, the owner should identify the accepted global plan, roadmap, PRD, or strategy document to migrate. For new projects, run `$to-roadmap` after a project intake grill.

## Owner Gates

Owners normally participate in only three gates:

1. Grill: clarify requirements and decisions.
2. Manual acceptance: verify the delivered behavior.
3. Finish confirmation: allow docs sync and branch closeout.

Spec draft, planning, implementation, verification, and review-fix are agent-owned after grill unless a blocking product decision appears.

## Included Skills

See `skills/` for installed core skills and `vendor/` for the non-installed Matt Pocock snapshot.

## Maintenance

See `docs/KNOWN_GAPS.md` for current follow-up work and validation gaps.
