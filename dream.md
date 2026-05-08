---
description: Memory consolidation — synthesize recent learnings into durable, well-organized memories
---

# Dream: Memory Consolidation

Perform a reflective pass over memory files. Synthesize recent learnings into durable, well-organized memories so future sessions orient quickly.

Memory directory: auto-discover by running `ls ~/.claude/projects/*/memory/MEMORY.md` and using the match for the current project. Alternatively, compute the path key from `$PWD`: `echo "$PWD" | sed 's|^/||; s|/|-|g'` gives the projects key, then `~/.claude/projects/-${key}/memory/`.

Session transcripts: same parent directory as memory (large JSONL files — grep narrowly, never read whole files).

---

## Phase 0 — Current session

Review what was discussed, decided, or discovered in the current conversation. This is usually the primary source of new signal — look for feedback, corrections, project decisions, or user preferences worth persisting.

## Phase 1 — Orient

- `ls` the memory directory to see what already exists
- Read the `MEMORY.md` index to understand the current structure
- Skim existing topic files to improve them rather than creating duplicates

## Phase 2 — Gather recent signal

Look for new information worth persisting. Sources in rough priority order:

1. **Current session findings** from Phase 0
2. **Existing memories that drifted** — facts that contradict something in the codebase now
3. **Transcript search** — if specific context is needed, use the Grep tool to search JSONL transcripts for narrow terms in the project directory

Do not exhaustively read transcripts. Look only for things already suspected to matter.

## Phase 3 — Consolidate

For each thing worth remembering, write or update a memory file at the top level of the memory directory. Use the memory file format and type conventions from the system prompt's auto-memory section — it is the source of truth for what to save, how to structure it, and what NOT to save.

File naming convention: `{type}_{slug}.md` where type is one of: `user`, `project`, `feedback`, `reference`.
Required frontmatter fields: `name`, `description`, `type`.
Include `**Why:**` and `**How to apply:**` sections when the memory records a decision or preference.

Focus on:
- Merging new signal into existing topic files rather than creating near-duplicates
- Converting relative dates ("yesterday", "last week") to absolute dates
- Deleting contradicted facts — if today's investigation disproves an old memory, fix it at the source

Note: Memories sync across all hosts via Syncthing. Avoid writing host-specific absolute paths (like `/home/notroot/` or `/Users/notroot/`) directly into memory content — use `$HOME` or describe the path pattern instead, so content remains interpretable on macOS, Linux desktop, laptop, and server.

## Phase 4 — Prune and index

Update `MEMORY.md` so it stays under 100 lines AND under ~25KB. It is an **index**, not a dump — each entry should be one line under ~150 characters: `- [Title](file.md) — one-line hook`. Never write memory content directly into it.

- Remove pointers to memories that are stale, wrong, or superseded
- Demote verbose entries: if an index line is over ~200 chars, shorten it and move detail to the topic file
- Add pointers to newly important memories
- Resolve contradictions — if two files disagree, fix the wrong one

---

Return a brief summary of what was consolidated, updated, or pruned. If nothing changed (memories are already tight), say so.
