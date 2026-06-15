---
name: write-spec
description: Turn confirmed grill results, repo context, and accepted research into a concise executable SPEC.md for feature delivery. Use after requirements have been grilled and before goal-mode planning, especially when easy-coding needs a spec draft source for goal-plan.
---

# Write Spec

Turn grill output into a short, executable spec that can be planned and implemented without guessing.

Use the normalized spec format in `references/spec-template.md`.
If the spec is nuanced or the work is high-risk, use `references/spec-document-reviewer-prompt.md` to dispatch a separate reviewer pass after the draft is written.

## Inputs

Use these sources, in this order:

1. confirmed grill decisions
2. repo context and current docs
3. accepted research from official or mature references
4. explicit assumptions marked as assumptions

Never write an unconfirmed guess as a fact.

If grill results are still missing a blocking decision, ask the smallest blocking question and stop.

## Output

Write `SPEC.md` or the path requested by the user. If the project uses easy-coding docs, write to `docs/batch/batchN-feature-name/SPEC.md`. If no file is requested and no docs model exists, output the spec in chat.

Prefer a spec that includes:

- goal and user value
- problem context
- scope and non-goals
- confirmed decisions and assumptions
- approaches considered, when there was a real choice
- user-facing behavior
- edge cases and failure modes
- acceptance criteria
- risks and follow-ups
- open questions, if any

Keep the spec scoped enough to plan in one pass. If it is too broad, split it before planning.

## Review Loop

Before handing the spec to planning:

- Check whether every behavior has a source.
- Check whether the structure is crisp and sectioned, not a wall of prose.
- Check whether the selected approach is justified against alternatives, when alternatives existed.
- Check whether any open question blocks planning.
- Check whether acceptance criteria are observable.
- Check whether terminology matches the project docs and grill decisions.
- Trim scope if the spec is too broad.
- Fix placeholders, contradictions, and ambiguous wording inline.
- If the spec is substantial, run the reviewer prompt against the draft and fold in the useful findings.

If the caller wants explicit review, stop after writing the spec and ask them to review it before planning. Otherwise, continue once the spec is self-consistent and unblocked.

## Stop Conditions

Stop and return to grill when:

- a blocking product decision is still missing
- the spec depends on an unverified assumption
- the behavior cannot be stated clearly enough to test

If no stop condition remains, treat the spec as the accepted execution source for planning.

## Handoff

When the spec is clean, hand it to `goal-plan` for goal-mode planning.
