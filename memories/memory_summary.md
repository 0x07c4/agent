## User Profile

用户主要在 Linux 环境下做 AI 工具、前端/桌面端和工作区协作相关开发，当前长期项目包括 `Solo` 与 `cocoa`。沟通上偏好中文、直接、少废话，不喜欢只讲泛泛方案，更看重可落地判断、明确边界和实际改动。工作方式上明显偏好 human-in-the-loop，也会把不同模型/agent 分工得很清楚；如果某次任务被定义成 review、监督或纠偏，就不希望 agent 越界直接实现。用户也持续维护 Codex 的本地 memory，希望未来 agent 能少走弯路、少做无效扫描，并把稳定经验沉淀成可复用规则。

## User preferences

- 默认使用中文，保持直接、简洁、少废话。
- 当任务是 review 别人的实现时，优先保持 review 身份；用户明确说过“你不要自己修复，如果有问题对deepseek提出修改意见”。
- 如果工作流已经分工成 “Codex reviews, DeepSeek implements”，不要自己漂移成实现者，除非用户重新指定。
- 不要把“测试全绿”当成任务完成的充分条件；如果用户要的是 contract / prompt / handoff 语义检查，就继续按这个层面审。
- 进度更新只做状态汇报，不要写成征求许可的语气。
- 能复用现有 skill、脚本、工作流时优先复用，不要无谓重造。

## General Tips

- 进入任务前先读 `PROFILE.md` 和 `ACTIVE.md`；这个 memory repo 的全局规则依赖它们。
- 处理高噪声工程命令时优先用 `rtk`，尤其是 `git diff/status/log`、测试、lint、构建、包管理等；短小精确命令不需要包装。
- 对 `cocoa` 里的 review/handoff 任务，先找 `docs/ai-handoff.md` 之类的 contract 文件，再看实现和测试，效率比只读 diff 更高。
- 做 memory/文档整理时优先重组结构，不要默认在文件末尾持续追加。

## What's in Memory

### $HOME/workspace/cocoa

#### 2026-05-03

- Review DeepSeek `/escalate last` handoff: cocoa, DeepSeek, /escalate last, docs/ai-handoff.md, prompt semantics, context clipping
  - desc: 汇总了 `$HOME/workspace/cocoa` 中一次 review-only handoff：DeepSeek 实现 first-class `/escalate last`，Codex 负责验收语义与 handoff contract，而不是自己修复。先搜这个主题可快速找到 `MEMORY.md` 里的 review 边界、检查顺序、风险点和验证命令。
  - learnings: `rtk python -m unittest tests.test_runtime tests.test_cli` 虽然 79 项通过，仍然漏不出 prompt-boundary 问题；重点要检查 pending/completed/rejected 分类，以及 stdout/stderr/diff 是否做了 clipping。

### Older Memory Topics

暂无更早的高信号 Phase 2 主题。
