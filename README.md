# Agent Config Snapshot

This repository stores a sanitized snapshot of a local Codex setup.

Included:
- `AGENTS.md`
- `bin/sync-agent-config.sh`
- `systemd/`
- `docs/systemd-sync.md`
- `config.toml` with local project trust blocks removed
- `rules/`
- `memories/`
- non-system skills from `~/.codex/skills`

Excluded:
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

Notes:
- local home paths are rewritten to `$HOME`
- this snapshot is meant for backup and portability, not full runtime state sync

## Manual sync

Use the installed helper:

```bash
$HOME/.codex/bin/sync-agent-config.sh
```

Or run the repo copy directly:

```bash
./bin/sync-agent-config.sh --dry-run
```

See `docs/systemd-sync.md` for the daily `systemd --user` timer setup.
