# skills

Personal Agent Skills library for Claude Code: methodology skills usable from
any repo. The context-engineering standard and its enforcement skills
(context-init, context-audit) live in the Context-Engineering repo; this repo
holds everything else.

## Commands

- `node ../Context-Engineering/scripts/context-lint.mjs .` — mechanical
  compliance check (needs the Context-Engineering repo cloned alongside)

## Gotchas

- Skills here are junction-linked into `~/.claude/skills` by the workstation
  installer: edits go live immediately, no copy step; a NEW skill needs one
  installer run to create its junction.
- The frontmatter `description` is the discovery interface — Claude picks
  skills by description alone; keep what + when (triggers) sharp.

## Hard constraints

- Every skill ships with 3 evals (`evals/`); evals change BEFORE skill
  content.
- Nothing here may violate the context-engineering standard (SKILL.md <500
  lines, references one level deep, third-person descriptions).
