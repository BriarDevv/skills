# Eval 03: unrecorded decision blocks completion

## Query

"Listo, me gusta como quedó el empty state nuevo, cerrá la tarea."

## Fixture

During the session a new empty-state pattern (icon + one-line hint + primary
action) was designed for `catalogo/inventario`; `## Decisions` has no entry
for it yet.

## Expected behavior

- [ ] Completion is NOT claimed while the decision is unrecorded.
- [ ] Appends `- YYYY-MM-DD — <decision>` (today's date) under
      `### catalogo/inventario`, one line.
- [ ] Only then reports done, mentioning the recorded entry.
