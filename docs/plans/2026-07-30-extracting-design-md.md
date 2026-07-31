# extracting-design-md Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: superpowers:executing-plans (inline). Authoring method: superpowers:writing-skills (Iron Law, evals first, baseline before content) + skill-creator:skill-creator (paired baseline/with-skill runs, objective assertions, benchmark, eval viewer for the human). Spec: `docs/specs/SPEC-extracting-design-md.md` — its Decisions section is the content contract.

**Goal:** Author `skills/extracting-design-md/` (SKILL.md + 3 evals) proven by a graded baseline/with-skill comparison on a deliberately drifted fixture app.

**Method reconciliation:** writing-skills orders the loop (baseline observed BEFORE skill content is written — deviating from skill-creator's same-turn paired launch, which optimizes wall-clock, not validity; the benchmark delta is unaffected). skill-creator supplies the measurement rig: workspace iterations, `evals.json`, `eval_metadata.json`, `grading.json` (fields `text`/`passed`/`evidence`), `python -m scripts.aggregate_benchmark`, analyst pass, `generate_review.py --static` so the owner reviews outputs BEFORE self-directed revisions.

## Global Constraints

- All artifacts in English; chat in rioplatense Spanish. Conventional commits + `Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>`; branch + ff-merge in the skills repo.
- Evals change BEFORE skill content (repo hard constraint). No skill content before the baseline run is observed (Iron Law).
- Fixture + workspace live under the job tmp dir (NOT the repo — keeps the repo clean; deviation from skill-creator's sibling-workspace default, location-only).
- The skill under test never edits fixture application code; assertion 7 verifies it by hash comparison.

### Task 1: Fixture + workspace

Fixture at `<tmp>/extract-test/`: a small Next-style app, NO DESIGN.md, with planted drift — 7 distinct gray values across files (mix of hex/rgb/Tailwind gray-*), 3 button radius treatments, a back button consistent across 3 detail views (`productos/[id]`, `pedidos/[id]`, `clientes/[id]`), and 2 competing card styles for the same data (bordered in `productos`, shadowed in `destacados`). Snapshot SHA-256 of every fixture file to `<workspace>/fixture-hashes.txt`. Workspace `<tmp>/extracting-design-md-workspace/iteration-1/` + `evals.json` (prompt only) + `eval_metadata.json` (assertions empty).

Eval prompt (realistic owner voice, ES): "che, el diseño de este proyecto quedó re inconsistente — cada botón es distinto y hay como mil grises. Quiero adoptar DESIGN.md acá: armalo desde lo que ya hay y decime qué tan desprolijo está y cómo lo vamos limpiando."

### Task 2: RED — baseline

Launch the baseline subagent (no skill) with the eval prompt + fixture path; save outputs to `iteration-1/adopt-designmd/without_skill/outputs/`; capture `timing.json` from the completion notification. While it runs, draft the assertions into `eval_metadata.json`:

1. `design-md-produced` — DESIGN.md exists at fixture root.
2. `gates-pass` — design-md-gen runs clean on it AND context-lint design checks PASS.
3. `drift-families-evidenced` — report names ≥2 drift families (radii, grays) with occurrence counts and file:line evidence.
4. `tokens-collapsed` — proposed color tokens < raw gray variant count.
5. `decisions-backfilled` — dated back-button Decision under the detail-view surfaces.
6. `contradiction-not-invented` — competing card styles surfaced as an open question, not silently resolved.
7. `code-untouched` — fixture hashes unchanged (excluding DESIGN.md/design.tokens.css).
8. `plan-with-convergence` — migration plan in per-surface batches stating the drift count as the convergence metric.

Document baseline failures verbatim (they drive GREEN content).

### Task 3: GREEN — evals, skill, with-skill run

Write the 3 repo evals per the spec's Evals section (Query/Fixture/Expected format). Then SKILL.md: workflow per the spec's Decisions (inventory → harvest+cluster → drift report → propose tokens → backfill Decisions → write+generate+lint → migration plan + hand-off), convergence loop section, bounded-scaling section, judgment notes; description pushy-on-triggers without step enumeration; explain-why over MUSTs; add counters for the specific baseline failures observed. Launch the with-skill subagent (same prompt + skill body + reference paths), outputs to `with_skill/outputs/`, capture timing. Reset fixture from hashes first if the baseline mutated it.

### Task 4: Grade, benchmark, viewer

Grade both runs against the 8 assertions (programmatic where possible: existence, gen+lint exit codes, hash compare; judgment for evidence/quality) into `grading.json` per run (fields `text`/`passed`/`evidence`). Run `python -m scripts.aggregate_benchmark iteration-1 --skill-name extracting-design-md` from the skill-creator directory; analyst pass per `agents/analyzer.md` (non-discriminating assertions, variance). Generate the viewer: `generate_review.py iteration-1 --skill-name extracting-design-md --benchmark .../benchmark.json --static <workspace>/review.html` and give the owner the path BEFORE making self-directed revisions.

### Task 5: Ship

Apply owner feedback (and REFACTOR counters for graded failures; evals first when reality wins). README table row. Lint PASS. Branch `feat/extracting-design-md`, commit, ff-merge, push. Offer the description-optimization loop (`run_loop.py`, ~20 trigger queries) as an opt-in follow-up — token-heavy, runs in background.
