# LEARNINGS

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
