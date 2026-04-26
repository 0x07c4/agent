# ERRORS

## [ERR-20260426-001] skill-creator-init-script-not-executable
**Logged**: 2026-04-26T00:00:00+08:00
**Priority**: low
**Status**: noted

### Summary
系统 skill-creator 的 `init_skill.py` 在当前环境里不能直接执行，会返回 `permission denied`；用 `python3 path/to/init_skill.py ...` 可以正常初始化 skill。

### Error
```text
zsh:1: permission denied: $HOME/.codex/skills/.system/skill-creator/scripts/init_skill.py
```

### Context
- Command: `$HOME/.codex/skills/.system/skill-creator/scripts/init_skill.py llm-wiki ...`
- Situation: 创建全局 Codex `llm-wiki` skill
- Environment: Linux / Codex sandbox / skill-creator system skill

### Suggested Fix
- 初始化 skill 时优先用 `python3 $HOME/.codex/skills/.system/skill-creator/scripts/init_skill.py ...`
- 不要把这个错误误判成 skill-creator 不可用或路径错误

## [ERR-20260425-001] vite-dev-server-listen-eperm-in-sandbox
**Logged**: 2026-04-25T00:00:00+08:00
**Priority**: low
**Status**: pending

### Summary
在 Codex sandbox 内直接启动 Vite dev server 可能因为监听本地端口失败而返回 `EPERM`；同一命令用审批后的 escalated 权限可以正常启动。

### Error
```text
Error: listen EPERM: operation not permitted 127.0.0.1:5173
```

### Context
- Command: `npm run dev -- --host 127.0.0.1`
- Situation: 为前端改动启动本地预览服务
- Environment: Linux / Codex workspace-write sandbox / Vite

### Suggested Fix
- 如果 Vite/本地服务启动因 `listen EPERM` 失败，直接按 sandbox 策略用 `require_escalated` 重跑同一命令
- 不要误判成端口占用或 Vite 配置错误，除非错误信息是 `EADDRINUSE`

## [ERR-20260417-001] eastmoney-urllib-remote-disconnected
**Logged**: 2026-04-17T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Summary
访问东方财富 `push2his.eastmoney.com` 的分时/`K` 线接口时，`urllib` 和基于它的 `requests/urllib3` 可能被服务端直接断开连接，但同一 URL 用 `curl` 可以正常返回 JSON。

### Error
```text
http.client.RemoteDisconnected: Remote end closed connection without response
requests.exceptions.ConnectionError: ('Connection aborted.', RemoteDisconnected(...))
```

### Context
- Command: Python `urllib.request.urlopen(...)` / `requests.get(...)`
- Situation: 拉取东财 `trends2/get` 或 `kline/get`
- Environment: Linux / Python 3.14

### Suggested Fix
- 东财行情抓取层不要只依赖 `urllib`；遇到 `RemoteDisconnected` 时优先自动回退到 `curl`
- 如果 UI 当前只显示“待同步”占位态，要把真实错误文本直接透传到图表区域，避免静默吞错

## [ERR-20260411-002] codex-apps-wham-blocked-by-chatgpt-edge
**Logged**: 2026-04-11T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Summary
`codex_apps` 启动失败时，如果 `auth.json` 里登录态看起来正常，不要先把问题归因到 token 过期；这次实际根因是访问 `https://chatgpt.com/backend-api/wham/apps` 时被 ChatGPT/Cloudflare 边缘返回了拦截 HTML，导致 MCP 握手拿到的不是 JSON。

### Error
```text
MCP startup failed: handshaking with MCP server failed
...
error sending request for url (https://chatgpt.com/backend-api/wham/apps)
...
Unexpected content type: text/html; charset=UTF-8
...
Unable to load site
```

### Context
- Command: Codex CLI / TUI 启动 `codex_apps` MCP
- Situation: 会话启动时出现 `MCP startup incomplete (failed: codex_apps)`
- Environment: Linux desktop / `auth_mode = "chatgpt"` / 本地 auth 存在

### Suggested Fix
- 先查 `$HOME/.codex/log/codex-tui.log`，不要只看前台警告
- 如果日志里出现 `Unexpected content type: text/html`、`Unable to load site` 或 Cloudflare/Ray ID，优先判断为网络出口、VPN、代理、IPv6 或 IP 风控问题，而不是本地 MCP 配置错误
- 可用外部探针快速分层：对 `https://chatgpt.com/backend-api/wham/apps` 做 `HEAD`，若返回 `405` 且 `Allow: POST`，再做最小 `POST {}`；若返回 `401 JSON`，说明链路已恢复，剩下更可能是客户端态或瞬时边缘问题
- 这类情况下，单纯重新登录通常不是首选；更有效的是换网络、关 VPN/代理、尝试不同出口，必要时优先测试是否能从浏览器正常打开 `chatgpt.com`
- 如果只是 `codex_apps` 失败而主模型连接正常，说明问题可能只落在 `chatgpt.com/backend-api/wham/apps` 这条链路上，不要误判成整个 Codex 都不可用

## [ERR-20260411-001] gtk-widget-instantiation-segfaults-headless
**Logged**: 2026-04-11T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Summary
在无图形会话里直接用 Python/GI 实例化 GTK widget 进行布局探测，可能不会优雅报错，而是直接触发段错误。

### Error
```text
Process exited with code 139
```

### Context
- Command: `python - <<'PY' ... Gtk.Label() / Gtk.Box() ... PY`
- Situation: 想在终端里无头验证 GTK widget 默认属性与可见性
- Environment: Linux / PyGObject / GTK4 / 无可用 display
- Follow-up: 直接实例化 `Gtk.DrawingArea` 并导入包含自定义绘图控件的模块（如 `chop.app` 里的图表组件）做离屏渲染验证，也可能同样以 `code 139` 崩掉

### Suggested Fix
- 不要把“导入成功”推断成“可以无头实例化 widget”
- 涉及 GTK widget 树调试时，优先在真实图形会话里运行，或先做 `Gtk.init_check()` / display 检查
- 无头环境下尽量只检查模块、版本和纯 Python 数据，不直接 new GTK widget
- 如果想验证绘图结果，优先在真实图形会话里跑应用，或把 Cairo 绘制逻辑继续下沉到不依赖 GTK widget 实例化的纯函数

## [ERR-20260324-001] codex-workspace-latency
**Logged**: 2026-03-24T00:00:00+08:00
**Priority**: high
**Status**: pending

### Summary
Codex 在工作区协作中可能因为扫描无关目录或缺少明确范围边界而显著变慢。

### Error
```text
工作区协作阶段长时间无正文输出，看起来像“卡住”或“网络慢”，但实际常常是范围过大或模型仍在整理结果。
```

### Context
- Command: codex exec --json
- Situation: 已挂载工作区并进入工作区协作
- Environment: Linux / Tauri / React / Codex CLI

### Suggested Fix
- 使用 `.ignore` 控制工作区范围
- 对工作区协作使用更宽松但更准确的模式化等待文案
- 将内部工具事件压缩为用户可理解的阶段摘要

## [ERR-20260324-002] misleading-apply-before-preview
**Logged**: 2026-03-24T00:00:00+08:00
**Priority**: high
**Status**: pending

### Summary
当用户尚未看到具体改动内容时就出现“应用改动”，会直接破坏建议链路的可信度。

### Error
```text
方向选择阶段出现了应用按钮，但用户还没有看到具体改动或明确预览。
```

### Context
- Command: N/A
- Situation: Solo 主区方向卡 / 建议卡交互
- Environment: Solo workbench UI

### Suggested Fix
- 把“点击卡片预览”和“确认选择方向”拆成两个动作
- 只有具体预览卡才允许出现“应用改动 / 执行命令”

## [ERR-20260328-002] archive-delete-read-only-in-sandbox
**Logged**: 2026-03-28T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Summary
当前 Codex 会话里可以从 `/mnt/archive` 读取并复制到 `/mnt/games`，但尝试删除 `/mnt/archive` 上的大量旧文件时会触发 `Read-only file system`，导致删除失败。

### Error
```text
rm: cannot remove '...': Read-only file system
```

### Context
- Command: `rm -rf /mnt/archive/SteamLibrary/...`
- Situation: Steam 游戏迁移后清理 NTFS 源盘旧文件
- Environment: Linux desktop / Codex sandbox / `/mnt/archive` 为 `ntfs3`

### Suggested Fix
- 先假设当前会话对该挂载点的删除不可靠，删除前做一次小文件试探或直接提醒用户在本机终端执行
- 对“可复制但不可删”的挂载点，不要把前一次写成功推断成后续删除也一定可行

## [ERR-20260328-001] rsync-missing-in-environment
**Logged**: 2026-03-28T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Summary
在这台 Linux 机器上执行大文件迁移时，不能默认假设 `rsync` 已安装；这次 `rsync` 直接不存在，导致首次迁移命令失败。

### Error
```text
zsh:2: command not found: rsync
```

### Context
- Command: `rsync -a --info=progress2 ...`
- Situation: 迁移 Steam 游戏目录与 manifest
- Environment: Linux desktop / Steam library migration

### Suggested Fix
- 在需要批量复制前，先用 `command -v rsync` 检查环境
- 如果没有 `rsync`，直接回退到 `cp -a` 或先安装 `rsync`
- 不要把“有进度条”当成默认能力，先确认工具存在

## [ERR-20260329-001] qt-cmake-build-dir-stale-needs-fresh-configure
**Logged**: 2026-03-29T00:00:00+08:00
**Priority**: low
**Status**: pending

### Summary
Qt/QML 项目在已有 `build/` 目录上增量改 CMake 依赖后，`cmake --build` 可能在 reconfigure 阶段抛出 `configure_file: No such file or directory`，即使源码本身没有语法错误。

### Error
```text
CMake Error at /usr/lib/cmake/Qt6Core/Qt6CoreMacros.cmake:2511 (configure_file):
  No such file or directory
...
CMakeLists.txt:20 (qt_add_qml_module)
```

### Context
- Project type: Qt Quick / QML / CMake
- Situation: 在已有构建目录里新增 `Qt6::Network` 依赖后重新编译
- Environment: CMake 4.3.0 / Qt6

### Suggested Fix
- 先不要怀疑源码一定坏了，优先把问题归因到 `build/` 目录生成状态脏了
- 优先执行 `cmake --fresh -S <src> -B <build>` 重新生成，再继续 `cmake --build`
- 对 Qt/QML 项目，`cmake --fresh` 比手工删 `build/` 更稳也更省事

## [ERR-20260329-002] clash-tun-fake-ip-breaks-github-ssh
**Logged**: 2026-03-29T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Summary
如果本机开启了 Clash TUN 并使用 `fake-ip`，`git push` 走 GitHub SSH 时可能表现为被 `198.18.x.x` 主动断开；这时改 `HTTP_PROXY`、切换 `GIT_SSH_COMMAND`，甚至改走 `ssh -p 443` 往往都不够，因为流量已经在系统网络层被 TUN 接管。

### Error
```text
Connection closed by 198.18.x.x port 22
fatal: Could not read from remote repository.
```

### Context
- Command: `git push origin main`
- Situation: GitHub SSH push 失败，但本地 git 状态和远端地址看起来正常
- Environment: Linux desktop / Clash TUN / fake-ip

### Suggested Fix
- 优先在 Clash 里把 `github.com` / `ssh.github.com` 做 `DIRECT` 或加入 `fake-ip-filter`
- 如果必须保留代理，先确认所选代理组真的支持 GitHub SSH 的 TCP 转发
- 仅修改 shell 里的代理变量通常无效，因为 TUN 已经在应用层以下接管了连接
- 如果 `22` 和 `443` 都被同样的 `198.18.x.x` 连接关闭错误击中，优先判断为 TUN / fake-ip 问题，而不是仓库权限问题
- 临时替代方案是关闭 TUN，或改用 HTTPS remote 再 push

## [ERR-20260329-004] git-push-blocked-by-bad-system-ssh-config
**Logged**: 2026-03-29T22:35:35+08:00
**Priority**: medium
**Status**: pending

### Summary
某些环境下，`git push` 走 SSH 会因为系统级 SSH 配置文件权限异常直接失败；可通过临时绕过系统 SSH 配置完成推送。

### Error
```text
Bad owner or permissions on /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf
fatal: Could not read from remote repository.
```

### Context
- Command: `git push -u origin main`
- Situation: 本地仓库、远端地址和权限看起来正常，但 SSH 在读取系统配置时先失败
- Environment: Linux / Codex shell / GitHub SSH remote

### Suggested Fix
- 先不要误判成仓库权限或 remote 地址错误
- 直接用 `GIT_SSH_COMMAND='ssh -F /dev/null' git push ...` 绕过系统 SSH 配置重试
- 如果重试成功，说明问题在本机 SSH 配置链，而不是 GitHub 仓库本身

## [ERR-20260329-003] sandbox-blocks-sudo-package-install
**Logged**: 2026-03-29T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Summary
当前 Codex shell 沙箱带有 `no new privileges`，即使用户明确要求，也不能在会话里通过 `sudo` 安装系统包。

### Error
```text
sudo: /etc/sudo.conf is owned by uid 65534, should be 0
sudo: The "no new privileges" flag is set, which prevents sudo from running as root.
```

### Context
- Command: `sudo pacman -S --noconfirm rsync`
- Situation: 用户要求由 Codex 直接安装 `rsync`
- Environment: Codex sandbox / Linux / sudo escalation blocked

### Suggested Fix
- 不要在当前沙箱里反复重试 `sudo` / `pacman` 提权安装
- 直接让用户在自己的终端运行系统安装命令
- 装完后再由 Codex继续做验证或后续配置同步

## [ERR-20260329-004] fcitx5-remote-reload-can-fail-in-codex-shell-even-with-dbus-env
**Logged**: 2026-03-29T00:00:00+08:00
**Priority**: low
**Status**: pending

### Summary
在 Codex shell 里即使存在 `DBUS_SESSION_BUS_ADDRESS`、`DISPLAY`、`WAYLAND_DISPLAY`，`fcitx5-remote -r` 仍可能因无法建立 DBus 连接而失败。

### Error
```text
terminate called after throwing an instance of 'std::runtime_error'
  what():  Failed to create dbus connection
```

### Context
- Command: `fcitx5-remote -r`
- Situation: 已通过 `rime_deployer --build` 生成新 schema，准备热重载 fcitx5
- Environment: Codex shell / Linux / 图形会话变量存在但 DBus 连接不可用

### Suggested Fix
- 不要把这类失败误判成 Rime 配置错误
- 先用 `rime_deployer --build` 确认配置已成功落盘
- 需要立即生效时，优先让用户在图形会话里通过托盘菜单“重新部署”或重启 `fcitx5`

## [ERR-20260329-005] direct-script-exec-may-fail-in-codex-shell-even-with-x-bit
**Logged**: 2026-03-29T00:00:00+08:00
**Priority**: low
**Status**: pending

### Summary
在 Codex shell 里，本地脚本即使已经带可执行位，直接执行仍可能报 `permission denied`；改成 `bash path/to/script.sh` 可以正常运行。

### Error
```text
zsh:1: permission denied: $HOME/workspace/utopia/scripts/update-from-shorin.sh
```

### Context
- Command: `$HOME/workspace/utopia/scripts/update-from-shorin.sh`
- Situation: 文件权限为 `-rwxr-xr-x`
- Environment: Codex shell / Linux / 脚本在用户工作目录下

### Suggested Fix
- 先不要误判成文件权限没设对
- 优先用 `bash script.sh` 或 `zsh script.sh` 执行本地脚本
- 需要验证可执行位时，单独用 `ls -l` 检查

## [ERR-20260330-001] alsa-cli-may-fail-even-when-pipewire-sees-cards
**Logged**: 2026-03-30T00:00:00+08:00
**Priority**: low
**Status**: pending

### Summary
在当前 Codex shell 环境里，即使 `/proc/asound/cards` 有声卡、PipeWire 也能正常枚举到 ALSA 设备，`amixer -c 0` 这类直接走 ALSA 控制面的命令仍可能报 `Invalid card number`。

### Error
```text
Invalid card number '0'.
```

### Context
- Command: `amixer -c 0 scontrols`
- Situation: 排查 Arch Linux 模拟音频输出异常时，PipeWire 已显示 `Built-in Audio Analog Stereo`
- Environment: Codex shell / Linux / PipeWire 正常 / `/proc/asound/cards` 可读

### Suggested Fix
- 先不要把问题误判成“系统没有声卡”
- 在这个环境里优先使用 `wpctl`、`pactl`、`journalctl` 做诊断
- 如果必须看 ALSA mixer，改让用户在自己的图形终端或真实 shell 里执行 `amixer` / `alsamixer`

## [ERR-20260413-001] xelatex-fontspec-hardcoded-font-name-may-fail-across-hosts
**Logged**: 2026-04-13T22:15:00+08:00
**Priority**: low
**Status**: pending

### Summary
在 XeLaTeX 项目里给 `fontspec` / `ctex` 直接写死一个看起来常见的字体名，不代表当前主机一定能解析到；即使 TeX Live 已完整安装，也可能因为字体注册名不一致而直接编译失败。

### Error
```text
! Package fontspec Error:
(fontspec)                The font "Latin Modern Roman" cannot be found;
```

### Context
- Command: `make pdf`
- Situation: 为了减少 XeLaTeX 字体 warning，尝试在模板里加入 `\setmainfont{Latin Modern Roman}`
- Environment: Codex shell / Linux / XeLaTeX 可用 / `fontspec` 可用，但宿主机字体注册名与预期不一致

### Suggested Fix
- 不要为了“清空 warning”随手给模板加入未经验证的宿主机字体名
- 如果项目本来已经能编译，优先保留可移植配置
- 只有在先确认 `fc-list` / 目标环境字体清单后，再加 `\setmainfont{...}`

## [ERR-20260423-001] chmod-may-fail-on-workspace-files-even-when-file-writes-succeed
**Logged**: 2026-04-23T00:00:00+08:00
**Priority**: low
**Status**: noted

### Summary
在当前 Codex sandbox 里，即使可以通过 `apply_patch` 正常创建或修改仓库内文件，后续对同一路径执行 `chmod` 仍可能失败并报只读文件系统。

### Error
```text
chmod: changing permissions of '.codex/skills/openpencil/scripts/check_openpencil_env.sh': Read-only file system
```

### Context
- Command: `chmod +x .codex/skills/openpencil/scripts/check_openpencil_env.sh`
- Situation: 新建本地 skill 后，准备给脚本补执行权限
- Environment: Codex shell / workspace-write sandbox / 同一路径文件内容可正常写入

### Suggested Fix
- 不要把 `chmod` 是否成功当成文件是否可用的前提
- 优先直接用 `bash script.sh` 运行脚本
- 如果确实依赖可执行位，再评估是否需要提权或改到可变更权限的目录

## [ERR-20260423-002] project-local-skills-cleanup-may-leave-a-readonly-dot-agents-sentinel
**Logged**: 2026-04-23T00:00:00+08:00
**Priority**: low
**Status**: noted

### Summary
在当前环境里，删除 repo 内由 `skills add` 生成的 `.agents/` 目录后，根目录可能残留一个只读的空 `.agents` 文件；单纯 `rm` 后它仍会继续存在。

### Context
- Situation: 将 `huashu-design` 从项目级安装迁移为全局安装后，清理 `./.agents` 痕迹
- Observation: `.agents/` 目录删除后，仓库根目录出现 `0444` 权限的空文件 `.agents`

### Suggested Fix
- 不要在这里反复和这个哑文件搏斗
- 直接把 `.agents`、`.claude/`、`skills-lock.json` 写入 `.gitignore`
- 把这类文件视为外部 skill 管理器的环境产物，而不是业务代码的一部分

## [ERR-20260425-001] asyncio-to-thread-can-hang-in-current-codex-sandbox
**Logged**: 2026-04-25T00:00:00+08:00
**Priority**: medium
**Status**: noted

### Summary
在当前 Codex Linux sandbox 里，Python `asyncio.to_thread()` 包同步 provider HTTP 调用时，单元测试会挂住且没有正常返回；去掉 `to_thread()` 后同一 mocked `urllib.request.urlopen` 测试立即通过。

### Context
- Project: `cocoa`
- Situation: 为 OpenAI-compatible provider adapter 写 stdlib-only HTTP 客户端
- Observation: `asyncio.to_thread(self._complete_sync, request)` 进入测试后卡住；直接 `return self._complete_sync(request)` 后 `unittest` 全量通过

### Suggested Fix
- 在当前 sandbox 下，不要默认用 `asyncio.to_thread()` 包 provider 调用
- MVP 阶段宁可保持同步 HTTP 调用，等需要并发 provider 时再引入明确可测的 executor/worker 设计
- 回归测试用 `timeout` 包裹整轮 unittest，避免挂起进程阻塞任务
## [ERR-20260425-001] git-commit-fails-read-only-dotgit
**Logged**: 2026-04-25T00:00:00+08:00
**Priority**: medium
**Status**: resolved

### Summary
`git commit` 直接失败：`Unable to create '.git/index.lock': Read-only file system`。

### Error
```text
fatal: Unable to create '$HOME/workspace/cocoa/.git/index.lock': Read-only file system
```

### Context
- Task: 提交 cocoa 修改
- Environment: 工作区只允许特定写路径，`.git` 目录不可写

### Suggested Fix
- 使用已批准的提权执行提交（或要求放开该路径写权限），再重试。

## 2026-04-26 - 中断后的 Spark 半成品代码导致 Rust 编译失败

在 Solo 中，之前把 runtime projection 任务交给 Spark 后被中断，留下了未完成的 `src-tauri/src/lib.rs` 改动：错误作用域引用 `execution_mode/workspace/state_handle/current_turn_id`、move 后继续借用 `stage/detail/level/turn_intent`、非 Result 函数里使用 `?`、以及错误解构 `Option<usize>`。经验：中断 worker 后必须先清理/编译验证半成品代码，再继续产品或实现讨论。

## 2026-04-26 - TTY raw mode 会破坏 CLI composer 换行重绘

在 terminal-native CLI 里用 `tty.setraw()` 做逐键输入时，会关闭输出处理，导致 `\n` 不再自动回到行首，菜单重绘会在真实终端中斜向错位。经验：轻量 composer 优先用 `tty.setcbreak()`，并对重绘输出使用明确的换行策略；真实 PTY 验证要覆盖输入 `/` 后的多行候选菜单。

## 2026-04-26 - OpenPencil CLI 路径误判
在 Solo v3 画稿任务中，错误判断 v2 曾通过 OpenPencil chat/agent 入口生成。用户纠正：v2 也是通过 skill 直接操纵 OpenPencil Desktop。后续使用 OpenPencil 时应先确认实际可用命令和历史成功路径，不能把失败归因到用户未配置 agent chat。当前环境里 sandbox 内 `op status` 会误报 false，需要在 sandbox 外执行；`op design` 0.7.4 吃 batch DSL，不吃 Markdown/natural-language brief。
