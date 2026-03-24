# systemd user 定时同步

这套配置只做一件事：每天用 `systemd --user` 调一次 `$HOME/.codex/bin/sync-agent-config.sh`。

## 安装脚本

先把同步脚本放到本机：

```bash
mkdir -p "$HOME/.codex/bin"
install -m 755 ./bin/sync-agent-config.sh "$HOME/.codex/bin/sync-agent-config.sh"
```

## 安装 unit

```bash
mkdir -p "$HOME/.config/systemd/user"
cp ./systemd/sync-agent-config.service "$HOME/.config/systemd/user/"
cp ./systemd/sync-agent-config.timer "$HOME/.config/systemd/user/"
systemctl --user daemon-reload
systemctl --user enable --now sync-agent-config.timer
```

## 验证

```bash
systemctl --user status sync-agent-config.timer
systemctl --user list-timers sync-agent-config.timer
```
