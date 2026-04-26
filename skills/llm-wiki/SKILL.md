---
name: llm-wiki
description: Query and maintain the user's local agent knowledge wiki at `$HOME/workspace/wiki`. Use when Codex needs durable local knowledge, source-backed context, agent-field background, wiki/Obsidian workflow context, `llm-wiki` CLI usage, ingest/query/lint/reindex workflows, or when the user asks to consult, update, connect, or rely on the local wiki before answering.
---

# LLM Wiki

## Purpose

Use this skill to treat `$HOME/workspace/wiki` as Codex's local, traceable knowledge base. Prefer it over broad repo scanning when the task asks for prior context, agent-domain knowledge, Obsidian/wiki workflow, durable writeback, or source-backed answers.

## Entry Point

Default repo:

```bash
$HOME/workspace/wiki
```

Use the bundled wrapper instead of assuming `llm-wiki` is globally installed:

```bash
SKILL_DIR="$HOME/.codex/skills/llm-wiki"
"$SKILL_DIR/scripts/llm-wiki" status --json
```

Override the repo only when needed:

```bash
LLM_WIKI_REPO=/path/to/wiki "$SKILL_DIR/scripts/llm-wiki" query "question" --json
```

## Query Workflow

1. Read `$HOME/workspace/wiki/wiki/index.md` before broad wiki exploration.
2. Use `scripts/llm-wiki search "<topic>" --json` for cheap discovery.
3. Use `scripts/llm-wiki query "<question>" --json` when building an answer that needs traceable snippets.
4. Open only the relevant wiki pages and raw sources after the CLI narrows the scope.
5. Answer with source paths or wiki page references when claims depend on curated content.

Useful commands:

```bash
"$SKILL_DIR/scripts/llm-wiki" search "agent skills" --json
"$SKILL_DIR/scripts/llm-wiki" query "What makes a good agent skill?" --json
"$SKILL_DIR/scripts/llm-wiki" status --json
"$SKILL_DIR/scripts/llm-wiki" graph --json
```

## Maintenance Workflow

When the task changes durable wiki content:

1. Treat `raw/` as immutable source evidence.
2. Update `wiki/` as the maintained knowledge layer.
3. Prefer updating existing pages before creating new pages.
4. Update `wiki/index.md` and `wiki/log.md`.
5. Run:

```bash
"$SKILL_DIR/scripts/llm-wiki" reindex --check
"$SKILL_DIR/scripts/llm-wiki" lint
```

Use one-source-at-a-time ingest unless the user asks for batching. Do not write query results as durable wiki content without explicit user confirmation.

## Notes Boundary

`$HOME/workspace/notes` is upstream personal thinking, not the maintained wiki layer. Read it when the task is about the user's long-term product direction or intent. Do not edit it during wiki maintenance unless the user explicitly asks.

Selected notes can be snapshotted into `raw/sources/notes/` and then ingested into `wiki/`.

## Obsidian Boundary

Obsidian is the human-facing review and navigation layer. Use it as context for workflow decisions, but do not modify `.obsidian/` configuration unless the user explicitly asks.

## Durable Writeback Rule

If an answer produces reusable knowledge, propose a wiki target under `wiki/synthesis/`, `wiki/concepts/`, `wiki/entities/`, or `wiki/sources/`. Write the page only after the user confirms, then update `wiki/index.md` and `wiki/log.md`.
