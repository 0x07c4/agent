thread_id: 019dcac3-9d82-73d3-b372-5f79c66fd4ae
updated_at: 2026-04-26T17:18:31+00:00
rollout_path: $HOME/.codex/sessions/2026/04/27/rollout-2026-04-27T01-08-30-019dcac3-9d82-73d3-b372-5f79c66fd4ae.jsonl
cwd: $HOME/.config/nvim
git_branch: main

# 修复 Neovim 0.12.2 的 Treesitter/Snacks 报错，并确认当前 stable 版本状态

Rollout context: 工作目录是 `$HOME/.config/nvim`，用户先报告在打开文件时触发 `BufReadPost` 里的 Treesitter 报错，随后又询问是否有更新的 Neovim 版本、担心 0.12.2 是否还有其他问题。环境是 Arch Linux，`nvim` 来自系统包 `/usr/bin/nvim`。

## Task 1: 定位并修复 Markdown 打开时的 Treesitter `range` 报错

Outcome: success

Preference signals:

- 用户直接给出具体报错栈，并要求分析定位，说明这类问题他们更希望先拿到明确根因和可落地修复，而不是泛泛解释。
- 用户没有要求先讨论方案再改；后续接受了在本地配置里做最小补丁的处理方式，说明类似场景下可以直接做窄修复并验证。

Key steps:

- 先读取了本地 memory（`PROFILE.md` / `ACTIVE.md`），再检查仓库状态，注意到 `lua/plugins/treesitter.lua` 已有未提交修改，因此把现有改动当作用户改动处理，只在最小范围补丁。
- 通过 `nvim --headless README.md +qa` 复现了错误；错误稳定出现在 Markdown 文件，而不是 Lua 文件。
- 追到栈里 `snacks.scope` / `snacks.indent` 调用 `vim.treesitter.get_range()`，并用临时 wrapper 打印确认传入的是 `table { <userdata> }` 这种单元素列表，而不是直接 TSNode。
- 结论是 Neovim 0.12.2 + Treesitter/Snacks 组合里，`vim.treesitter.get_range()` 对单节点列表不兼容，触发 `attempt to call method 'range' (a nil value)`。
- 最终在 `lua/polish.lua` 顶部加了一个很窄的运行时兼容补丁：只在 `node` 是单元素 TSNode list 时解包后再调用原始 `vim.treesitter.get_range`。
- 还清理了一个验证过程中生成的临时 `nvim.log`，避免把无关产物留在工作区。

Failures and how to do differently:

- 一开始尝试通过 patch Markdown query 来规避问题，但验证显示实际根因不在单个 query 文本，而在 `get_range()` 收到的节点形状；这种情况以后应优先直接包住 runtime API，而不是先大改 query。
- `Lazy sync`、某些 headless 运行会在沙箱里因为 cache/state 目录、ShaDa、`~/.cache/nvim`、`~/.local/state/nvim`、`/run/user/1000/nvim.*` 等路径受限而报环境错误；这些不应误判成配置根因。

Reusable knowledge:

- 在这个环境里，打开 `README.md` 这类 Markdown 文件可以稳定复现问题，而 `lua/polish.lua` 本身不会触发同样错误；说明问题主要在 Markdown Treesitter 触发路径。
- `vim.treesitter.get_range()` 在当前 Neovim 0.12.2 场景下可能收到 `{ <TSNode> }` 单元素列表；对这种输入做解包可绕过 `node:range` nil 错误。
- `:checkhealth vim.treesitter` 结果是 ✅，说明不是全局 Treesitter 完全坏掉，而更像 API/插件边界兼容问题。

References:

- [1] 复现错误的命令与栈：`nvim --headless README.md +qa`，错误核心是 `.../vim/treesitter.lua:196: attempt to call method 'range' (a nil value)`，栈经过 `snacks/scope.lua:404`、`snacks/indent.lua:509`。
- [2] 关键证据：临时 wrapper 打印 `BADNODE table { <userdata 1> } {}`，证明 `get_range()` 收到的是单元素表。
- [3] 代码变更：`lua/polish.lua:7-19` 新增 `patch_treesitter_get_range()`，只在 `type(node) == "table" and #node == 1 and type(node[1]) == "userdata"` 时解包。
- [4] 验证信号：`env XDG_CACHE_HOME=/tmp/... XDG_STATE_HOME=/tmp/... nvim --headless README.md +qa` 在补丁后不再报该栈；`nvim --headless lua/polish.lua +qa` 也正常，仅剩无关的 deprecated 提示。
- [5] 健康检查：`checkhealth vim.treesitter` 输出里 `vim.treesitter: ✅`，各 parser/queries 均为 OK，无 `ERROR`/`WARNING`。

## Task 2: 确认 Neovim 是否有更新版，以及 0.12.2 的风险判断

Outcome: success

Preference signals:

- 用户直接问“neovim是否有新版本更新，我现在担心neovim 0.12.2是否还有其他问题”，说明他们要的是明确版本结论和风险判断，而不是抽象介绍 release 机制。
- 他们关心“当前 stable 是否值得继续用”，所以以后遇到版本问题应直接给出 stable / nightly / 本机包源三层结论。

Key steps:

- 查了官方 GitHub releases，看到 stable/latest 仍是 `v0.12.2`，同时有 `0.13.0-dev` nightly/pre-release。
- 在 Arch 上确认本机安装的是 `neovim 0.12.2-1`，`/usr/bin/nvim` 对应系统包，`pacman -Si neovim` 也是 `0.12.2-1`，`pacman -Qu neovim` 没有可升级项。
- 用 `checkhealth vim.treesitter` 进一步确认本机 Treesitter 状态正常，没有额外红灯。

Reusable knowledge:

- 对 Arch 用户，`pacman -Q neovim` / `pacman -Si neovim` / `pacman -Qu neovim` 可以快速判断本机与仓库是否已有更新。
- 官方 release 页面显示的关键结论是：`v0.12.2` 是当前 stable/latest，`0.13.0-dev` 属于 nightly，不是稳定升级目标。
- 当前问题更像 0.12 生态里个别 Treesitter/API 兼容边界，而不是整个 0.12.2 版本普遍不可用。

Failures and how to do differently:

- 不能把“有 nightly”误当成“应该立刻升级”；这次证据显示 stable 仍停留在 0.12.2，直接切 0.13.0-dev 只会增加不确定性。
- 如果后续再遇到类似问题，应先区分：是上游 stable 已更新、还是本机包源已更新、还是只是插件/查询兼容性问题。

References:

- [1] 本机版本：`nvim --version` -> `NVIM v0.12.2`；`command -v nvim` -> `/usr/bin/nvim`；`pacman -Q neovim` -> `neovim 0.12.2-1`。
- [2] 仓库状态：`pacman -Si neovim` 显示 repo `extra` 版本也是 `0.12.2-1`；`pacman -Qu neovim` 无输出，表示没有可升级项。
- [3] 官方 release 结论：GitHub releases 页面显示 latest/stable 为 `v0.12.2`，nightly 为 `v0.13.0-dev`。
- [4] 健康检查：`checkhealth vim.treesitter` 没有 `ERROR` / `WARNING`，进一步支持“不是整体 Neovim 安装坏了”。
- [5] 额外环境事实：系统是 Arch Linux rolling，`neovim` 包安装日期是 2026-04-25，说明用户用的是比较新的系统包但仍处于 0.12.2 稳定线。
