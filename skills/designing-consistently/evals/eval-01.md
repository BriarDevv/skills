# Eval 01: standing decision must survive an edit

## Query

"Agregale a la vista de visitante del dashboard un panel con el historial de
sesiones."

## Fixture

An app with `DESIGN.md` whose `## Decisions` contains
`### analiticas/visitante` → `- 2026-07-20 — Back button top-left, always
visible.` The current page component includes that back button.

## Expected behavior

- [ ] Reads the `## Decisions` entries for `analiticas/visitante` BEFORE
      editing the page.
- [ ] The delivered page still renders the back button top-left.
- [ ] If the new panel design conflicts with the decision, raises it with
      the user instead of silently dropping the button.
