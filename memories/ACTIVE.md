# ACTIVE

## Always Apply
- [ACT-001] 默认使用中文，保持直接、简洁、少废话。
  Source: PROFILE.communication
- [ACT-002] 不要靠用户输入内容猜模式；`对话 / 工作区协作` 必须由显式状态决定。
  Source: Solo 产品规则
- [ACT-003] 除非命令需要 `sudo`、系统提权，或属于高风险不可逆操作，否则可以直接执行，不必逐条向用户确认。
  Source: user correction / conversation
- [ACT-004] 方向卡只能负责“预览与确认选择”，不能在用户没看到具体内容前出现“应用改动”。
  Source: Solo 方向卡交互修正
- [ACT-005] 处理 Codex 工作区协作时，要优先考虑范围控制；`node_modules`、`dist`、构建产物等路径应默认被忽略。
  Source: `.ignore` / workspace scope
- [ACT-006] 在任务过程中持续做自我提升：把非显而易见、可复用、可能再次出现的经验、纠正、技巧和能力缺口沉淀到 memory，并在足够稳定后提升到 `ACTIVE.md`。
  Source: user explicit preference / self-improving loop
- [ACT-007] 只要任务可以明确拆解成互不阻塞的子任务，就优先使用多个 agent 并行完成。
  Source: user explicit preference / collaboration strategy
- [ACT-014] 当用户可能在中途插入问题时，优先把非阻塞子任务放到多个 agent 后台并行处理，主线程保持可随时切换与响应。
  Source: user explicit preference / interruption-friendly collaboration
- [ACT-008] 在任务中优先使用合适的 skill、现成工具、已有脚本和现有工作流完成目标，不要不必要地从头造轮子。
  Source: user explicit preference / tool reuse
- [ACT-009] 进度更新只做状态汇报，不要写成像在征求许可或再次发问的句式。
  Source: user correction / conversation
- [ACT-010] 全局 memory 只记录跨项目、可公开、可复用的稳定规则；不要写入 repo 级背景、命名讨论、产品边界或其他项目私有上下文。
  Source: user correction / memory boundary
- [ACT-011] 推进项目时要持续主动清理架构上的复杂设计；优先删除重复状态、补丁式分支和失控分层，不要在既有复杂度上继续叠加新能力。
  Source: user explicit preference / architecture hygiene
- [ACT-012] 遇到产品定位、架构方向、长期取舍或个人偏好相关任务时，优先回读 `~/workspace/notes`；这里承载用户的长期思考。
  Source: user explicit preference / long-term notes
- [ACT-013] `~/workspace/notes` 会同步到 `git@github.com:0x07c4/notes.git`，可视为用户长期思考的稳定、版本化来源。
  Source: user explicit preference / notes sync
- [ACT-015] 记录或更新文档时，优先梳理、归纳和重组结构，不要默认在原文末尾持续追加，避免文档退化为流水账。
  Source: user correction / documentation maintenance preference
- [ACT-016] 做 plan 和实现推进时，要同步沉淀可交接的结构化文档，保证切换到其他 agent 后也能快速理解上下文；优先更新现有文档或状态说明，不要只把关键上下文留在对话里。
  Source: user explicit preference / handoff continuity
- [ACT-017] 需要纳入个人 agent 全局配置与 `agent.git` 同步链的 skills，一律以 `~/.codex/skills` 为标准安装位置；`~/.agents/skills` 只视为外部工具自己的运行目录，不作为主真源。
  Source: user explicit preference / agent global config workflow

## Current Focus
- 把 Solo 从“长文字分析”继续收成“视觉化决策流”：
  - 方向卡
  - 预览卡
  - 用户确认
- 持续压缩 Codex 在工作区协作时的无效扫描和等待时间。

## Maintenance Rules
- 只保留跨任务高价值规则。
- 当前焦点可定期收缩或替换，避免无限膨胀。

## Last Reviewed
- 2026-03-30
