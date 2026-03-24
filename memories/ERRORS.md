# ERRORS

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
