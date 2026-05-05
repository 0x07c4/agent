## User Profile

用户主要在 Linux 上做 AI developer tools、agent runtime、工作区协作和前端/桌面端相关开发，当前长期主线是 `Solo`、`cocoa`，以及把这些工作包装成求职资产。沟通偏好稳定且明确：默认中文，直接、简洁、少废话，不喜欢泛泛方案，更重视能落地的判断、明确边界和实际改动。用户强偏好 human-in-the-loop，尤其反感 agent 越界接管决策；在产品和工程上都更看重“建议 + 预览 + 用户确认”、清晰状态投影、低复杂度、低误用成本。

工作方式上，用户会把不同 agent/model 的角色边界划得很清楚。如果某次任务被定义成 review、监督、纠偏或 handoff 验收，就不希望 agent自己顺手实现。用户也持续维护本地 memory、skills、wiki 和 notes，希望未来 agent 优先复用现有结构、减少无效扫描，并把稳定经验沉淀为可追溯规则。职业目标上，用户正在从汽车行业软件转向 `AI systems / agent runtime / developer tools` 主线，并希望 `cocoa` / `Solo` 的技术路线能同时服务真实使用价值和求职叙事。

## User preferences

- 默认使用中文，保持直接、简洁、少废话。
- 进度更新只做状态汇报，不要写成征求许可的语气。
- review 任务保持 review 身份；用户明确说过“你不要自己修复，如果有问题对deepseek提出修改意见”。
- 如果工作流已经分工成 “Codex reviews, DeepSeek implements”，不要自己漂移成实现者，除非用户重新指定。
- 不要把“测试全绿”当成任务完成的充分条件；如果用户要的是 contract / prompt / handoff 语义检查，就继续按这个层面审。
- 能复用现有 skill、脚本、工作流时优先复用，不要无谓重造。
- 对 `cocoa`，默认按 terminal-native、human-in-the-loop agent runtime 理解，不要做“另一个 OpenCode”。
- `cocoa` 的改动应继续遵守 `建议 + 预览 + 用户确认`；文件改动保持在 proposal/diff/apply 门后。
- `@path` 在 `cocoa` 里不是隐藏语法；应默认有可见补全和直接可发现的上下文体验。
- `cocoa` 的 cost-aware routing 方向是对的，但配置形态不要堆一堆环境变量；用户明确说过“这种使用环境变量的实现非常丑而且容易出错”。
- 做 `cocoa` / `Solo` 产品定位或求职包装时，默认把它们写进 `$HOME/workspace/career-transition` 的叙事材料，而不是只留在对话里。
- 对求职相关任务，优先把目标收成 `AI systems / agent runtime / developer tools` 主线；不要默认继续围绕汽车行业软件。
- 做面经/资料收集时，用户要的是“定期收集最近的面试经验”这种可执行流程，不是只补几份静态文档。
- 做周期性资料收集时，先按真实目标岗位收敛搜索词和相关性规则；宁可少而准，也不要用泛 `AI infra`、泛 LLM、泛求职词制造噪声。
- 对 `Solo` UI，默认不要自动启动本地预览或 dev server；用户更愿意自己用 `npm run tauri dev`。
- `Solo` UI 默认走 control-plane / observability-first 方向，不走 chat-first、解释驱动界面。
- `Solo` 的资源访问应该“在 agent 需要的时候再向用户询问访问资源的权限”，不要一上来就要求用户预配置目录。
- `Solo` UI 要尽量“把文字提示转换成图像元素”；目标是一眼能看出状态和动作，而不是靠段落解释。
- 做 wiki 相关任务时，先从仓库自己的 `wiki/index.md`、README、playbooks、CLI/tests 进入，不要先大范围乱扫。
- `~/workspace/notes` 与 wiki 有明确边界：notes 是 upstream thinking，wiki 是 compiled knowledge，不要混写。
- 当用户说 wiki 要“接入我的codex”，默认优先走现有 `~/.codex/skills` 和 memory routing 体系，而不是新造全局集成方案。
- 遇到像 `pencil` 这种多义工具名，先确认用户说的是哪个产品，再给工具链建议。
- 当用户已经说“保持现有方案”时，不要继续反复比较备选。
- 遇到本地开发环境报错且根因线索足够时，可以直接做最小修复并验证，不必先停在抽象讨论。

## General Tips

- 进入任务前先读 `PROFILE.md` 和 `ACTIVE.md`；这个 memory repo 的全局规则依赖它们。
- 处理高噪声工程命令时优先用 `rtk`，尤其是 `git diff/status/log`、测试、lint、构建、包管理等；短小精确命令不需要包装。
- 做 memory/文档整理时优先重组结构，不要默认在文件末尾持续追加。
- 对 `cocoa` review/handoff 任务，先读 `docs/ai-handoff.md` 再看实现和测试，效率高于只读 diff。
- 对 `cocoa` 运行验证，常用命令是 `mypy src/cocoa` 和 `PYTHONPATH=src python -m unittest discover -s tests -q`。
- 对 `Solo` UI 改动，`npm run lint` + `npm run build` 是可靠校验对。
- 对 wiki 仓库，`llm-wiki lint`、`llm-wiki reindex --check`、`python3 -m unittest discover -s tests` 是核心验证链；如果 `wiki/index.md` 时间戳漂移，先 rerun `reindex`。
- 对本机 Neovim/Arch 版本判断，先看 `pacman -Q neovim`、`pacman -Si neovim`、`pacman -Qu neovim`，再区分 stable/nightly/plugin 兼容性。
- 在 dirty worktree 里推进任务时，默认把现有未提交改动视为用户改动；最小化补丁并谨慎 stage。

## What's in Memory

### $HOME/workspace/cocoa

#### 2026-05-03

- Review DeepSeek `/escalate last` handoff: cocoa, DeepSeek, /escalate last, docs/ai-handoff.md, prompt semantics, context clipping
  - desc: 汇总了 `$HOME/workspace/cocoa` 中一次 review-only handoff：DeepSeek 实现 first-class `/escalate last`，Codex 负责验收语义与 handoff contract，而不是自己修复。先搜这个主题可快速找到 review 边界、检查顺序、风险点和验证命令。
  - learnings: `rtk python -m unittest tests.test_runtime tests.test_cli` 虽然通过，仍可能漏掉 prompt-boundary 问题；重点核对 pending/completed/rejected 分类，以及 stdout/stderr/diff 是否做了 clipping。

#### 2026-05-02

- cocoa runtime/product shaping: cocoa, terminal-native agent, provider-agnostic, token anxiety, /mode, /usage, environment variables
  - desc: 覆盖 `cocoa` 从 terminal-native MVP、proposal edit、`@path` completion、thread-context replay，到 cost-aware runtime/product positioning 的整组记忆。适合在做 runtime/CLI/UX/求职叙事联动时先搜。
  - learnings: `cocoa` 的正确方向是“cost-aware, provider-agnostic agent runtime”，不是“另一个 OpenCode”；第一版 `COCOA_*` env 路由虽然跑通，但已被用户明确否定，后续要改成结构化 profile/config。

### $HOME/workspace/career-transition

#### 2026-05-02

- career-transition repo and role priority: career-transition, AI systems, agent runtime, developer tools, GOALS.md, transition-narrative
  - desc: 汇总求职仓库的结构、角色优先级、叙事主线，以及 `cocoa` / `Solo` 如何包装成求职资产。适合在更新求职材料、角色筛选或项目叙事时先搜。
  - learnings: 仓库应作为“求职决策系统”维护；当前明确主线是 `AI systems / agent runtime / developer tools`，汽车行业经历只作为可迁移系统能力证据。

- interview intelligence workflow: interview-intel, radar.csv, weekly review, 定期收集最近的面试经验, AI infra, agent runtime interview experience
  - desc: 覆盖面经情报流的目录、模板、脚本和失败点。适合在把静态面经文档升级成真实周期化收集流程时先搜。
  - learnings: 仅有 `interview-intel/` 结构、模板和 `scripts/new-interview-intel-week.sh` 还不够；真正高价值的是按目标岗位收敛检索词，并把结果定期写回 `radar.csv` 与周报。

### $HOME/.config/nvim

#### 2026-04-26

- Treesitter `node:range` crash and stable-version triage: neovim, treesitter, snacks.nvim, get_range, pacman -Q neovim, v0.12.2
  - desc: 覆盖本机 `$HOME/.config/nvim` 下 Markdown 打开时报 `vim.treesitter.get_range()`/`node:range` 的定位与修复，以及 Arch 上 Neovim stable/nightly/包源状态的判断方法。
  - learnings: 问题根因是 `get_range()` 收到单元素 TSNode 列表，不是 query 文本本身；修复路径是 `lua/polish.lua` 的窄兼容 wrapper，而不是大改 Treesitter query。版本判断要分 stable、nightly 和本机包源三层。

### Older Memory Topics

#### $HOME/workspace/wiki

- wiki local knowledge system and Codex integration: llm-wiki, wiki/index.md, ~/workspace/notes, agent frontier radar, ~/.codex/skills/llm-wiki, ACT-021
  - desc: 覆盖 wiki 仓库的 repo orientation、notes-to-wiki 边界、agent frontier source pack、Obsidian Agent Workbench repo-health 面板，以及通过 `~/.codex/skills/llm-wiki` 接入 Codex 的路径。适用 `cwd=$HOME/workspace/wiki` 和相关本地长期知识任务。

#### $HOME/workspace/solo

- Solo control-plane UI and on-demand resource authorization: Solo, control plane, observability-first, checkpoint, visual UI, EmptyVisual, npm run lint, npm run build
  - desc: 覆盖 Solo UI 从解释型界面收成 control-plane/observability-first 的过程，包括 ask-at-need 资源授权、减少文字提示、用视觉状态替代说明文案。适用 `cwd=$HOME/workspace/solo` 的产品/UI 改动。

#### $HOME and $HOME/workspace/solo

- OpenPencil toolchain clarification: openpencil, pencil.dev, open-pencil, op CLI, .op, @zseven-w/openpencil
  - desc: 记录 `pencil` 命名歧义的澄清过程，以及当前稳定方案是 `OpenPencil skill + @zseven-w/openpencil + op CLI + .op artifacts`。适合在再次讨论 Solo 设计工具链时先搜。
