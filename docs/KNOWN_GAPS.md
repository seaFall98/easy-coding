# Known Gaps

These are known follow-ups for `easy-coding` after the initial bootstrap.

## Workflow Gaps

- `easy-coding`, `write-spec`, `goal-plan`, `doc-sync`, and `finish` should be forward-tested together on a real feature before the workflow is considered stable.
- `goal-plan` should be tested against multiple specs, not only the original ZBlog P3 workflow.
- `doc-sync` now knows the easy-coding docs model, but different project layouts still need examples.
- `finish` assumes a GitHub PR workflow and should document alternatives for projects without GitHub.
- `setup-easy-coding` should be tested on both a new repo and an existing repo with legacy roadmap/status docs.
- `to-roadmap` should be tested on both new-project vision input and existing-roadmap migration input.
- The first real adoption should verify that existing project-specific roadmap/status files are reused by default instead of creating duplicate `easy-roadmap.md` / `current-work.md` files.

## Packaging Gaps

- The repository includes a local snapshot of Matt Pocock skills. There is no upstream sync workflow.
- The exact upstream commit for each copied Matt Pocock skill is not recorded yet.
- Some copied skills may benefit from `agents/openai.yaml` metadata if the installer/UI expects richer display data.

## Validation Gaps

- `npx skills add seaFall98/easy-coding --list` detects the 7 default core easy-coding skills from GitHub.
- `npx skills add mattpocock/skills --list` detects the upstream Matt Pocock skills dependency.
- Subagent packaging review found no stale `goal-plan-decomposer` or `finish-feature-dev` references after the rename.
- ZBlog P5 exposed a real forward-test failure: the pipeline reached completion, but review-fix was not performed until the owner prompted, and the workflow did not make the pre-finish local checkpoint commit explicit enough. The `easy-coding`, checklist, and `finish` skills now include mandatory review-fix and checkpoint-commit gates; future fresh-agent tests should pressure these exact failure modes.
- No ZBlog P4 dry run has been completed with the full setup -> grill -> spec -> goal-plan path.
