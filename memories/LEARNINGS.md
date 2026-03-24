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
