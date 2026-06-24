# easy-coding

`easy-coding` is a lightweight AI-driven feature delivery workflow.

It provides one entry skill, `$easy-coding`, that routes a complex web/full-stack feature through the right workflow stage and supporting skills: setup, roadmap, grill, spec draft, goal-mode planning, implementation, verification, review-fix, manual acceptance, and finish.

## Positioning

This project blends Matt Pocock skills, required subagent review, vendored code-review guidance, and practical AI coding workflow patterns from real feature development. It is not a fork of those projects and does not promise upstream sync.

Default installation includes only easy-coding core skills. A local Matt Pocock skills snapshot is kept under `vendor/` for attribution and reference, but it is not installed by default to avoid duplicate skill names. The `review-fix` skill vendors `awesome-skills/code-review-skill` as Markdown reference only. External review tools such as OpenCodeReview are optional and configured separately.

## Install

Prerequisites:

- `npx skills` available in your agent environment
- Matt Pocock skills installed separately
- Subagent tooling available in the agent environment for mandatory review-fix
- OpenCodeReview skill and `ocr` CLI only when you want the optional OCR review path
- `git` for branch workflows
- authenticated `gh` CLI when using `$finish` with GitHub PRs

Install Matt Pocock skills first, then install easy-coding core skills:

```bash
npx skills add mattpocock/skills --all
npx skills add seaFall98/easy-coding --all
```

Configure the optional OpenCodeReview path only when you intend to use OCR:

```bash
npx skills add alibaba/open-code-review --skill open-code-review
npm install -g @alibaba-group/open-code-review
ocr config provider
ocr config model
ocr llm test
```

Do not install this repository with `--full-depth` unless you intentionally want to inspect or install the vendor snapshot. Default installation detects only the 8 core easy-coding skills.

For local testing from this checkout:

```bash
npx skills add /path/to/easy-coding --all
```

## Main Entry

Use:

```text
$easy-coding light ...
$easy-coding standard ...
$easy-coding heavy ...
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
3. Finish confirmation: allow docs sync and PR closeout.

Spec draft, planning, implementation, verification, and review-fix are agent-owned after grill unless a blocking product decision appears.

Internal checkpoints are not owner gates. After grill, the agent should not stop merely because it wrote a spec, wrote a plan, completed a phase, passed a build, or updated docs. Those are commentary/status updates; the next normal owner-facing stop is the manual acceptance handoff.

## Review-Fix

Review-fix is mandatory once before manual acceptance. Easy Coding requires an independent subagent review of the completed implementation using `$review-fix`, and the result must be recorded in `docs/batch/.../REVIEW.md`. If subagent tooling is unavailable, the agent must stop at a blocker instead of handing off for acceptance.

Review-fix normally reviews the current uncommitted/staged working diff before the checkpoint commit. That does not conflict with requiring a commit before manual acceptance: the order is review document, fix, rerun affected checks, then create the local checkpoint commit. OpenCodeReview is optional supporting evidence, but it does not replace the required subagent reviewer.

## Finish

Finish is intentionally lightweight. After manual acceptance, the agent should sync docs/status, merge the latest base branch locally, push the accepted branch, and create or report the PR. It must not start another review or rerun general regressions during finish; conflict resolution means leaving finish mode and getting acceptance again.

## Goal Mode

Goal-mode execution is explicit opt-in, not inferred from ordinary feature work.

Use wording such as:

```text
$easy-coding heavy 创建 goal完成Pipeline ...
$easy-coding heavy goal-mode ...
$easy-coding heavy create a goal after grill and complete the internal pipeline ...
```

When goal-mode is explicitly requested, the agent should finish grill, write the spec and implementation plan, write a concise goal prompt for itself, start the goal with the available goal tool, and continue the Agent-Owned Internal Pipeline until a blocker or manual acceptance handoff. If goal tooling is unavailable, the agent should continue the same pipeline without goal mode rather than handing the task back to the owner.

## Included Skills

See `skills/` for installed core skills and `vendor/` for the non-installed Matt Pocock snapshot.

## Maintenance

See `docs/KNOWN_GAPS.md` for current follow-up work and validation gaps.
