# Global AGENTS

## Self-Improvement

Use the global memory directory at `$HOME/.codex/memories`.

Before starting any task:
1. Read `$HOME/.codex/memories/PROFILE.md`
2. Read `$HOME/.codex/memories/ACTIVE.md`
3. Apply them as global memory before analyzing the user request

Log only when the outcome is non-obvious, reusable, or likely to recur.

Evaluate whether to log a memory entry when any of the following happens:
1. A command, tool call, or operation fails unexpectedly
2. The user corrects a mistake, assumption, or outdated statement
3. A requested capability does not exist yet
4. An external API, integration, or tool behaves differently than expected
5. A non-obvious workaround, debugging insight, or better recurring approach is discovered

Write entries by type:
- `$HOME/.codex/memories/LEARNINGS.md` for learnings, corrections, knowledge gaps, and best practices
- `$HOME/.codex/memories/ERRORS.md` for unexpected errors and debugging notes
- `$HOME/.codex/memories/FEATURE_REQUESTS.md` for missing capabilities the user wants

Promotion rules:
1. If a pattern recurs or is broadly useful across tasks, promote it into `$HOME/.codex/memories/ACTIVE.md`
2. Keep `ACTIVE.md` concise and current
3. Only promote something into this `AGENTS.md` when it becomes a stable top-level rule, or when the user explicitly asks

Behavior expectations:
- Default to Chinese when writing memory entries unless the user asks otherwise
- Do not interrupt the user for every possible learning; log silently when confidence is high
- Do not log trivial typos, one-off noise, or low-value observations
- Do not edit `AGENTS.md` automatically unless the user explicitly asks
