# Global AGENTS

## Token Budget

Input tokens are the primary budget risk. Default to token-efficient operation:

- Start with a judgment from existing context before doing broad investigation.
- Prefer `rg` and narrow line reads over opening whole large files.
- Do not start subagents unless the user explicitly asks for agents,
  delegation, or parallel work.
- Do not run lint/build/test/check commands unless verification is necessary
  for a code change or the user asks for validation.
- Avoid commands that produce large diffs/logs; use summarized tooling such as
  `rtk` for noisy engineering commands.
- In `省额度` mode, do the minimum necessary read/command set and ask before any
  high-input-cost operation.

## Self-Improvement

Use the global memory directory at `$HOME/.codex/memories`.

Default to token-efficient memory use. The injected memory summary is the first
source of truth; do not reread memory files for every task.

Before starting a task:
1. Use the injected memory summary first.
2. Read `$HOME/.codex/memories/PROFILE.md` / `ACTIVE.md` only when the task is
   non-trivial, history-sensitive, preference-sensitive, or the summary is
   insufficient.
3. In `省额度` mode, skip memory file reads unless the user explicitly asks for
   memory lookup or the task cannot be answered safely without it.
4. Search `MEMORY.md` or rollout summaries only after a specific, relevant need
   is identified; do not do broad memory scans by default.

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

@$HOME/.codex/RTK.md
