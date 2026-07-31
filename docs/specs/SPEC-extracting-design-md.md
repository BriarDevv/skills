# SPEC: extracting-design-md

Date: 2026-07-30
Status: Approved (design validated interactively; approach A + convergence
loop + bounded scaling per owner)

## Purpose

Nothing today produces a DESIGN.md from an already-built project:
`context-init` offers it for new repos, `designing-consistently` maintains it
after it exists. This skill closes the gap: reverse-engineer the design
system a codebase actually uses, report its drift objectively, and hand a
migration plan to `designing-consistently` — iterating until the drift count
converges to zero ("pixel perfect" as a measured state, not a feeling).

## Decisions (fixed)

- **Skill-only (approach A).** No new scripts: harvesting is judgment work
  over Grep/Read; the objective gates already exist (design-md-gen +
  context-lint design checks in Context-Engineering).
- **Scope: extract + report + plan.** The skill writes DESIGN.md and its
  generated tokens; it NEVER rewrites application code. Code migration is a
  separate, approved step executed under `designing-consistently`.
- **Workflow:** (1) inventory style sources (stylesheets/@theme, tailwind
  config, component classes; monorepo → per app); (2) harvest + cluster
  values with occurrence counts and file:line evidence; (3) drift report —
  near-duplicates, outliers, competing component patterns, each with counts
  and evidence; (4) propose tokens by semantic role + frequency — drift
  variants are NOT consecrated as tokens, they become migration targets;
  owner confirms naming/palette intent; (5) backfill `## Decisions` per
  surface from code evidence only — contradictory patterns become open
  questions for the owner, never invented resolutions; (6) write DESIGN.md →
  generate → context-lint design checks PASS; (7) migration plan in
  per-surface batches, effort-sized, zero code edits — hand off to
  `designing-consistently`.
- **Convergence loop (the "pixel perfect" mechanism).** The drift report's
  finding count is the metric. After each migration batch executes, re-run
  the harvest on the touched scope: the count must go DOWN; done = zero
  findings + lint PASS. Re-runs are idempotent — extracting from a clean
  repo yields no new findings. The description covers re-audit as a trigger
  so the loop re-enters naturally.
- **Bounded scaling ("sin irse a la mierda").** Read-only harvesting may fan
  out (one subagent per app or major section) with a single-context
  consolidation; writes NEVER parallelize across surfaces that share
  components; one batch completes its gates (generate + lint green) before
  the next starts; propose a Workflow/ultracode fan-out only when the
  surface count makes serial work impractical (roughly >15 surfaces) and the
  user opts in — the harness requires that opt-in anyway.
- **Description:** pushy on triggers (skill-creator guidance: Claude
  undertriggers) without enumerating workflow steps (writing-skills
  guidance: process summaries become shortcuts). Triggers include "the UI
  looks inconsistent" phrasing, multiplied values, adoption, and post-batch
  re-audit.
- **Authoring method: both lenses.** writing-skills (Iron Law, evals first,
  pressure scenarios, form-matches-failure) + skill-creator (paired
  with-skill AND baseline runs launched the same turn, objectively
  verifiable assertions, explain-why over heavy MUSTs). The benchmark
  viewer/aggregation tooling is not installed — paired runs are graded
  manually against the assertions; noted, not hidden.
- Lives in the `skills` repo; cross-references Context-Engineering
  (`reference/design-md.md`, template, generator) and
  `designing-consistently` by name.

## Evals (written before skill content)

1. **Drift detection:** fixture app with 3 button radii and 7 grays →
   report finds both families with counts + file:line evidence; proposes a
   collapsed token set (not one token per variant) and lists the variants as
   migration targets.
2. **Faithful backfill:** a pattern consistent across views (back button in
   3 pages) → recorded as a dated Decision with evidence; a contradictory
   pattern (2 card styles for the same data) → open question for the owner,
   no invented resolution.
3. **Gates + hands off code:** produced DESIGN.md passes generator +
   context-lint design checks; the migration plan contains zero code edits
   and states the drift count as the convergence metric.

## Verification

- Paired subagent runs (baseline vs with-skill) on a drifted fixture, same
  turn; assertions graded manually; failures feed skill revisions
  (evals change first when reality wins).
- skills repo lint PASS; README table row added.
- First real run = KioscoDiagonal pilot (sub-project 2 dogfooding).
