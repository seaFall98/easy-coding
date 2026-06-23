# Publishing Notes

## Positioning

`easy-coding` is a lightweight AI-driven feature delivery workflow that combines Matt Pocock skills, OpenCodeReview, and our own project practices.

## Packaging

- Default installation includes only the easy-coding core skills in `skills/`.
- Matt Pocock skills are external prerequisites installed from `mattpocock/skills`.
- OpenCodeReview is an external prerequisite and the preferred default review-fix reviewer. Install its skill plus `ocr` CLI from `alibaba/open-code-review`; do not vendor it into this repository. The Codex plugin is optional because automatic Easy Coding review-fix invokes the CLI directly.
- Keep the Matt Pocock snapshot under `vendor/` as non-installed attribution/reference material.
- Do not duplicate external skill names in the default install set.
- Keep attribution explicit in `ATTRIBUTION.md`.
- Do not promise automatic upstream sync.

## Maintenance

- Update the workflow as the repository evolves.
- Keep the main `SKILL.md` short.
- Move examples, routing hints, and recurring patterns into references.
