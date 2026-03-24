# agent

This repo is a sanitized snapshot of my Codex configuration.

## Included

- `config.toml`
  - global model / personality / feature settings
  - project-specific trust entries are intentionally omitted
- `rules/default.rules`
  - approval rules snapshot
  - local home path is rewritten as `$HOME`
- `memories/`
  - global Codex memory loop files
- `skills/`
  - non-system installed skills snapshot

## Excluded

These files are intentionally not synced:

- `auth.json`
- `history.jsonl`
- `session_index.jsonl`
- `sessions/`
- `log/`
- `logs_*.sqlite*`
- `state_*.sqlite*`
- `models_cache.json`
- `tmp/`
- `shell_snapshots/`

They contain authentication state, personal history, runtime cache, or machine-local data.

## Notes

- This repo is meant to sync reusable Codex configuration, not live runtime state.
- If you want project trust entries on another machine, add them locally in `config.toml`.
