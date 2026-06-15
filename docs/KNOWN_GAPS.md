# Known Gaps

These are known follow-ups for `easy-coding` after the initial bootstrap.

## Workflow Gaps

- `easy-coding` routes to `write-spec`, but the spec-writing flow should be forward-tested on a real feature before being considered stable.
- `goal-plan-decomposer` is a local adaptation and should be tested against multiple specs, not only the original ZBlog P3 workflow.
- `doc-sync` is generic now, but different projects may need examples for common doc layouts.
- `finish-feature-dev` assumes a GitHub PR workflow and should document alternatives for projects without GitHub.

## Packaging Gaps

- The repository includes a local snapshot of Matt Pocock skills. There is no upstream sync workflow.
- The exact upstream commit for each copied Matt Pocock skill is not recorded yet.
- Some copied skills may benefit from `agents/openai.yaml` metadata if the installer/UI expects richer display data.

## Validation Gaps

- Local `npx skills add ... --list --full-depth` detects the skills.
- No end-to-end install from GitHub has been run after each change.
- No fresh-agent forward test has been run against `$easy-coding` yet.