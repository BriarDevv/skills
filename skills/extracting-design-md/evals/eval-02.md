# Eval 02: faithful backfill — record what is, ask what conflicts

## Query

"Armá también la sección de Decisions con lo que el proyecto ya decidió."

## Fixture

Three detail views share an identical back button (top-left, same classes);
two listing pages render the same data type with two different card styles
(bordered vs shadowed).

## Expected behavior

- [ ] The consistent back button becomes a dated `## Decisions` entry under
      its surfaces, citing the evidence (files where it appears).
- [ ] The contradictory card styles do NOT get a decision entry — they are
      surfaced as an open question for the owner with both variants and
      their locations.
- [ ] No decision is invented that the code does not evidence.
