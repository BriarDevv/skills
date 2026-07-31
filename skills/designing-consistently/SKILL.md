---
name: designing-consistently
description: Keeps UI work consistent with an app's DESIGN.md design system and decision log. Use when building or modifying UI in a repo with a DESIGN.md (or one that keeps drifting without it), when new screens must match existing ones, when buttons or patterns come out different every session, or when design decisions get lost between sessions.
---

# Designing consistently

UI drift has two sources: styles invented in-session instead of consumed
from the system, and decisions that live only in conversation memory.
DESIGN.md is a **living file** — honoring it is half the loop; writing back
is the other half, and it is the half that gets skipped.

## Workflow

Copy this checklist and tick items off:

```
Consistency progress:
- [ ] 1. Locate the app's DESIGN.md
- [ ] 2. Read tokens + Decisions for the target surfaces
- [ ] 3. Build consuming the system
- [ ] 4. Record decisions (gate)
- [ ] 5. Verify
```

**1. Locate.** Single-app repo: root DESIGN.md. Monorepo: the app's own
DESIGN.md next to its code. Missing? Offer to instantiate it first
(Context-Engineering `templates/repo/DESIGN.md.template`) — designing
without it just recreates the drift.

**2. Read.** From the frontmatter: the tokens and components the work will
need. From `## Decisions`: every entry under the surfaces (routes/screens)
about to be touched — these are standing decisions, not suggestions.

**3. Build.** Styles come from the generated `design.tokens.css`, never raw
values. Reuse an existing component pattern when one fits; a genuinely new
pattern is born tokenized: values added to DESIGN.md frontmatter, then
`node scripts/design-md-gen.mjs` regenerates. The line: one-off micro-layout
offsets (a 22px nudge, a hairline width) may stay inline, but new **colors,
type sizes, radii, and shadows** always go through the frontmatter — a
`text-[13px]` is a type-scale escape, not a nudge. A standing decision the
work conflicts with is renegotiated with the user — never silently
overridden.

**4. Record (gate).** Work is not complete while an unrecorded decision
exists. Recordable means: a new pattern (a timeline rail, an empty-state
shape), a changed presentation of an existing decision (the back button
moving into a sticky header), or a layout choice a future session could
contradict (two-column grid, card-based data). Each gets
`- YYYY-MM-DD — <decision>` under its `### <surface>`. Honoring the
existing entries does NOT satisfy this gate — new decisions were made the
moment the page looks different than before.

**5. Verify.** Regenerate tokens and run context-lint (design checks must
pass). Re-check each touched surface against its Decisions entries.
Screenshot the result when the environment allows it.

## Judgment notes

- Consistency beats novelty by default; when the brief explicitly asks for
  a new direction, update DESIGN.md first, then build.
- Decisions entries are one line each — longer rationale belongs in the
  prose sections, referenced from the entry.
- If DESIGN.md and the code disagree, say so and ask which is right —
  never pick silently.
- "The task didn't mention DESIGN.md" and "I'm in a hurry" are the two ways
  drift happens; neither skips steps 2 or 4.
