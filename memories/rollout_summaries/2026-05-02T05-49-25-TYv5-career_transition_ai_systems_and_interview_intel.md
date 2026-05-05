thread_id: 019de73c-0dca-7383-92b1-2625742bffef
updated_at: 2026-05-02T09:51:21+00:00
rollout_path: $HOME/.codex/sessions/2026/05/02/rollout-2026-05-02T13-49-25-019de73c-0dca-7383-92b1-2625742bffef.jsonl
cwd: $HOME/workspace

# 先搭求职仓库骨架，再把目标收成 AI systems 主线，并新增面经情报流

Rollout context: 用户先要一个本地 Git 仓库记录离职/求职过程，随后把职业目标从汽车行业迁移到 AI systems / agent runtime / developer tools 优先，最后希望项目还能定期收集最近面经，作为准备输入。工作目录主要在 `$HOME/workspace/career-transition`，仓库已初始化并绑定 private GitHub 远端。

## Task 1: 创建并维护 career-transition 求职仓库

Outcome: success

Preference signals:

- 用户说“我最近打算换工作，我想先创建一个仓库来记录我的学习\面试\找工作的过程” -> 说明他希望把求职过程结构化沉淀到版本管理里，而不是只在聊天里讨论。
- 用户后续追问“这个仓库应该如何使用呢” -> 说明他更需要可执行的仓库工作流，而不是泛泛建议。
- 用户说“我现在也没有清晰的目标，你能帮我设计吗” -> 说明目标设计应先产出可迭代版本，再逐步收敛。

Key steps:

- 在 `$HOME/workspace/career-transition` 创建并初始化 Git 仓库，首次提交了求职记录骨架。
- 写了 `README.md`、`GOALS.md`、`applications/`、`interviews/`、`learning/`、`targets/`、`templates/` 等结构。
- 之后把仓库目标逐步重写为“离开汽车行业，迁移到更通用的软件系统领域”，并最终提升为“AI systems / agent runtime / developer tools”为主线。
- 通过多次提交把 `GOALS.md`、`targets/role-archetypes.md`、`positioning/transition-narrative.md`、`learning/` 下的路线和叙事同步成一致版本，并推送到 private GitHub 远端。

Failures and how to do differently:

- 一开始把目标设计得偏宽，包含 performance / backend / edge platform 等多个方向；后续用户明确“想离开汽车行业”，并进一步把 AI 相关岗位提升优先级，说明以后一开始就要先问清行业归宿和主线优先级。
- 之前曾尝试为离职准备通知模板，但用户明确说 Bosch 有完善离职流程，不需要这种文档；以后遇到大厂/成熟公司离职场景，优先控制交接边界和求职节奏，不要默认制作离职通知文本。

Reusable knowledge:

- 仓库最终定位是：`career-transition` 作为“求职决策系统”，而不是日记。
- 当前主线顺序已调整为：`AI systems / agent runtime / developer tools` > `performance / observability`（支撑底盘）> `edge platform` / `backend infrastructure`（备选入口）。
- 用户明确不想继续做汽车行业软件；Bosch / parking / SDV 经历只作为可迁移系统能力的证据，不是下一份工作的行业目标。
- 用户明确偏好中文、直接、少废话；仓库文档也应偏结构化、可复用、可交接。

References:

- [1] Repository path: `$HOME/workspace/career-transition`
- [2] Main goal file: `GOALS.md` now records `AI systems / agent runtime / developer tools` as `P0`
- [3] Role filter file: `targets/role-archetypes.md`
- [4] Transition narrative: `positioning/transition-narrative.md`
- [5] Final local commit before later additions: `a40fb4b Prioritize AI systems career track`

## Task 2: 为 career-transition 新增面经情报流

Outcome: partial

Preference signals:

- 用户说“这个项目还可以做一件事，就是提供收集面经的能力，定期收集最近的面试经验” -> 说明他想把外部面经作为持续输入源，而不是一次性收藏。
- 用户随后质疑“你只是增加了一些文档，怎么做到定期收集面经呢” -> 说明他要的是可执行的周期化收集机制，而不是纯静态模板。
- 结合前文主线，他更关心 AI systems / agent runtime / devtools 的面试情报，而不是泛泛的所有行业面经。

Key steps:

- 新增了 `interview-intel/` 目录作为“面经情报流”，包括：
  - `README.md`：定义面经不是收藏夹，而是情报流；
  - `sources.md`：关键词、公司、过滤规则；
  - `radar.csv`：结构化主表；
  - `weekly/2026-W19.md`：周报模板实例。
- 新增模板：`templates/interview-intel-note.md`、`templates/interview-intel-weekly.md`。
- 新增脚本：`scripts/new-interview-intel-week.sh`，用于创建新的周报文件。
- 在 `README.md` 和 `AGENTS.md` 里写明：每周收集、去重、归类、提炼趋势，再回流到 `learning/`、`interviews/` 和简历/项目表达；只保存链接、摘要、问题类型和准备动作，不整篇复制原文。

Failures and how to do differently:

- 目前只完成了目录/模板/脚本骨架，没有真正实现联网抓取或自动定期拉取面经；当用户追问“怎么做到定期收集”时，说明还缺少真正的执行器。
- 下一步需要从“静态文档”升级到“实际运行流程”：例如定时脚本、手动周任务、检索命令、结果写入 `radar.csv` 和周报，而不是只停留在结构设计。
- 这次推送也受限于联网提权额度，导致最后一版提交本地已完成但远端未推上去；未来如果要把周期任务自动化，可能还需要明确可用的联网/提权边界。

Reusable knowledge:

- 面经情报应该优先服务 AI systems / agent runtime / developer tools 主线，其次才是 performance / observability、edge platform、backend infrastructure。
- 面经收集规则：优先最近 7-14 天、带公司/岗位/轮次/问题类型、可转化为准备动作的内容；避免整篇复制原文、避免记录隐私或内部保密信息。
- 面经来源应围绕 AI infra、agent runtime、AI coding、developer tools、工程效率平台、performance/observability 等关键词。

References:

- [1] New files: `interview-intel/README.md`, `interview-intel/sources.md`, `interview-intel/radar.csv`, `interview-intel/weekly/2026-W19.md`
- [2] Templates: `templates/interview-intel-note.md`, `templates/interview-intel-weekly.md`
- [3] Script: `scripts/new-interview-intel-week.sh`
- [4] Updated policy location: `AGENTS.md`
- [5] Current local state at the end of rollout: repo ahead of origin by 1 commit after `e5a139a Add interview intelligence workflow` was committed locally but not pushed due to environment/network limits

