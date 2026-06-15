# Known Gaps

These are known follow-ups for `easy-coding` after the initial bootstrap.

## Workflow Gaps

- `easy-coding`, `write-spec`, `goal-plan`, `doc-sync`, and `finish` should be forward-tested together on a real feature before the workflow is considered stable.
- `goal-plan` should be tested against multiple specs, not only the original ZBlog P3 workflow.
- `doc-sync` now knows the easy-coding docs model, but different project layouts still need examples.
- `finish` assumes a GitHub PR workflow and should document alternatives for projects without GitHub.
- `setup-easy-coding` should be tested on both a new repo and an existing repo with legacy roadmap/status docs.

## Packaging Gaps

- The repository includes a local snapshot of Matt Pocock skills. There is no upstream sync workflow.
- The exact upstream commit for each copied Matt Pocock skill is not recorded yet.
- Some copied skills may benefit from `agents/openai.yaml` metadata if the installer/UI expects richer display data.

## Validation Gaps

- Local `npx skills add ... --list --full-depth` detects the skills.
- No fresh-agent forward test has been run against `$easy-coding` yet.