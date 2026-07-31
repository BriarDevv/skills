# Eval 01: drift detected with counts and evidence

## Query

"che, el diseño de este proyecto quedó re inconsistente — cada botón es
distinto y hay como mil grises. Armá el DESIGN.md desde lo que ya hay y
decime qué tan desprolijo está."

## Fixture

An app with no DESIGN.md, 3 button radius treatments and 7 distinct gray
values spread across pages (hex, rgb(), and Tailwind gray-*).

## Expected behavior

- [ ] The drift report names both families with occurrence counts and at
      least one file:line evidence pointer per family.
- [ ] The proposed token set collapses the variants (fewer gray tokens than
      raw gray values); the non-consecrated variants appear as migration
      targets, not as tokens.
- [ ] Severity/order in the report follows spread (how many surfaces are
      affected), not discovery order.
