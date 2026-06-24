# Publishing Notes

## Positioning

`easy-coding` is a lightweight AI-driven feature delivery workflow that combines Matt Pocock skills, required subagent review, and our own project practices.

## Packaging

- Default installation includes only the easy-coding core skills in `skills/`.
- Matt Pocock skills are external prerequisites installed from `mattpocock/skills`.
- Subagent review is the mandatory Easy Coding review-fix reviewer role. The `review-fix` skill vendors `awesome-skills/code-review-skill` as Markdown reference only. OpenCodeReview is optional external support.
- Keep the Matt Pocock snapshot under `vendor/` as non-installed attribution/reference material.
- Do not duplicate external skill names in the default install set.
- Keep attribution explicit in `ATTRIBUTION.md`.
- Do not promise automatic upstream sync.

## Maintenance

- Update the workflow as the repository evolves.
- Keep the main `SKILL.md` short.
- Move examples, routing hints, and recurring patterns into references.
