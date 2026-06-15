# Spec Writing

## Purpose

Turn grill output into a short, executable spec that can be reviewed and planned without guessing.

## Source Rules

- Use confirmed grill decisions first.
- Use repo context and current docs second.
- Use mature or official references third.
- Mark any inference explicitly.
- Never write a guess as a fact.

## Required Sections

Write the spec with these sections:

1. Goal
2. Scope
3. Non-goals
4. Confirmed decisions
5. Open questions
6. User-facing behavior
7. Acceptance criteria
8. Risks or follow-ups

## Review Loop

After drafting:

- Check whether every behavior has a source.
- Check whether any open question blocks planning.
- Check whether the acceptance criteria are observable.
- If the spec is too broad, trim it before planning.

## Stop Conditions

Stop and return to grill when:

- a blocking product decision is still missing
- the spec depends on an unverified assumption
- the behavior cannot be stated clearly enough to test
