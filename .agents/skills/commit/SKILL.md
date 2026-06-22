---
name: commit
description: Commit completed work in small, logical, known-good chunks with clear imperative messages. Use after finishing a coherent unit of work that is in a working state.
---

# Commit

This skill is the example skill for the harness: it is authored once in
`.agents/skills/commit/` and mirrored verbatim into every vendor's skills
directory by `.agents/setup.py`. Editing it here updates it everywhere.

## When

Commit when a coherent unit of work is **finished and in a known-good state** —
a working feature, a self-contained refactor, a doc update. Do not commit a
half-written or broken tree. Prefer **several small logical commits** over one
large dump.

## How

```
git add <paths for this logical chunk>
git commit -m "<imperative summary>"
```

- Scope each commit to one logical change; stage explicit paths rather than
  `git add -A` when a working tree holds unrelated edits.
- Write the message in the imperative mood ("add", "fix", "rename"), present
  tense, no trailing period in the summary line.
- Generated mirrors (`.claude/`, `.codex/`, `CLAUDE.md`) are gitignored and will
  not be staged — never commit them.

## Rules

- One commit per logical chunk.
- Never commit a broken or half-finished tree.
- Don't push unless asked.
