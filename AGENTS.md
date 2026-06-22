# Agent guide

This repository is a **vendor-agnostic agent harness**: one source of truth in
`.agents/`, compiled into each AI coding agent's native config. The repo *is*
the example — there is no separate product.

## Source of truth — `.agents/`

Everything agent-related is authored here:

- `.agents/permissions.toml` — permission intent (allow / ask / deny)
- `.agents/hooks.toml` — hook intent, with `{file}` / `{repo_root}` placeholders
- `.agents/skills/<name>/SKILL.md` — skill sources (one dir per skill)
- `.agents/setup.py` — the compiler that emits the per-vendor configs

Generated mirrors — **gitignored, read-only, do not edit:** `.claude/`,
`.codex/`, and the root `CLAUDE.md` (a `@AGENTS.md` shim so Claude Code inlines
this file).

Editing any mirror is wasted work: the next compile overwrites it. **All changes
go in `.agents/` or `AGENTS.md`.** Mirrors stay fresh automatically — a
`session_start` hook runs `.agents/setup.py` each session. Manual check:
`python3 .agents/setup.py --check` (non-zero exit if a mirror is stale).

## Bootstrap on a fresh clone

Mirrors are gitignored, so a fresh clone has none — including the hook that keeps
them fresh. Run the compiler once after cloning; it self-maintains afterward:

```bash
python3 .agents/setup.py
```

Requires Python 3.11+ (stdlib `tomllib`, no dependencies).

## How to change things

- **Permissions** — edit `.agents/permissions.toml`, then recompile.
- **Hooks** — edit `.agents/hooks.toml`. Use `{file}` / `{repo_root}`
  placeholders; a hook with a placeholder a vendor can't map is omitted for that
  vendor, not emitted with a guess.
- **Skills** — add `.agents/skills/<name>/SKILL.md` (plus any sibling files);
  they're mirrored verbatim into every vendor. Deleting a source removes its
  mirrors on the next compile.
- **A new vendor** — see `docs/adding-a-vendor.md`.

## Conventions

- **Directory docs are `AGENTS.md`, not `README.md`.** `README.md` exists only at
  the root (the human front door). Subdirectories use `AGENTS.md`.
- **Commits** — see the `commit` skill: small, logical, known-good chunks.

## Don't

- Don't edit `.claude/`, `.codex/`, or the root `CLAUDE.md` — they're generated.
- Don't commit the generated mirrors (they're gitignored for a reason).
- Don't hand-author a vendor config to work around the compiler — teach the
  compiler instead, so every vendor stays in sync.
