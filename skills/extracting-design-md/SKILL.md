---
name: extracting-design-md
description: Reverse-engineers a DESIGN.md (Google Labs format) from an already-built project and measures its design drift. Use whenever an existing codebase should adopt DESIGN.md, when UI values have multiplied (several grays, mixed radii, inconsistent buttons), when the owner says the UI looks inconsistent or "quedó desprolijo", or to re-audit drift after a migration batch — even if nobody says the word DESIGN.md.
---

# Extracting DESIGN.md

Turns the design system a codebase *actually uses* into a DESIGN.md the
tooling can enforce. Output format matters as much as analysis quality: a
freeform writeup can be excellent and still unenforceable — only the spec
format (YAML frontmatter tokens + prose sections + `## Decisions`) plugs
into the generator, the lint gates, and `designing-consistently`. The
convention lives in Context-Engineering `reference/design-md.md`.

## Workflow

Copy this checklist and tick items off:

```
Extraction progress:
- [ ] 1. Inventory style sources
- [ ] 2. Harvest and cluster values
- [ ] 3. Drift report
- [ ] 4. Propose tokens (owner confirms naming)
- [ ] 5. Backfill Decisions
- [ ] 6. Write DESIGN.md → generate → lint
- [ ] 7. Migration plan + hand-off
```

**1. Inventory.** Stylesheets/@theme, tailwind config, and the classes and
inline styles components actually use. Monorepo: one extraction per app.

**2. Harvest.** Grep the values by family (colors, radii, spacing, type,
shadows) with occurrence counts and file:line locations — counts are what
make the report objective instead of vibes.

**3. Drift report.** Near-duplicates, outliers, competing patterns; each
family with its variant count, evidence, and the surfaces it touches. Order
by spread (surfaces affected), not by discovery order. Theme modes are not
drift: a selector-scoped reassignment of the same variable
(`[data-theme="dark"] { --paper: … }`) is an intentional mode — it goes to
frontmatter `modes:` (values + selector, per the convention). Drift is the
unscoped, scattered variation. A mode whose values duplicate another's is
the same mode with a widened selector, not a new value set.

**4. Propose tokens.** Semantic role + frequency decide: the dominant or
correct variant becomes the token; the other variants become migration
targets, NOT tokens — 7 grays in the code is one `ink-muted` token plus six
rows in the plan. Confirm names and palette intent with the owner before
writing the file.

**5. Backfill Decisions.** Read each surface. A pattern the code evidences
consistently (the same back button in three views) becomes a dated entry
under its `### <surface>`. A pattern the code CONTRADICTS (two card styles
for the same data) is the owner's call, not yours: record it as an open
question with both variants and their locations. Deciding it yourself hides
exactly the inconsistency the owner asked you to surface.

**6. Write + gates.** DESIGN.md in the spec format: frontmatter tokens
(quoted hex), prose sections in spec order, `## Decisions`. Then run
`design-md-gen` and `context-lint` from the Context-Engineering clone — the
design checks passing is the definition of "the file is real", not an
optional polish.

**7. Migration plan + hand-off.** Per-surface batches, effort-sized, with
exact find→replace rows per fix (mechanical to execute), and ZERO code edits
in this skill. Execution belongs to `designing-consistently` — its
read-consume-record loop takes over from here. State the metric: re-run the
harvest after each batch; the drift finding count must go down; done at
zero findings + lint PASS. "Pixel perfect" is a measurement, not a feeling.

## Scaling (bounded)

Harvesting is read-only and may fan out — one subagent per app or major
section — consolidated in a single context. Writes never parallelize across
surfaces that share components; a batch finishes its gates before the next
starts. A Workflow/ultracode fan-out is worth proposing only past roughly
15 surfaces, and only with the owner's opt-in.

## Judgment notes

- Re-runs are the point, not a smell: extraction on a clean repo yields zero
  findings — that idempotence is what makes the drift count a metric.
- If the repo has no build to verify against, say so in the plan — never
  silently skip a verification step.
- The report speaks to the owner: severity comes from how much of the UI a
  family touches, and every claim carries its evidence.
