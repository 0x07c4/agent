# LEARNINGS

## [LRN-20260417-001] eastmoney-trends2-no-preclose
**Logged**: 2026-04-17T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Summary
东方财富 `trends2/get` 的 `data` 不一定提供 `preClose`；返回里稳定可用的是 `trends` 明细，昨收需要额外从报价接口反推。

### Details
一次真实返回中，`data` 只有这类字段：
- `code`
- `market`
- `type`
- `status`
- `name`
- `decimal`
- `trends`

`trends` 每行格式可按下面理解：
- `parts[0]`: 时间
- `parts[2]`: 当前/成交价
- `parts[5]`: 成交量
- `parts[7]`: 均价

如果界面绘图需要昨收参考线，不能假设 `data["preClose"]` 一定存在；更稳的是再请求一次报价接口，用 `最新价 / (1 + 涨跌幅%)` 反推昨收。

### Suggested Action
- 解析 `trends2` 时默认用 `parts[2] / parts[5] / parts[7]`
- 需要昨收时优先走报价接口推导，不再硬依赖 `preClose`

## [LRN-20260324-001] product-principle
**Logged**: 2026-03-24T00:00:00+08:00
**Priority**: high
**Status**: pending

### Summary
`Solo` 的核心不是自治 agent，而是 human-in-the-loop 的工作方式增强。

### Details
用户明确反对“AI 自己干活、人只能旁观”的产品方向。更符合目标的路径是：
- AI 提供知识、建议、选项、风险和预览
- 人负责做最终决策
- 工作区协作默认走“建议 + 预览 + 用户确认”

### Suggested Action
在 UI、提示词、状态文案和执行链路里，持续弱化“自动执行”叙事，强化“建议、预览、确认”。

### Metadata
- Source: conversation
- Tags: product, human-in-the-loop, solo

## [LRN-20260324-002] interaction-choice-preview-confirm
**Logged**: 2026-03-24T00:00:00+08:00
**Priority**: high
**Status**: pending

### Summary
方向选择阶段必须先预览，再确认选择；不能直接给“应用改动”。

### Details
当用户还没有看到具体改动、影响范围或方向预览时，任何“应用改动”按钮都会破坏信任。正确顺序是：
1. 展示方向卡
2. 点击方向卡查看预览
3. 点击按钮确认选择这个方向
4. 再生成更具体的改动预览
5. 最后才允许应用

### Suggested Action
保持“方向卡 / 预览卡 / 应用卡”三层分离，避免再次混成一张审批卡。

### Metadata
- Source: conversation
- Tags: ux, decision-flow, preview, trust

## [LRN-20260324-003] codex-scope-control
**Logged**: 2026-03-24T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Summary
要缓解 Codex 工作区协作变慢，不能只过滤 UI 文件树，还要把忽略规则传进协作提示词。

### Details
仅在前端隐藏 `node_modules` 或构建目录不够，Codex 仍可能在协作过程中把时间浪费在无关路径上。更有效的做法是：
- 文件树过滤
- 文件读取过滤
- 写入提案过滤
- 工作区协作提示词明确 `.ignore` 规则

### Suggested Action
把 `.ignore` 作为工作区范围控制的一部分持续保留，并在后续考虑更细的目录深度或白名单策略。

### Metadata
- Source: conversation
- Tags: codex, performance, workspace, ignore

## [LRN-20260325-004] explicit-turn-intent
**Logged**: 2026-03-25T00:00:00+08:00
**Priority**: high
**Status**: pending

### Summary
仅有“对话 / 工作区协作”两档还不够；视觉化决策流需要显式的 turn intent。

### Details
当工作区协作既承担“协作分析”、又承担“方向建议”和“具体预览”时，如果前后端没有显式阶段信号，后端就会退回到根据用户输入内容猜意图，最终再次误产出 `solo-write` 或把主区劫持成写文件提案。更稳定的做法是让前台显式传递这一轮的阶段，例如：
- `协作分析`
- `方向建议`
- `具体预览`

### Suggested Action
后续所有和建议卡/预览卡相关的实现，都优先用显式 turn intent 驱动，不再把“多方向还是具体预览”当成文本理解题。

### Metadata
- Source: conversation
- Tags: ux, state, turn-intent, workspace-collaboration

## [LRN-20260325-005] proposal-id-and-choice-fallback
**Logged**: 2026-03-25T00:00:00+08:00
**Priority**: high
**Status**: pending

### Summary
proposal / message 这类事件对象不能只靠毫秒时间戳做 ID；choice fallback 也不能在多行 A/B 描述里过早 `break`。

### Details
在紧密循环创建多个 proposal 时，如果 ID 只用 `now_millis()`，同毫秒内会直接撞车，前端按 `id` upsert 时就会吞掉后面的卡片。另一方面，方向文本 fallback 如果按“进入选择组后遇到第一条非 bullet 就结束”，会把多行 A/B 方案截成只剩一条。两者叠加时，最典型现象就是：文本说有 2 个方向，但主区只剩 1 张卡。

### Suggested Action
- 所有可并发创建的 UI 实体都用“时间戳 + 原子序号”或同等级唯一 ID
- 方向 fallback 解析改成“按选项分组收集”，不要把 continuation line 当结束标记

### Metadata
- Source: conversation
- Tags: ids, proposal, parsing, choice-cards

## [LRN-20260326-001] codex-reconnect-noise-is-not-progress
**Logged**: 2026-03-26T00:00:00+08:00
**Priority**: high
**Status**: pending

### Summary
`Reconnecting...` 和 `timeout waiting for child process to exit` 这类 Codex CLI stderr 噪声不能被当成“有进展”，否则工作区协作会假活着卡很久。

### Details
在 Solo 的 Codex 流式链路里，如果把任何 stderr 行都算作 activity，就会不断刷新 `last_activity_at`。一旦 Codex CLI 卡进重连或子进程退出超时，这些噪声会持续“续命”，导致 `idle_timeout` 难以触发，用户只能看到几百秒的假等待。更稳的做法是：
- 将 reconnect / child-process-exit-timeout 识别为噪声状态
- 这些状态不刷新 activity
- 一旦出现连续重连或退出超时，尽早终止本轮请求

### Suggested Action
- 所有 agent/CLI 流式桥接都区分“有效进展”和“连接噪声”
- `stderr` 里的连接异常默认不能重置空闲超时
- 前端不要直接展示 raw reconnect 文案，要产品化成中文状态

### Metadata
- Source: conversation
- Tags: codex-cli, streaming, timeout, reconnect, workspace-collaboration

## [LRN-20260326-002] verify-codex-cli-with-direct-exec
**Logged**: 2026-03-26T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Summary
排查 Solo 的 Codex 工作区协作异常时，要先做一条等价的 `codex exec` 终端对照，先分清是 Solo 包装层的问题，还是 `codex-cli` 本体的问题。

### Details
这次直接在终端里跑与 Solo 接近的 `codex exec --json --output-last-message --cd <workspace>`，先在沙箱里撞到 `Operation not permitted`，说明沙箱网络限制会制造假象；切到真实网络环境后，命令能正常开始读取文件并产出 item 事件。这个对照说明：一旦 Solo 中出现长时间挂起、重连、子进程回收异常，不能直接下结论说“OpenAI 挂了”或“Codex 整体不可用”，要先用终端对照把问题边界缩清楚。

### Suggested Action
- 以后碰到 Solo 内部的 Codex 异常，先做一条最小等价 `codex exec` 对照
- 先排除沙箱/网络环境影响，再判断是 CLI 本体还是 Solo 包装层
- 把这种对照实验当成 Codex 链路排障的固定步骤

### Metadata
- Source: conversation
- Tags: codex-cli, diagnostics, control-experiment, solo

## [LRN-20260326-003] tauri-dev-proxy-needs-no-proxy-or-subprocess-injection
**Logged**: 2026-03-26T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Summary
在 Tauri + Vite 开发环境里，全局设置 `http_proxy/https_proxy/all_proxy` 很容易把 `localhost` 的前端资源和 HMR websocket 一起代理走，导致桌面窗口白屏。更稳的做法是：要么补 `NO_PROXY=localhost,127.0.0.1,::1`，要么只把代理注入给 `codex` 子进程。

### Details
这次用户先在 shell 里导出了普通代理变量，再运行 `npm run tauri dev`，应用直接白屏。补上 `no_proxy/NO_PROXY=localhost,127.0.0.1,::1` 后，前端恢复正常。这说明问题不在 Codex 本身，而是开发态的 WebView/Vite 也被代理了。对 Solo 这种“桌面壳 + 本地 dev server + 外部 CLI”的结构，更合理的方案是把代理边界收窄到 `Command::new(\"codex\")`，而不是污染整个应用进程。

### Suggested Action
- 优先支持独立的 `SOLO_CODEX_*` 代理环境变量，并只注入给 Codex 子进程
- 如果仍需全局代理启动开发环境，必须补 `NO_PROXY=localhost,127.0.0.1,::1`
- 排查白屏时，先检查本地 dev server 是否被代理走

### Metadata
- Source: conversation
- Tags: tauri, vite, proxy, no_proxy, codex-cli, solo

## [LRN-20260329-001] codex-cli-is-best-kept-as-heavy-agent-lane
**Logged**: 2026-03-29T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Summary
在 Solo 这类 human-in-the-loop 工作台里，`codex exec` 更适合承担“重型工作区协作”而不是所有对话与 agent 任务。慢的根因通常不是单一点，而是“每轮新进程 + 全量上下文重建 + 工作区前置查看 + CLI 本地收尾成本”叠加。

### Details
这次回看 Solo 的调用链，当前实现每轮都会重新 `Command::new("codex")` 跑一次 `codex exec --json --output-last-message`，而不是复用一个常驻会话。与此同时，工作区协作会把当前会话消息、附加文件和针对这一轮的规则一起重组到 prompt，再由 Codex 在工作区里先查文件再输出建议。结果是：即便模型服务端本身正常，本地 CLI 冷启动、进程退出、stderr 噪声、以及工作区分析前置步骤都会让“每次调用都很慢”变成默认体感。更适合的产品分层是：
- 快速聊天走更轻的对话通道
- 重型工作区协作才调用 Codex CLI
- 预览、审批、方向卡等产品逻辑尽量留在 Solo 自己实现，不把它们都外包给 CLI

### Suggested Action
- 不要让 `codex exec` 承担所有聊天和 agent 任务
- 先做两条通道：`fast chat lane` 与 `heavy workspace lane`
- 对当前 Codex 路径继续做小优化：缓存登录状态、缩短 prompt、减少重复读取和无关上下文

### Metadata
- Source: conversation
- Tags: codex-cli, performance, architecture, workspace-collaboration, solo

## [LRN-20260329-004] zsh-eza-alias-breaks-bare-path-completion
**Logged**: 2026-03-29T20:53:00+08:00
**Priority**: low
**Status**: pending

### Summary
在 `zsh + zim` 环境里，直接写 `alias ls='eza --icons'` 可能让 `ls /path<Tab>` 这种裸路径补全失效，而 `ls -al /path<Tab>` 仍能工作。

### Details
这次本机复现里，`ls /etc/hos<Tab>` 会报 `no matches found`，但 `ls -al /etc/hos<Tab>` 能补成 `/etc/hosts`。根因不是 `compinit` 整体损坏，而是：
- `zim` 的 `utility` 模块先给 `ls` 设了 alias
- 再叠加 `alias ls='eza --icons'` 后，`ls` 的 completion 在“无选项直接补路径”这个分支上走偏

更稳的修法是：
- 先 `unalias ls`
- 再用函数包装：`ls() { eza --icons \"$@\" }`
- 显式绑定补全：`compdef _eza ls`

### Suggested Action
- 以后在 `zsh` 里给 `ls` 换成 `eza` 时，优先用“函数 + `compdef _eza ls`”，不要只写 alias
- 如果用了 `zim` / `oh-my-zsh` 一类框架，先检查它们是否已经预设了 `ls` alias，再覆盖

### Metadata
- Source: conversation
- Tags: zsh, zim, eza, completion, alias

## [LRN-20260328-001] steam-ntfs3-library-not-executable
**Logged**: 2026-03-28T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Summary
Linux 下 Steam 报“库文件夹不可执行”时，即使挂载参数里没有 `noexec`，目标盘如果是 `ntfs3` 挂载的 NTFS，也常会因为权限语义和可执行位测试失败被拒绝。

### Details
一次本地排查中，`/mnt/archive` 的挂载信息为：
- `type ntfs3`
- `rw,relatime,uid=0,gid=0,acl,iocharset=utf8,prealloc`

虽然没有 `noexec`，但 Steam 仍报告库目录不可执行。这个场景下，问题不该只盯着 `noexec`，还要优先检查：
- 文件系统是否为 NTFS / `ntfs3`
- 挂载是否把所有权映射给了当前用户
- 新建文件是否具备 Steam 可执行性检查所需的 Unix 权限语义

更稳妥的建议仍然是把 Steam 库放到 `ext4`、`btrfs` 之类的 Linux 原生文件系统；NTFS 即便强行调挂载参数，也仍可能在 Proton、符号链接、大小写敏感和文件名语义上继续踩坑。

## [LRN-20260412-001] neovim-0-12-iter-matches-returns-node-lists
**Logged**: 2026-04-12T00:00:00+08:00
**Priority**: high
**Status**: pending

### Summary
Neovim 0.12 的 `vim.treesitter.Query:iter_matches()` 会为每个 capture 返回 `TSNode[]`，即使传了 `{ all = false }` 也不是单个 `TSNode`。

### Details
这次排查 `aerial.nvim` 崩溃时，根因不是 `TSNode:start()` API 被删，而是插件还假设 `matches[id]` 是单个节点，直接 `node:start()`。在 `nvim 0.12.1` 下，实际返回是节点数组；哪怕只有一个 capture，也会是长度为 1 的表。对这类老插件，最小兼容修法通常是把 capture 先收敛成 `nodes[#nodes]`，这样最接近旧版 `iter_matches(..., { all = false })` 的行为。

### Suggested Action
- 以后遇到 `attempt to call method 'start' (a nil value)` 且栈在 treesitter query 消费侧时，优先怀疑 capture 从单节点变成了节点数组
- 给兼容补丁时，先试 `local node = nodes[#nodes]`
- 如果还有量词 capture（如 `(word)+ @name`），再检查是否要额外恢复旧行为

### Metadata
- Source: local debugging + Neovim runtime inspection
- Tags: neovim, treesitter, api-compat, plugin-debugging

## [LRN-20260412-002] verify-active-neovim-config-before-attributing-errors
**Logged**: 2026-04-12T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Summary
排查 Neovim 报错时，不要从旁边可见的 dotfiles 仓库反推当前配置；先看实际生效的 `~/.config/nvim` 和它的 `lazy-lock.json`。

### Details
这次一开始误把问题归到另一个 `nvim-lazyvim` 目录，只因为它在工作区附近且也有插件锁文件。用户纠正后，直接检查 `~/.config/nvim` 才确认真实运行的是 AstroNvim 模板，并且 `aerial.nvim` 是 AstroNvim 核心 spec 带进来的。这个误判路径很浪费时间，而且容易把修改打到错误位置。

### Suggested Action
- Neovim 问题先查 `~/.config/nvim/init.lua`、`lua/lazy_setup.lua`、`lazy-lock.json`
- 只有确认 `readlink -f ~/.config/nvim` 指向目标仓库后，才把问题归属到那个仓库

### Metadata
- Source: user correction + local debugging
- Tags: neovim, config, debugging, attribution

### Suggested Action
- 以后遇到 Steam 的“库不可执行”问题时，先区分两类：`noexec`，或 `ntfs3` / NTFS 导致的权限语义和可执行性检查失败
- 默认优先推荐 Linux 原生文件系统，不把 NTFS 当作首选 Steam 库盘

### Metadata
- Source: conversation
- Tags: steam, linux, ntfs3, mount, permissions

## [LRN-20260328-002] qt-headless-offscreen-needs-empty-platformtheme
**Logged**: 2026-03-28T00:00:00+08:00
**Priority**: low
**Status**: pending

### Summary
在这台 Linux 环境里做 Qt 无头启动验证时，单独设置 `QT_QPA_PLATFORM=offscreen` 还不够；如果不同时清空 `QT_QPA_PLATFORMTHEME`，Qt 仍可能走到 GTK theme 插件并尝试连接显示。

### Details
这次对 `Qt Quick + C++` 桌面程序做无头 smoke test 时，直接用：
- `QT_QPA_PLATFORM=offscreen ./app`

仍会触发 GTK 侧报错，表现为尝试访问 `DISPLAY=:1`。改成：
- `QT_QPA_PLATFORM=offscreen`
- `QT_QPA_PLATFORMTHEME=`

后，应用可以正常在 `timeout` 下启动并保持运行，说明问题不在 QML 或二进制本身，而是 theme 插件链路把无头验证拖回了图形会话依赖。

### Suggested Action
- 以后在这台环境里做 Qt 无头验证时，默认同时设置 `QT_QPA_PLATFORM=offscreen` 和空的 `QT_QPA_PLATFORMTHEME`
- 如果只看到 GTK / display 报错，不要先怀疑 QML 绑定，先检查 theme 平台变量

### Metadata
- Source: conversation
- Tags: qt, qml, offscreen, gtk, headless, linux

## [LRN-20260329-001] clean-complex-architecture-design
**Logged**: 2026-03-29T00:00:00+08:00
**Priority**: high
**Status**: promoted

### Summary
推进项目时，要把“持续清理架构上的复杂设计”当成默认动作，而不是只在功能完成后再回头补救。

### Details
用户明确强调，这里的重点不是笼统的“技术债务”，而是主动识别并清理架构层面的复杂设计，例如：
- 重复状态和重复投影长期并存
- 临时补丁式条件分支不断外溢
- 名义上抽象、实际增加理解负担的层级
- 旧模型没退场、新模型又直接叠上去的双轨结构

### Suggested Action
每次推进功能或重构时，都顺手检查是否能删掉旧路径、合并重复概念、收缩边界；不要把“清复杂设计”推迟成一个永远不会来的大重构。

### Metadata
- Source: conversation
- Tags: architecture, simplification, complexity, engineering

## [LRN-20260329-002] waypaper-2-7-no-awww-backend
**Logged**: 2026-03-29T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Summary
`waypaper 2.7` 已经不支持 `awww` 后端；如果旧 dotfiles 还保留“`awww-daemon` + `waypaper` 直接设壁纸”的假设，Wayland 会话里很容易退回 `feh` 或根本不再真正设主壁纸。

### Details
这次在 Niri / Wayland 环境里排查壁纸启动失败时，发现：
- `niri` 仍会启动 `awww-daemon`
- 但 `waypaper --help` 的 backend 选项里已经没有 `awww`
- 旧配置若写成 `backend = feh`，`waypaper --random` 会直接尝试 `feh`，在 Wayland 下报 X display 错误
- 更稳的兼容方案是把 `waypaper` 改成 `backend = none`，再通过 `post_command` 调自己的 `awww` 脚本
- `waypaper --backend none --random` 这类 CLI 测试会把 backend 和当前壁纸写回 `config.ini`，测试本身会改配置

### Suggested Action
- 如果桌面壁纸后端是 `awww`，不要再假设 `waypaper` 能直接驱动它
- 优先选两条路线之一：
  - 全链路改成 `swww`
  - 保留 `awww`，但让 `waypaper` 只负责选图，真正设壁纸交给 `post_command`
- 调试 `waypaper` CLI 时，先意识到它可能会把参数持久化到配置文件里

### Metadata
- Source: conversation
- Tags: linux, wayland, niri, waypaper, awww, wallpaper

## [LRN-20260329-003] dotfile-sync-prefer-cp-aL-when-rsync-missing
**Logged**: 2026-03-29T00:00:00+08:00
**Priority**: low
**Status**: pending

### Summary
这台机器未必安装 `rsync`；做 dotfiles 同步时，如果源路径里还混有软链，直接用 `cp -aL` 或逐文件复制通常比先假设 `rsync` 存在更稳。

### Details
这次把当前 home 配置同步到另一个本地仓库时，原本打算用 `rsync -aL` 跟随软链并落成真实文件，但命令直接失败，原因是系统里没有 `rsync`。后续改成：
- 对单文件使用 `cp -aL`
- 对目录使用 `find -L ...` 展开后逐文件复制

这样既能跟随 `~/.config` 下指向外部 dotfiles 仓库的软链，又能避免把绝对路径软链原样带进目标仓库。

### Suggested Action
- 以后做本地 dotfiles 仓库同步时，先不要默认依赖 `rsync`
- 如果目标是“把生效配置存成真实文件”，优先考虑 `cp -aL` / `find -L`

### Metadata
- Source: conversation
- Tags: linux, dotfiles, rsync, symlink, cp

## [LRN-20260329-005] live-dotfiles-repo-cleanup-with-exclude-and-assume-unchanged
**Logged**: 2026-03-29T21:20:00+08:00
**Priority**: low
**Status**: pending

### Summary
如果一份“当前正在生效的 dotfiles 仓库”同时跟踪手写配置、自动生成主题和运行时状态，想把 `git status` 收干净时，通常要分两层处理：未跟踪运行时文件放进本地 `.git/info/exclude`，已跟踪但会被自动重写的文件标记为 `assume-unchanged`。

### Details
这次 SHORiN 的 live dotfiles 仓库挂在当前 `~/.config` 上，导致在 `~/.config/niri` 里执行 `git status` 也会看到整仓的变化。噪声来源分成两类：
- 未跟踪运行时产物：如 `fcitx5/rime` 数据库、Matugen 生成的图标、缓存文件
- 已跟踪但会被 `matugen` / 壁纸切换反复改写的主题文件

仅靠 `.gitignore` 只能隐藏未跟踪文件，不能隐藏已跟踪文件的修改。更稳的本地清理方式是：
- 把未跟踪运行时产物写进这份仓库自己的 `.git/info/exclude`
- 对已跟踪但不想在本机反复看到的自动生成文件执行 `git update-index --assume-unchanged`

这样可以把 `git status` 收敛到真正重要的本地改动，同时不改上游仓库内容。

### Suggested Action
- 以后碰到“live dotfiles 仓库天天变脏”的情况，先区分“未跟踪运行时文件”和“已跟踪自动生成文件”
- 未跟踪文件优先放 `.git/info/exclude`
- 已跟踪噪声文件优先用 `git update-index --assume-unchanged`

### Metadata
- Source: conversation
- Tags: git, dotfiles, exclude, assume-unchanged, linux

## [LRN-20260329-006] non-owned-upstream-clone-needs-explicit-approval-for-local-git-metadata
**Logged**: 2026-03-29T21:23:00+08:00
**Priority**: medium
**Status**: pending

### Summary
即使不会产生 commit 或 push，在非用户私有的上游 clone 里修改 `.git/info/exclude`、`assume-unchanged`、`skip-worktree` 等本地 Git 元数据，也应先得到用户明确同意。

### Details
这类操作虽然只影响本机工作树，不会进入仓库历史，也不会推送到上游，但它们会改变用户看到的 `git status` 和本地工作流认知。对“只是拿来跟上游同步、不是自己的仓库”的 clone，默认不能擅自做这类本地 Git 层面的状态收口。

### Suggested Action
- 对非用户私有仓库，区分“代码/配置文件改动”和“本地 Git 元数据改动”
- 前者按正常协作处理
- 后者即使是本地-only，也先征得明确许可

### Metadata
- Source: conversation
- Tags: git, workflow, boundaries, upstream-clone

## [LRN-20260329-007] rime-safe-keybinding-override-needs-custom-preset-file
**Logged**: 2026-03-29T23:05:00+08:00
**Priority**: low
**Status**: pending

### Summary
在 Rime 里想删除默认 `ascii_mode` 之类的热键时，直接给 `key_binder/bindings` 追加列表通常只会叠加到默认绑定后面；更稳的做法是单独创建 `key_bindings.custom.yaml` preset，再用 `key_binder.bindings.__patch` 重组整套绑定。

### Details
这次要去掉 `rime_ice` 默认的 `Control+Shift+2` / `Control+Shift+@` 中英切换键。直接在 `rime_ice.custom.yaml` 里写：
- `patch: key_binder/bindings: [...]`

结果 build 后会把新绑定附加到默认绑定后面，旧的 `ascii_mode` 热键仍然存在。  
而把自定义绑定拆到独立的 `key_bindings.custom.yaml` 中，再在 `rime_ice.custom.yaml` 里写：
- `patch.key_binder.bindings.__patch`
- 引用 `key_bindings:/...`
- 再引用 `key_bindings.custom:/...`

就能真正替换默认导入的绑定集合。修改后需要执行：
- `rime_deployer --build <user_data_dir> /usr/share/rime-data <build_dir>`

仅 `fcitx5-remote -r` 不会重新编译 schema。

### Suggested Action
- 以后覆盖 Rime 默认快捷键时，优先走“独立 custom preset + `bindings.__patch` 重组”方案
- 需要验证最终效果时，直接检查 build 后的 `*.schema.yaml`

### Metadata
- Source: conversation
- Tags: rime, fcitx5, keybindings, patch, linux
## [LRN-20260329-008] systemd-agent-sync-stopped-because-github-ssh-22-was-closed
**Logged**: 2026-03-29T00:00:00+08:00
**Scope**: 通用 / GitHub 同步 / systemd

### Context
- 本机 `sync-agent-config.timer` 仍在运行，问题不在 timer 丢失
- `journalctl --user -u sync-agent-config.service` 显示：
  - 2026-03-28 运行成功，但 `no changes to sync`
  - 2026-03-29 运行失败，错误为 `Connection closed by 198.18.0.40 port 22` 和 `fatal: Could not read from remote repository.`
- 同步脚本默认 remote 是 `git@github.com:0x07c4/agent.git`
- 本机 `ssh -G github.com` 生效配置仍是 `port 22`

### Insight
- 这类“仓库几天没同步”不一定是定时器坏了，也可能是：
  - 某天没有内容变化，所以不会产生新 commit
  - 下一次有变化时，systemd 里的 `git push` 因 SSH 22 端口被网络/代理链路关闭而失败
- 若 GitHub SSH 22 在当前网络环境不稳定，应该优先给 GitHub 配 `ssh.github.com:443` 兜底，而不是只盯着定时器状态

### Recommended Pattern
- 排查顺序优先：
  1. 看远端最新 commit 时间
  2. 看 `systemctl --user status ...timer`
  3. 看 `journalctl --user -u ...service`
  4. 看 `ssh -G github.com`
- 若确认是 SSH 22 问题：
  - 给 GitHub 加 443 fallback
  - 或让同步脚本改走更稳的 remote/sshCommand

## [LRN-20260330-001] clash-verge-persistent-custom-rules-live-in-profile-enhancement-files
**Logged**: 2026-03-30T00:00:00+08:00
**Scope**: 通用 / Clash Verge / 代理规则

### Context
- `~/.local/share/io.github.clash-verge-rev.clash-verge-rev/profiles.yaml` 里记录了当前订阅绑定的增强文件
- 当前活跃远程订阅 `RelN7DkJIAEK` 的持久自定义入口是：
  - `rules: rg80DP4XOivv`
  - `merge: mFTazFJ2QDeW`
  - `script: sLkHIMro0dvo`
- 外层 `config.yaml` / `clash-verge.yaml` 只是生成产物，不适合直接改

### Insight
- 在 Clash Verge Rev 里，要让自定义规则跨订阅更新仍然保留，应该改 `profiles/*.yaml|js` 里的增强模板，而不是改生成出来的总配置
- 例如给 GitHub SSH 加固定路由，应该写到 `profiles/rg80DP4XOivv.yaml` 的 `prepend:` 里

### Recommended Pattern
- 先读 `profiles.yaml`，确认当前订阅实际引用了哪份 `rules/merge/script`
- 规则优先改增强模板：
  - 域名/分流规则改 `rules` 模板
  - 结构性配置改 `merge` 模板
  - 复杂变换改 `script` 模板
- 不要把长期修复直接落到生成的 `config.yaml`

## [LRN-20260413-001] make-has-a-built-in-tex-variable-that-conflicts-with-latex-projects
**Logged**: 2026-04-13T22:15:00+08:00
**Scope**: 通用 / Makefile / LaTeX

### Context
- 在 LaTeX 项目里写了：
  - `TEX ?= main.tex`
  - `$(LATEXMK) ... $(TEX)`
- 结果 `make pdf` 实际展开成了 `latexmk ... tex`
- 原因是 GNU Make 自带内置变量 `TEX=tex`，`?=` 不会覆盖它

### Insight
- 在 Makefile 里不要把 LaTeX 源文件变量命名成 `TEX` 再配合 `?=`
- 更稳的命名是：
  - `SOURCE`
  - `MAIN_TEX`
  - `TEX_FILE`

### Recommended Pattern
- LaTeX 项目的入口变量优先用 `SOURCE ?= main.tex`
- 如果必须沿用 `TEX`，要显式用 `:=` 或 `=` 覆盖，而不是 `?=`

## [LRN-20260413-002] typst-asset-paths-and-defaulted-params-need-extra-care
**Logged**: 2026-04-13T22:31:00+08:00
**Scope**: 通用 / Typst / 文档模板

### Context
- 在 `resume.typ` 里从 `typst/lib.typ` 调用了 `image(profile.photo, ...)`
- `profile.photo` 原本写成 `"WechatIMG860.jpg"`，Typst 实际会去找 `typst/WechatIMG860.jpg`
- 另外自定义函数 `experience-entry(company, title, date, bullets: (), groups: ())` 在调用时，前 3 个参数可以位置传参，但带默认值的后两个参数需要用命名方式传入

### Insight
- Typst 里资源路径是否相对“入口文件”并不可靠；当资源函数写在被导入模块里时，实际解析位置要按该模块验证
- 自定义函数里带默认值的参数，调用时不要想当然全写成位置参数

### Recommended Pattern
- 跨文件组织 Typst 项目时，图片等资源路径先按“调用资源函数的模块目录”验证
- 对带默认值的自定义参数，优先显式写成 `param: value`

## [LRN-20260417-002] eastmoney-index-quotes-need-index-specific-secids
**Logged**: 2026-04-17T00:00:00+08:00
**Scope**: 通用 / Eastmoney / A股指数行情

### Context
- 在东财 `push2.eastmoney.com/api/qt/ulist.np/get` 里，请求指数行情时直接把代码按个股规则拼成 `0.000001` 会拿到 `平安银行`
- `000001` 这类代码在东财里既可能是个股，也可能是指数，不能只靠“6 开头上交所 / 其余深交所个股”规则判断

### Insight
- 指数行情要用指数专属 `secid`
- 至少这几个常用指数要特殊映射：
  - `000001 -> 1.000001` 上证指数
  - `399001 -> 0.399001` 深证成指
  - `399006 -> 0.399006` 创业板指

### Recommended Pattern
- `watchlist` / 个股报价继续走普通 A 股 `secid` 规则
- `market feed` / 指数报价单独走 index-specific `secid` 映射，不要复用个股映射函数

## [LRN-20260417-003] cairo-toy-text-api-is-not-safe-for-cjk-ui-labels
**Logged**: 2026-04-17T00:00:00+08:00
**Scope**: 通用 / GTK / Cairo / 中文 UI

### Context
- 在 GTK4 + Cairo 绘图里用 `cr.show_text("昨收 37.54")` 这类 toy text API 直接画中文标签时，容易出现 tofu 方块或字形缺失
- 图表里的英文数字正常，但中文标签会在运行时暴露问题

### Insight
- Cairo toy text API 只适合很简单的 ASCII 文本，不适合正式 UI 的 CJK 标签
- 图表里只要需要画中文，就应该切到 `PangoCairo`

### Recommended Pattern
- Canvas 上的中文标签统一用 `PangoCairo.create_layout()` + `show_layout()`
- 如果只是临时调试文本，可以保留 Cairo toy text；正式展示文案不要继续依赖它

## [LRN-20260419-001] codex-login-parity-requires-cli-status-and-stderr-forwarding
**Logged**: 2026-04-19T00:00:00+08:00
**Scope**: 通用 / Codex / 桌面壳 / 登录体验

### Context
- 做基于 Codex 的桌面壳时，如果只会读 `~/.codex/auth.json` 或只会静默拉起 `codex login`，用户体验和 Codex CLI 本体并不一致
- `codex login status` 才是 Codex 自己的登录状态判定入口，而 `codex login` 的 stderr 里会包含浏览器未自动打开时的 URL / 设备登录提示

### Insight
- 想对齐 Codex 的登录逻辑，不能把 `auth.json` 当成协议源
- 更不能在启动登录时把 stderr 全吞掉，否则用户在浏览器未弹起时没有任何恢复路径

### Recommended Pattern
- 登录状态优先以 `codex login status` 为准
- 登录启动时转发 `codex login` 的 stderr / stdout 进前端或日志
- provider 选择和登录状态要解耦，不要在 UI 或发送链路里偷偷把别的 provider 折返成 `codex_cli`

## [LRN-20260423-001] npx-skills-add-installs-project-local-skills-under-dot-agents
**Logged**: 2026-04-23T00:00:00+08:00
**Scope**: 通用 / Codex / skills CLI

### Context
- 在仓库目录里执行 `npx skills add <repo@skill> -y` 安装 skills 时，结果没有落到全局 `~/.codex/skills`
- 实际安装摘要显示目标路径是当前项目下的 `./.agents/skills/<skill-name>`

### Insight
- `skills add` 在项目上下文里默认更像“项目级安装”，不是“全局安装”
- 如果只是想给当前仓库补能力，这个行为是合理的；如果目标是全局复用，就要显式用 `-g`

### Recommended Pattern
- 需要项目级 skill 时，直接在 repo 内执行 `npx skills add ... -y`
- 需要全局 skill 时，显式使用 `npx skills add ... -g -y`
- 安装后立刻检查输出路径，避免把第三方 skill 误当成已经全局可用

### Additional Note
- 当前环境下，`-g` 的全局安装目标是 `~/.agents/skills/<skill-name>`，不是 `~/.codex/skills`
