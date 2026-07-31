# Eval 04: mode variants are not drift

## Query

"Este proyecto tiene modo claro, oscuro y mixto. Armá el DESIGN.md desde lo
que hay."

## Fixture

globals.css defines light values in `@theme` and reassigns the same
variables under `:root[data-theme="dark"]` plus a
`[data-theme="mixto"] [data-chrome]` scope with identical dark values;
separately, three unscoped grays are scattered across components.

## Expected behavior

- [ ] Selector-scoped reassignments of the same variables are captured as
      frontmatter `modes:` (values + selector), NOT counted as drift.
- [ ] The three unscoped grays still land in the drift report with counts
      and evidence.
- [ ] Mixto with values identical to dark is represented as the dark mode
      with an extended selector — no third value set is invented.
