# RTK - Rust Token Killer (Codex CLI)

**Usage**: Token-optimized CLI proxy for shell commands.

## Rule

Use `rtk` for verbose supported commands whose output would otherwise waste
model context. Do not wrap tiny shell predicates, file-existence checks,
`mkdir`, `pwd`, `command -v`, short `sed -n` reads, or other commands where raw
output is already compact and exact.

Examples:

```bash
rtk git status
rtk git diff
rtk cargo test
rtk npm run build
rtk pytest -q
rtk ruff check
```

## Meta Commands

```bash
rtk gain            # Token savings analytics
rtk gain --history  # Recent command savings history
rtk proxy <cmd>     # Run raw command without filtering
```

## Verification

```bash
rtk --version
rtk gain
which rtk
```
