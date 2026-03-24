# FEATURE_REQUESTS

## [FEAT-20260324-001] global-memory-loop
**Logged**: 2026-03-24T00:00:00+08:00
**Priority**: high
**Status**: pending

### Requested Capability
为 Codex 建立全局记忆目录，保存 Lessons Learned、短期记忆和可复用技巧。

### User Context
用户希望 Codex 不只是当前会话记住上下文，而是能够跨任务保留高价值经验。

### Suggested Implementation
- 用 `~/.codex/memories/` 作为全局 memory 目录
- 用 `~/.codex/AGENTS.md` 作为统一入口
- 通过 `PROFILE.md / ACTIVE.md / LEARNINGS.md / ERRORS.md / FEATURE_REQUESTS.md` 分层管理

## [FEAT-20260324-002] visual-decision-flow
**Logged**: 2026-03-24T00:00:00+08:00
**Priority**: high
**Status**: pending

### Requested Capability
把 Solo 的工作区协作从长文字分析改造成更视觉化的决策流。

### User Context
用户明确希望把枯燥文字转换成视觉元素，参考卡片式决策体验。

### Suggested Implementation
- 方向卡
- 预览卡
- 用户确认
- 右栏退成上下文摘要，不抢主区

## [FEAT-20260324-003] codex-scope-optimization
**Logged**: 2026-03-24T00:00:00+08:00
**Priority**: medium
**Status**: pending

### Requested Capability
进一步降低 Codex 工作区协作的无效扫描和等待时间。

### User Context
当前用户非常在意 Codex 执行慢、等待长、范围失控的问题。

### Suggested Implementation
- `.ignore`
- 更强的 prompt scope 控制
- 目录深度限制
- 明确点名文件时再放宽范围
