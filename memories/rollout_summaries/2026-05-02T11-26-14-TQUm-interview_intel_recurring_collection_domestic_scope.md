thread_id: 019de870-68fc-7712-b2fb-c3a2c6142bbe
updated_at: 2026-05-04T09:56:29+00:00
rollout_path: $HOME/.codex/sessions/2026/05/02/rollout-2026-05-02T19-26-14-019de870-68fc-7712-b2fb-c3a2c6142bbe.jsonl
cwd: $HOME/workspace/career-transition
git_branch: main

# 面经收集从“泛 AI infra”逐步收敛到“国内中文真实面经优先”的自动化采集

Rollout context: 在 `$HOME/workspace/career-transition` 里围绕 `interview-intel` 做周期性面经收集。用户先追问“怎么做到定期收集”，随后又问“怎么阅读 CSV”，后来要求进一步收敛范围、优先国内中文面经，并明确说“应该列好任务，让 deepseek 来做执行”。最终今天的收集结果已自动更新到 `interview-intel/inbox/2026-05-04.md`，但主表 `radar.csv` 还未筛选入库。

## Task 1: 把定期收集做成真实可执行流程

Outcome: success

Preference signals:
- 用户纠正“你只是增加了一些文档，怎么做到定期收集面经呢” -> 说明以后遇到“定期/周期/自动收集”类需求，不能只写文档，必须落到可运行的调度入口。
- 用户后面追问“这是一个爬虫工具吗，现在已经收集了吗” -> 说明用户关心的是是否真的采集到了数据，而不是仅有流程说明。

Key steps:
- 新增每周执行入口脚本、用户级 `systemd --user timer`、以及 `inbox`/`review.md` 这类可阅读产物。
- 验证定时器已经安装并会周期触发；`2026-05-04` 当天确实生成了新的 inbox 文件。

Failures and how to do differently:
- 仅有 README/周报模板不算“定期收集”，以后看到“定期”二字先问：调度器在哪里、触发后写到哪里、人工筛选点在哪里。

Reusable knowledge:
- 对这类任务，`systemd --user` 是可落地的本地定时方案；`inbox` 适合作为候选缓冲区，`radar.csv` 适合作为筛选后的主表。
- 人读入口应该是 Markdown，而不是直接读 CSV。

References:
- `scripts/collect-interview-intel-week.sh`
- `scripts/install-interview-intel-timer.sh`
- `scripts/collect-interview-intel-inbox.py`
- `scripts/render-interview-intel-review.py`
- `interview-intel/inbox/2026-05-04.md`

## Task 2: 把阅读方式改成“CSV 底表 + Markdown 阅读面”

Outcome: success

Preference signals:
- 用户问“收集成 csv 的话，怎么阅读呢？” -> 说明用户默认需要一个更适合人看的阅读入口，而不是只给结构化底表。
- 这也反复对应到用户偏好：不要空泛讲方案，要给能直接看的产物。

Key steps:
- 增加 `review.md` 作为从 `radar.csv` 渲染出来的阅读视图。
- 在 README 里明确“不要直接读 CSV”，日常读 `review.md` 和 `inbox/<date>.md`。

Failures and how to do differently:
- 只有 CSV 底表不够，后续类似场景要默认提供一个 Markdown 或清单型阅读面。

Reusable knowledge:
- `radar.csv` 负责去重/状态流转/脚本处理；`review.md` 负责阅读。

References:
- `interview-intel/review.md`
- `scripts/render-interview-intel-review.py`
- `interview-intel/README.md`

## Task 3: 把采集范围收敛到用户真正要看的方向

Outcome: success

Preference signals:
- 用户明确说“所以我觉得你应该收敛面经收集的范围” -> 说明以后默认不要泛收所有 AI infra/LLM/题库内容。
- 用户进一步纠正“今天的优先处理的3条都不像是面经” -> 说明“像面经但不是面经”的招聘页、准备指南、题库都不应占优先区。
- 用户最后要求“优先整理国内的面经” -> 说明默认应把国内中文真实面经放在首位，国外流程/guide 只能补充。
- 用户明确说“我刚才说你应该列好任务，让deepseek来做执行” -> 说明遇到明确拆解好的执行型任务，用户希望交给 DeepSeek 做执行，而不是继续在对话里反复改。

Key steps:
- 查询词从泛 `AI infra/LLM infra/大模型基础设施` 逐步收缩到 `agent runtime / AI coding platform / developer tools infra / agent observability / eval`，最后又进一步偏向国内中文面经。
- 优先区过滤从“有 AI 关键词”改成“`p0/p1 + strong/medium + 真实面经信号 + 国内中文优先`”。
- 明确排除 `interview guide`、`hiring strategy`、`mock interview`、`questions and answers`、题库、准备指南、招聘页。

Failures and how to do differently:
- 一开始把 `interview process`、`interview with` 等流程页算进了优先区，导致出现 Cursor/OpenAI 流程页而非面经；后来通过更严格的真实经验信号修正。
- 之后又出现“优先处理”只剩国外 Cursor/OpenAI 流程页的问题，说明查询词还是过宽；后来继续收缩到国内中文面经优先。
- 最终 inbox 里出现了大量中文 agent/AI coding 面经，这更符合用户当前目标。

Reusable knowledge:
- 对面经收集，“真实面经信号”比“AI 相关”更重要；应把面经复盘、一面/二面、拿 offer/凉经、面试经历等作为优先信号。
- 泛题库、准备指南、mock interview 页面即使命中关键词，也应降级为 noise 或仅保留在全部候选里。
- 国内中文社区来源（牛客、一亩三分地、知乎、掘金、博客园、CSDN、个人博客）更适合作为当前默认优先来源。

References:
- `interview-intel/search-queries.csv`
- `interview-intel/relevance-rules.csv`
- `interview-intel/sources.md`
- `scripts/collect-interview-intel-inbox.py`
- `interview-intel/inbox/2026-05-04.md`

## Task 4: 今天的收集结果已更新，但主表还未入库

Outcome: partial

Preference signals:
- 用户问“deepseek已经执行完了，现在今天的面经已经更新了吗，还是我需要执行一下脚本？” -> 说明用户希望知道当前状态是否已同步到最新收集结果。
- 用户随后问“我应该如何查看你收集的面经” -> 说明最直接的阅读入口应该清晰、稳定、默认可用。

Key steps:
- 今天的收集脚本已经跑过，`interview-intel/inbox/2026-05-04.md` 与 `.csv` 都已更新到最新时间戳。
- 当前主表 `interview-intel/radar.csv` 仍未从 inbox 进行筛选入库，所以最终主表阅读视图还不是主要内容。

Failures and how to do differently:
- 目前只是“收集到 inbox”，还没有“筛选入 radar”。后续如果用户要继续推进，应优先补一个 human-in-the-loop 的 promotion 脚本，而不是把全部候选直接写主表。

Reusable knowledge:
- 当天 inbox 的时间戳会跟随采集脚本更新；用户无需再手动执行脚本来查看最新候选。
- 查看入口是 `less interview-intel/inbox/$(date +%F).md`。

References:
- `interview-intel/inbox/2026-05-04.md`
- `interview-intel/inbox/2026-05-04.csv`
- `interview-intel/radar.csv`
