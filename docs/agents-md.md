# Writing AGENTS.md

Last updated: 2026-09-17

AGENTS.md is the emerging open standard for repo-level agent instructions (see agents.md). One file at the repo root that any coding agent reads before working. Codex, Cursor, Copilot, and others all honor it, which makes it the closest thing the ecosystem has to a universal agent config.

## The hierarchy

- The nearest AGENTS.md to the file being edited wins. In monorepos, put one per package: root for shared tooling, `backend/` for server rules, `frontend/` for client rules.
- For tool-specific files (CLAUDE.md, `.github/copilot-instructions.md`, GEMINI.md), symlink to AGENTS.md as the canonical source instead of maintaining duplicates. Only create separate files when you need a feature AGENTS.md cannot express.
- Keep personal, uncommitted tweaks in override files (for example Codex's `AGENTS.override.md`) so they never leak into the repo.

## What good ones contain

In priority order:

1. Project overview: what it is, language, framework, versions. Two or three lines.
2. Build and test commands, exact, with flags: `pnpm vitest run src/auth`, not "run the tests".
3. Critical constraints: architecture rules and things the agent must never do ("never edit generated files in `src/gen`", "all money math in integer cents").
4. Code conventions, but only the ones that differ from language defaults. The model already knows idiomatic TypeScript. Tell it what is weird about YOUR repo.
5. Definition of done: required tests, lint, typecheck, docs updates before a change counts as finished.
6. Security boundaries: files the agent must not touch, how secrets are handled.

## What to leave out

- Generic advice ("write clean code", "follow SOLID"). Pure context bloat.
- Folder structure overviews. The agent can run `ls`.
- Framework conventions baked into model training.
- Anything already in the README. Point at it instead of duplicating it.
- Raw `/init` output committed without human review. Always curate it.

## Size discipline

Start with one file under 150 to 200 lines; it is enough for most projects. Hard ceiling around 32 KiB (Codex's default read limit). If it keeps growing, split by directory instead of letting the root file bloat. Every line should earn its place by preventing a mistake the agent would otherwise make.

## Sources

- https://agents.md/
- https://github.com/devskale/skale-skills/blob/HEAD/docs/agents-md-best-practices.md
