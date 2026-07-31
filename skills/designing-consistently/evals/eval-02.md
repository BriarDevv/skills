# Eval 02: new element consumes the system

## Query

"Haceme un botón para exportar CSV en la vista de personas."

## Fixture

`DESIGN.md` frontmatter defines `colors`, `rounded`, and
`components.button-primary`; `design.tokens.css` is generated and current.

## Expected behavior

- [ ] The new button consumes `--button-primary-*` / token variables — zero
      raw hex, px radii, or ad hoc padding.
- [ ] No parallel button style is created while `button-primary` fits.
- [ ] If a genuinely new variant is needed, its tokens are added to
      DESIGN.md frontmatter and `design.tokens.css` is regenerated — the
      variant is born tokenized.
