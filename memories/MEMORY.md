# Task Group: career-transition positioning and interview intelligence

scope: `$HOME/workspace/career-transition` as a structured job-search system, including role prioritization, transition narrative, and the now-runnable-but-still-human-filtered interview-intel collection loop.
applies_to: cwd=$HOME/workspace/career-transition; reuse_rule=safe for this repo and closely related career-packaging tasks; treat file lists, timers, and commit refs as checkout-specific

## Task 1: Build the repo as a job-search decision system and prioritize AI systems roles

### rollout_summary_files

- rollout_summaries/2026-05-02T11-46-19-DJx1-cocoa_cost_aware_runtime_and_job_search_positioning.md (cwd=$HOME/workspace/cocoa, rollout_path=$HOME/.codex/sessions/2026/05/02/rollout-2026-05-02T19-46-19-019de882-cdd2-7971-9bda-4e97a29ecafb.jsonl, updated_at=2026-05-02T12:22:06+00:00, thread_id=019de882-cdd2-7971-9bda-4e97a29ecafb, cross-links cocoa/Solo narrative into job search)
- rollout_summaries/2026-05-02T05-49-25-TYv5-career_transition_ai_systems_and_interview_intel.md (cwd=$HOME/workspace/career-transition, rollout_path=$HOME/.codex/sessions/2026/05/02/rollout-2026-05-02T13-49-25-019de73c-0dca-7383-92b1-2625742bffef.jsonl, updated_at=2026-05-02T09:51:21+00:00, thread_id=019de73c-0dca-7383-92b1-2625742bffef, repo setup + positioning)

### keywords

- career-transition, AI systems, agent runtime, developer tools, Bosch, automotive exit, GOALS.md, targets/role-archetypes.md, positioning/transition-narrative.md, cocoa-solo-agent-runtime-project.md

## Task 2: Make interview-intel recurring collection executable and readable, not just documented

### rollout_summary_files

- rollout_summaries/2026-05-02T11-26-14-TQUm-interview_intel_recurring_collection_domestic_scope.md (cwd=$HOME/workspace/career-transition, rollout_path=$HOME/.codex/sessions/2026/05/02/rollout-2026-05-02T19-26-14-019de870-68fc-7712-b2fb-c3a2c6142bbe.jsonl, updated_at=2026-05-04T09:56:29+00:00, thread_id=019de870-68fc-7712-b2fb-c3a2c6142bbe, timer + inbox + review flow landed)
- rollout_summaries/2026-05-02T05-49-25-TYv5-career_transition_ai_systems_and_interview_intel.md (cwd=$HOME/workspace/career-transition, rollout_path=$HOME/.codex/sessions/2026/05/02/rollout-2026-05-02T13-49-25-019de73c-0dca-7383-92b1-2625742bffef.jsonl, updated_at=2026-05-02T09:51:21+00:00, thread_id=019de73c-0dca-7383-92b1-2625742bffef, original structure before execution path existed)

### keywords

- interview-intel, systemd --user timer, scripts/collect-interview-intel-week.sh, scripts/install-interview-intel-timer.sh, radar.csv, review.md, inbox, 定期收集最近的面试经验, CSV 底表 + Markdown 阅读面

## Task 3: Narrow interview-intel to domestic Chinese real interview reports first

### rollout_summary_files

- rollout_summaries/2026-05-02T11-26-14-TQUm-interview_intel_recurring_collection_domestic_scope.md (cwd=$HOME/workspace/career-transition, rollout_path=$HOME/.codex/sessions/2026/05/02/rollout-2026-05-02T19-26-14-019de870-68fc-7712-b2fb-c3a2c6142bbe.jsonl, updated_at=2026-05-04T09:56:29+00:00, thread_id=019de870-68fc-7712-b2fb-c3a2c6142bbe, scope tightened after noisy results)

### keywords

- 国内中文真实面经, 牛客, 一亩三分地, 知乎, 掘金, 博客园, CSDN, search-queries.csv, relevance-rules.csv, agent runtime, AI coding platform, developer tools infra, interview guide, mock interview, questions and answers

## Task 4: Today’s inbox is updated, but promotion into `radar.csv` is still pending

### rollout_summary_files

- rollout_summaries/2026-05-02T11-26-14-TQUm-interview_intel_recurring_collection_domestic_scope.md (cwd=$HOME/workspace/career-transition, rollout_path=$HOME/.codex/sessions/2026/05/02/rollout-2026-05-02T19-26-14-019de870-68fc-7712-b2fb-c3a2c6142bbe.jsonl, updated_at=2026-05-04T09:56:29+00:00, thread_id=019de870-68fc-7712-b2fb-c3a2c6142bbe, collection fresh, promotion incomplete)

### keywords

- interview-intel/inbox/2026-05-04.md, interview-intel/inbox/2026-05-04.csv, interview-intel/radar.csv, less interview-intel/inbox/$(date +%F).md, human-in-the-loop, promotion script

## User preferences

- when the user said "我最近打算换工作，我想先创建一个仓库来记录我的学习\\面试\\找工作的过程" -> default to a versioned repo/workflow, not a chat-only plan [Task 1]
- when the user asked "这个仓库应该如何使用呢" -> provide an executable repo workflow, not just broad advice [Task 1]
- when the user said "我现在也没有清晰的目标，你能帮我设计吗" -> produce an iterative first pass and then help narrow it, instead of waiting for perfect inputs [Task 1]
- when the user clarified "并且我现在确实想离开汽车行业了" and then "把ai相关岗位的目标提升优先级" -> do not keep automotive software as the target lane; treat `AI systems / agent runtime / developer tools` as the job-search mainline [Task 1]
- when the user said the repo should "提供收集面经的能力，定期收集最近的面试经验" and then challenged "你只是增加了一些文档，怎么做到定期收集面经呢" -> static structure is insufficient; future work needs an actual recurring execution path [Task 2]
- when the user asked "这是一个爬虫工具吗，现在已经收集了吗" -> report whether data was actually collected, not just whether docs/scripts exist [Task 2]
- when the user asked "收集成 csv 的话，怎么阅读呢？" -> default to a Markdown reading surface, not CSV-only output [Task 2]
- when the user said "所以我觉得你应该收敛面经收集的范围" and then "今天的优先处理的3条都不像是面经" -> do not let interview guides, hiring pages, or 题库占据优先区; optimize for real interview reports first [Task 3]
- when the user said "优先整理国内的面经" -> prioritize domestic Chinese real interview reports over overseas process pages or generic guides [Task 3]
- when the user explicitly said "我刚才说你应该列好任务，让deepseek来做执行" -> for clearly decomposed execution tasks, hand off execution work to DeepSeek instead of repeatedly iterating in chat [Task 3]
- when the user asked "deepseek已经执行完了，现在今天的面经已经更新了吗，还是我需要执行一下脚本？" and "我应该如何查看你收集的面经" -> answer with current sync status and a stable reading entrypoint immediately [Task 4]

## Reusable knowledge

- The repo is best framed as a job-search decision system, not a diary [Task 1]
- Current role priority is `AI systems / agent runtime / developer tools` first, `performance / observability` as the supporting systems angle, and `edge platform` / `backend infrastructure` as backup entry points [Task 1]
- Bosch / parking / SDV experience should be used as evidence of transferable systems ability, not as the next-industry target [Task 1]
- The durable files for this repo are `GOALS.md`, `targets/role-archetypes.md`, `positioning/transition-narrative.md`, and the cocoa/Solo packaging docs that tie product work back to job-search narrative [Task 1]
- Interview-intel now has a runnable local collection path: `systemd --user timer` triggers the weekly collection scripts, `inbox` is the candidate buffer, `radar.csv` is the filtered main table, and `review.md` is the human reading surface [Task 2]
- `radar.csv` handles dedupe/state/script workflows, while `review.md` and `inbox/<date>.md` are the intended reading surfaces; "不要直接读 CSV" is the durable default for this flow [Task 2]
- Interview-intel should prioritize recent 7-14 day signals with company/role/round/question-type detail and a concrete preparation action; avoid copying full posts or retaining private/internal information [Task 2]
- For interview collection, “真实面经信号” is more important than generic “AI 相关” matching; prioritize 面经复盘、一面/二面、拿 offer/凉经、面试经历 over broad AI keyword hits [Task 3]
- Domestic Chinese sources such as 牛客、一亩三分地、知乎、掘金、博客园、CSDN、个人博客 are the default high-priority channels for the current target [Task 3]
- `interview guide`, `hiring strategy`, `mock interview`, `questions and answers`, 题库, 准备指南, and 招聘页 should be downgraded to noise or at most left in the full candidate set [Task 3]
- The current “latest readable result” pattern is: today’s collection updates `interview-intel/inbox/<date>.md` and `.csv`; the user can read it directly with `less interview-intel/inbox/$(date +%F).md` without rerunning the script [Task 4]
- The remaining missing piece is a human-in-the-loop promotion script from inbox into `radar.csv`; until that exists, inbox freshness does not mean the main table is curated [Task 4]

## Failures and how to do differently

- Symptom: role strategy feels wide and unfocused. Cause: starting with too many adjacent tracks at once. Fix: confirm industry exit and role-priority order early, then collapse the repo around that mainline [Task 1]
- Symptom: the user rejects some apparently helpful career docs. Cause: assuming generic resignation/offboarding materials are needed. Fix: in mature-company exits like Bosch, focus on transition boundaries and job-search execution, not boilerplate departure templates [Task 1]
- Symptom: interview-intel work looks complete on paper but does not satisfy the request. Cause: only creating folders/templates/scripts without a real run loop. Fix: add an actual recurring collection procedure or scheduler that writes into inbox/review surfaces and only then talk about workflow completeness [Task 2]
- Symptom: the collected data exists but still feels unusable. Cause: only exposing CSV bottom tables. Fix: always pair the structured table with a Markdown reading surface such as `review.md` or `inbox/<date>.md` [Task 2]
- Symptom: search results become noisy and low-value. Cause: using broad `AI infra` or generic LLM/job-search terms, or over-weighting “AI” over “真实面经”. Fix: scope queries to `agent infrastructure / agent runtime / AI coding platform / developer tools infra / observability / eval`, then apply real-interview and domestic-Chinese filters before priority ranking [Task 3]
- Symptom: Cursor/OpenAI process pages or foreign workflow articles dominate the priority bucket. Cause: `interview process` / `interview with` style terms stayed too broad. Fix: tighten relevance rules toward `p0/p1 + strong/medium + 真实面经信号 + 国内中文优先` and explicitly exclude guides, mocks, and question banks [Task 3]
- Symptom: users cannot tell whether “today’s interview intel” is already current. Cause: not stating sync status and reading entrypoint explicitly. Fix: answer with whether today’s inbox was refreshed and whether `radar.csv` promotion has happened yet [Task 4]
- Symptom: all candidates remain stuck outside the main table. Cause: collection is automated but promotion is still manual. Fix: add a human-in-the-loop promotion script instead of bulk-writing inbox candidates into `radar.csv` [Task 4]

# Task Group: cocoa review handoff workflows

scope: Review-only handoffs in `$HOME/workspace/cocoa`, especially when another model implements and Codex is asked to validate contract fidelity rather than self-fix.
applies_to: cwd=$HOME/workspace/cocoa; reuse_rule=safe for similar cocoa review/handoff tasks, especially `/escalate last` or prompt-contract validation; treat code paths and line references as checkout-specific

## Task 1: Review DeepSeek's `/escalate last` implementation, tests green but prompt semantics need fixes

### rollout_summary_files

- rollout_summaries/2026-05-03T06-38-29-9qjL-review_deepseek_escalation_implementation.md (cwd=$HOME/workspace/cocoa, rollout_path=$HOME/.codex/sessions/2026/05/03/rollout-2026-05-03T14-38-29-019dec8f-555f-7990-9ef2-ad2617e9ef99.jsonl, updated_at=2026-05-03T16:19:21+00:00, thread_id=019dec8f-555f-7990-9ef2-ad2617e9ef99, review concluded `needs-fix`)

### keywords

- cocoa, DeepSeek, /escalate last, escalation, prompt semantics, context clipping, pending proposals, completed results, docs/ai-handoff.md, src/cocoa/runtime.py, rtk python -m unittest tests.test_runtime tests.test_cli

## Task 2: Redirect review findings back to the implementer instead of patching directly

### rollout_summary_files

- rollout_summaries/2026-05-03T06-38-29-9qjL-review_deepseek_escalation_implementation.md (cwd=$HOME/workspace/cocoa, rollout_path=$HOME/.codex/sessions/2026/05/03/rollout-2026-05-03T14-38-29-019dec8f-555f-7990-9ef2-ad2617e9ef99.jsonl, updated_at=2026-05-03T16:19:21+00:00, thread_id=019dec8f-555f-7990-9ef2-ad2617e9ef99, preserve review/implementation boundary)

### keywords

- review-only, handoff, DeepSeek implements, Codex reviews, needs-fix, user correction, 不要自己修复, 修改意见, role boundary, green tests not enough

## User preferences

- when reviewing another model's implementation, the user corrected: "你不要自己修复，如果有问题对deepseek提出修改意见" -> stay in review mode, write actionable change requests, and do not silently patch code [Task 1][Task 2]
- when the workflow is split across agents/models, the rollout reinforced "Codex reviews, DeepSeek implements" -> preserve that boundary and avoid role drift unless the user explicitly changes it [Task 1]

## Reusable knowledge

- For cocoa `/escalate last` review, read `docs/ai-handoff.md` first and validate the contract directly: active-provider behavior, no fresh workspace scan, no `cocoa-proposal` parsing into pending proposals, routing/usage metadata, and review-only output expectations [Task 1]
- `rtk python -m unittest tests.test_runtime tests.test_cli` passed with 79 tests, so this rollout established that unit-green status does not rule out handoff/prompt-contract defects [Task 1][Task 2]
- The reviewed implementation already wired `/escalate last` through CLI/runtime and usage recording; the higher-value review target was prompt construction in `src/cocoa/runtime.py`, not command plumbing [Task 1]
- In this rollout the key issue class was escalation-prompt fidelity: pending vs completed/rejected item selection, plus clipping large stdout/stderr/diff payloads to stay aligned with the cost-aware/context-reuse goal [Task 1][Task 2]
- `docs/cost-aware-runtime.md` already marked Phase 3 `/escalate last` as done with explicit-mode behavior, so review had to focus on semantic correctness rather than whether the feature existed at all [Task 1]

## Failures and how to do differently

- Symptom: tests pass but the handoff still feels wrong. Cause: the escalation prompt classified items by tool type (`file_write` vs `command`) instead of actual pending/completed/rejected state. Fix: inspect prompt assembly logic and verify state semantics against `docs/ai-handoff.md` before accepting the implementation [Task 1][Task 2]
- Symptom: escalation context becomes too large or fights the cost-aware design. Cause: full stdout/stderr/diff payloads were included without clipping. Fix: review context-size controls explicitly and require clipping/truncation for large command output and diffs [Task 1]
- Symptom: review work turns into implementation work. Cause: collapsing the review and fix steps after a green test run. Fix: if the user asked for feedback-only, return `needs-fix` plus concrete change requests and stop there [Task 2]
- Do not treat a successful unit suite as sufficient when the review target is prompt semantics or handoff contract fidelity; add a contract-level checklist to the review path [Task 1][Task 2]

# Task Group: cocoa runtime product shaping and terminal-native workflow

scope: Building `$HOME/workspace/cocoa` as a terminal-native, human-in-the-loop agent runtime, including proposal-edit UX, `@path` context, thread replay, and cost-aware/provider-agnostic product direction.
applies_to: cwd=$HOME/workspace/cocoa; reuse_rule=safe for similar cocoa runtime/product work and nearby CLI/runtime flows; treat code references and exact commands as checkout-specific

## Task 1: Establish cocoa as a terminal-native agent runtime MVP, not a generic CLI

### rollout_summary_files

- rollout_summaries/2026-05-02T11-46-19-DJx1-cocoa_cost_aware_runtime_and_job_search_positioning.md (cwd=$HOME/workspace/cocoa, rollout_path=$HOME/.codex/sessions/2026/05/02/rollout-2026-05-02T19-46-19-019de882-cdd2-7971-9bda-4e97a29ecafb.jsonl, updated_at=2026-05-02T12:22:06+00:00, thread_id=019de882-cdd2-7971-9bda-4e97a29ecafb, positioning updated)
- rollout_summaries/2026-04-25T05-03-00-c8pt-cocoa_terminal_native_agent_loop_edit_ux_and_context.md (cwd=$HOME/workspace/cocoa, rollout_path=$HOME/.codex/sessions/2026/04/25/rollout-2026-04-25T13-03-00-019dc305-09f1-7272-8180-4181909d0c2b.jsonl, updated_at=2026-04-26T17:31:01+00:00, thread_id=019dc305-09f1-7272-8180-4181909d0c2b, MVP/runtime references)

### keywords

- cocoa, terminal-native agent, codex, claude-code-run, human-in-the-loop, approval gate, proposal workflow, runtime, provider-agnostic, another OpenCode, docs/architecture.md

## Task 2: Make file edits proposal-based, exact-match, and recoverable

### rollout_summary_files

- rollout_summaries/2026-04-25T05-03-00-c8pt-cocoa_terminal_native_agent_loop_edit_ux_and_context.md (cwd=$HOME/workspace/cocoa, rollout_path=$HOME/.codex/sessions/2026/04/25/rollout-2026-04-25T13-03-00-019dc305-09f1-7272-8180-4181909d0c2b.jsonl, updated_at=2026-04-26T17:31:01+00:00, thread_id=019dc305-09f1-7272-8180-4181909d0c2b, exact edit proposals + `@path` completion + thread replay)

### keywords

- operation=replace, exact file edit proposal, old text not found for file edit proposal, @path, prompt_toolkit, /pending, /diff, /apply, replace_all, unified diff, src/cocoa/runtime.py, src/cocoa/proposals.py

## Task 3: Add cost-aware routing/usage tracking, but avoid env-var-heavy configuration

### rollout_summary_files

- rollout_summaries/2026-05-02T11-46-19-DJx1-cocoa_cost_aware_runtime_and_job_search_positioning.md (cwd=$HOME/workspace/cocoa, rollout_path=$HOME/.codex/sessions/2026/05/02/rollout-2026-05-02T19-46-19-019de882-cdd2-7971-9bda-4e97a29ecafb.jsonl, updated_at=2026-05-02T12:22:06+00:00, thread_id=019de882-cdd2-7971-9bda-4e97a29ecafb, partial because config shape was rejected)

### keywords

- cost-aware runtime, token anxiety, routing_decision, usage_recorded, /mode, /usage, COCOA_MODEL_MODE, COCOA_CHEAP_*, COCOA_PREMIUM_*, ProviderResponse, docs/cost-aware-runtime.md

## Task 4: Feed tool results into thread context so interrupted work resumes cleanly

### rollout_summary_files

- rollout_summaries/2026-04-25T05-03-00-c8pt-cocoa_terminal_native_agent_loop_edit_ux_and_context.md (cwd=$HOME/workspace/cocoa, rollout_path=$HOME/.codex/sessions/2026/04/25/rollout-2026-04-25T13-03-00-019dc305-09f1-7272-8180-4181909d0c2b.jsonl, updated_at=2026-04-26T17:31:01+00:00, thread_id=019dc305-09f1-7272-8180-4181909d0c2b, context replay work)

### keywords

- thread context, tool results, command outputs, file write results, pending proposal state, clipped outputs, _build_thread_context, _context_lines_for_item, resumable workflow

## User preferences

- when the user said "我想要构建我自己的Terminal Native Agentic Coding System，名字就是这个文件夹的名字：cocoa，你可以参考~/workspace下的codex项目和claude-code-run项目" -> treat `cocoa` as a terminal-native agent runtime and mine `codex` + `claude-code-run` for runtime/UX patterns instead of inventing a generic CLI [Task 1]
- the user's standing default is `建议 + 预览 + 用户确认` -> file changes must remain behind proposal/diff/apply gates, not direct application [Task 1][Task 2]
- when the user complained `而 @也没有补全提示` -> `@path` should be a first-class composer affordance with visible completion, not a hidden parse-time trick [Task 2]
- when the user asked whether `cocoa` still matters given `opencode`, then reframed it around token anxiety and mixed-model routing, the preference was clear: do not build "另一个 OpenCode"; build a cost-aware, provider-agnostic runtime [Task 1][Task 3]
- when the user later said "不，现在这种使用环境变量的实现非常丑而且容易出错" -> future routing/config design should default to explicit, structured profiles rather than a pile of `COCOA_*` env vars [Task 3]

## Reusable knowledge

- `cocoa` started as a Python-first MVP with append-only JSONL state, provider adapters, workspace context injection, an approval gate, and proposal-based writes; those are the stable primitives to reuse before expanding the product surface [Task 1]
- `codex` is the better reference for thread/turn/item modeling, approval boundaries, and replayable context; `claude-code-run` is more useful for terminal composer UX and background orchestration [Task 1]
- On this machine, `mypy src/cocoa` is the correct type-check command; `python -m mypy` was unreliable because `mypy` is exposed via pipx on PATH [Task 1][Task 2]
- Existing-file edits are represented as pending file-write items with `operation=replace` and exact string replacement semantics; `old` must match exactly once unless `replace_all=true` [Task 2]
- `@path` completion now exists in both the `prompt_toolkit` composer and the raw-terminal fallback; completion/suggestion work has to be tested at both the candidate layer and the outer suggestion guard [Task 2]
- The thread-context builder now projects command outputs, file read/write summaries, and pending proposal state, not just chat text; long outputs should be clipped before they enter model context [Task 4]
- Adding routing/usage as new event kinds works with the current projection model because unknown events can stay out of the main UI projection; `/usage` reads from the event log summary path [Task 3]
- The correct unittest invocation for this repo is `PYTHONPATH=src python -m unittest discover -s tests -q` [Task 2][Task 3]

## Failures and how to do differently

- Symptom: edit proposal technically failed but the user only sees `old text not found for file edit proposal: README.md`. Cause: surfacing a raw runtime error instead of a workflow recovery path. Fix: make `/pending`, `/diff`, and `/apply` explain why the proposal is unusable and direct the user toward fresh `@path` context or `/reject` [Task 2]
- Symptom: `@path` completion still does not appear after low-level candidate support was added. Cause: the outer suggestion layer still rejected non-`/` lines. Fix: test both `_completion_candidates` and `_suggestion_items`, not just one layer [Task 2]
- Symptom: type-check command appears broken. Cause: using `python -m mypy` in an environment where only the pipx-installed `mypy` binary is available. Fix: call `mypy src/cocoa` directly [Task 1][Task 2]
- Symptom: cost-aware runtime works but the user rejects the implementation shape. Cause: routing/configuration was expressed as many `COCOA_*` environment variables. Fix: move toward structured config objects, explicit profile management, and a clearer persistence/UI path [Task 3]
- Symptom: commit accidentally picks up unrelated local changes. Cause: not checking/staging carefully in a dirty worktree. Fix: inspect `git status`, keep unrelated files like `README.md` or `tmp/` out of the task commit, and stage only the current feature set [Task 2][Task 4]

# Task Group: neovim treesitter debugging and version triage

scope: Debugging the user's local Neovim config in `$HOME/.config/nvim`, especially Treesitter/plugin boundary failures and package-version reality checks on Arch Linux.
applies_to: cwd=$HOME/.config/nvim; reuse_rule=safe for similar local Neovim debugging on this machine; treat plugin stacks and file paths as checkout-specific

## Task 1: Fix the Markdown `node:range` crash with a narrow runtime compatibility patch

### rollout_summary_files

- rollout_summaries/2026-04-26T17-08-30-j3BQ-neovim_treesitter_range_bug_and_version_check.md (cwd=$HOME/.config/nvim, rollout_path=$HOME/.codex/sessions/2026/04/27/rollout-2026-04-27T01-08-30-019dcac3-9d82-73d3-b372-5f79c66fd4ae.jsonl, updated_at=2026-04-26T17:18:31+00:00, thread_id=019dcac3-9d82-73d3-b372-5f79c66fd4ae, runtime patch validated)

### keywords

- neovim, treesitter, snacks.nvim, get_range, node:range, README.md, lua/polish.lua, BADNODE, BufReadPost, markdown, headless

## Task 2: Check whether Neovim stable actually changed before recommending upgrades

### rollout_summary_files

- rollout_summaries/2026-04-26T17-08-30-j3BQ-neovim_treesitter_range_bug_and_version_check.md (cwd=$HOME/.config/nvim, rollout_path=$HOME/.codex/sessions/2026/04/27/rollout-2026-04-27T01-08-30-019dcac3-9d82-73d3-b372-5f79c66fd4ae.jsonl, updated_at=2026-04-26T17:18:31+00:00, thread_id=019dcac3-9d82-73d3-b372-5f79c66fd4ae, stable/nightly/package-source triage)

### keywords

- nvim --version, pacman -Q neovim, pacman -Si neovim, pacman -Qu neovim, v0.12.2, 0.13.0-dev, Arch Linux, stable, nightly, checkhealth vim.treesitter

## User preferences

- when the user brings a concrete stack trace and asks for diagnosis, they want a root-cause answer and a narrow fix, not a generic explanation [Task 1]
- when the user asks "neovim是否有新版本更新，我现在担心neovim 0.12.2是否还有其他问题" -> answer with stable/nightly/local-package conclusions directly, not release-process theory [Task 2]

## Reusable knowledge

- In this environment, `nvim --headless README.md +qa` reproduces the Treesitter/Snacks crash while `lua/polish.lua` does not, so Markdown is the key trigger path [Task 1]
- The actionable root cause was `vim.treesitter.get_range()` receiving a single-element TSNode list (`{ <TSNode> }`) and then calling `node:range` on the wrapper table; unwrapping that single node before delegating avoids the crash [Task 1]
- `checkhealth vim.treesitter` was clean, so this was a boundary-compatibility bug rather than a globally broken Treesitter install [Task 1][Task 2]
- For Arch version triage, `pacman -Q neovim`, `pacman -Si neovim`, and `pacman -Qu neovim` are the quickest local/package-source checks before recommending upgrades [Task 2]
- At the time of this rollout, official stable/latest was still `v0.12.2`; `0.13.0-dev` was nightly/pre-release, not a stable upgrade target [Task 2]

## Failures and how to do differently

- Symptom: crash happens on Markdown open and the first instinct is to patch queries. Cause: the real issue sits at the runtime API boundary, not in one query file. Fix: instrument the API call shape first and prefer a narrow compatibility wrapper over broad query edits [Task 1]
- Symptom: headless/Lazy commands throw cache/state/ShaDa permission errors. Cause: sandboxed temp/state paths, not necessarily the Neovim config. Fix: do not mistake environment permission noise for the config root cause [Task 1]
- Symptom: presence of a nightly build is mistaken for an upgrade recommendation. Cause: conflating upstream nightly with current stable/package reality. Fix: separate stable release status, distro package status, and plugin compatibility into three explicit conclusions [Task 2]

# Task Group: wiki local knowledge system and Codex integration

scope: `$HOME/workspace/wiki` as the user's local AI-agent knowledge base, including notes ingestion, frontier radar, Obsidian workbench, and the `llm-wiki` Codex skill/routing workflow.
applies_to: cwd=$HOME/workspace/wiki; reuse_rule=safe for this wiki repo and adjacent local-knowledge tasks; treat file paths and plugin code as checkout-specific

## Task 1: Orient on the wiki repo from its own index and runbooks

### rollout_summary_files

- rollout_summaries/2026-04-25T08-58-07-LC3e-wiki_codex_integration_and_agent_frontier_radar.md (cwd=$HOME/workspace/wiki, rollout_path=$HOME/.codex/sessions/2026/04/25/rollout-2026-04-25T16-58-07-019dc3dc-4a6d-71b0-8083-10e658c00d28.jsonl, updated_at=2026-04-25T16:57:11+00:00, thread_id=019dc3dc-4a6d-71b0-8083-10e658c00d28, repo orientation)

### keywords

- llm-wiki, wiki/index.md, wiki/log.md, reindex, lint, search, query, status, graph, playbooks/ingest-checklist.md, playbooks/lint-checklist.md

## Task 2: Preserve the `~/workspace/notes` upstream boundary and ingest stable notes via snapshots

### rollout_summary_files

- rollout_summaries/2026-04-25T08-58-07-LC3e-wiki_codex_integration_and_agent_frontier_radar.md (cwd=$HOME/workspace/wiki, rollout_path=$HOME/.codex/sessions/2026/04/25/rollout-2026-04-25T16-58-07-019dc3dc-4a6d-71b0-8083-10e658c00d28.jsonl, updated_at=2026-04-25T16:57:11+00:00, thread_id=019dc3dc-4a6d-71b0-8083-10e658c00d28, notes ingestion boundary)

### keywords

- ~/workspace/notes, raw/sources/notes, notes-to-wiki-checklist, agent-interaction-formalism.md, compiled knowledge, source snapshot, wiki/concepts/agent-runtime.md

## Task 3: Build frontier radar, Obsidian workbench repo-health view, and Codex skill routing

### rollout_summary_files

- rollout_summaries/2026-04-25T08-58-07-LC3e-wiki_codex_integration_and_agent_frontier_radar.md (cwd=$HOME/workspace/wiki, rollout_path=$HOME/.codex/sessions/2026/04/25/rollout-2026-04-25T16-58-07-019dc3dc-4a6d-71b0-8083-10e658c00d28.jsonl, updated_at=2026-04-25T16:57:11+00:00, thread_id=019dc3dc-4a6d-71b0-8083-10e658c00d28, frontier + workbench + Codex integration milestone)

### keywords

- agent frontier radar, Agent Skills, Agent Evaluation, status --json, Obsidian Agent Workbench, llm-wiki skill, ~/.codex/skills/llm-wiki, ACT-021, sync-agent-config.sh --dry-run, c1a21ba

## User preferences

- when the user said "你先理解一下这个仓库" -> start orientation from the repo's own `wiki/index.md`, README, playbooks, and CLI/tests before broad scanning [Task 1]
- when the user asked how `~/workspace/notes` should relate to the wiki and then said "我觉得你的方案可行" -> treat notes as upstream thinking and the wiki as compiled knowledge, not two peer stores edited interchangeably [Task 2]
- when the user asked to collect "最近ai agent邻域的一些进展信息" -> default to a source-pack + synthesis radar for fast-moving topics instead of scattering many tiny pages [Task 3]
- when the user wanted the wiki "接入我的codex" -> assume local long-term-context work should be discoverable from Codex through the existing global skill/routing chain [Task 3]

## Reusable knowledge

- `llm-wiki` is a stdlib-only Python CLI with `reindex`, `search`, `query`, `lint`, `status`, `graph`, and `ingest-init`; `wiki/index.md` and `wiki/log.md` are the best first-entry documents [Task 1]
- `wiki/index.md` is generated and its date can drift whenever `reindex` runs; if `reindex --check` fails after repo changes, rerun `reindex` before treating it as a content problem [Task 1]
- The maintained boundary is: `~/workspace/notes` stays upstream, stable notes get snapshotted into `raw/sources/notes/`, and maintained wiki pages are derived from those sources with traceability preserved [Task 2]
- Fast-moving frontier work is best clustered by theme: models, runtime, skills, standards, evaluation; do not promote every scouting source into stable concept pages immediately [Task 3]
- The Obsidian workbench should stay read-only, dense, and summary-oriented; render repo health in-panel and keep JSON copy buttons as secondary handoff helpers [Task 3]
- The minimal effective Codex integration is a skill under `~/.codex/skills/llm-wiki` plus a concise routing rule like `[ACT-021]`, not a background daemon [Task 3]

## Failures and how to do differently

- Symptom: wiki checks fail after normal content changes. Cause: generated `wiki/index.md` date drift. Fix: rerun `reindex` before `--check` and do not misread the timestamp refresh as content breakage [Task 1]
- Symptom: wiki pages lose provenance or become a second notes repo. Cause: copying `~/workspace/notes` directly into maintained pages. Fix: snapshot into `raw/sources/notes/` first and ingest one stable source at a time [Task 2]
- Symptom: frontier research becomes noisy or prematurely canonical. Cause: treating scouting notes as stable truth. Fix: keep them in curated source packs first, then promote selectively by theme [Task 3]
- Symptom: Codex integration work sprawls into global config churn. Cause: inventing new plumbing or editing broad global docs. Fix: prefer a discoverable skill, a small `ACTIVE.md` rule, and wrapper scripts; use `python3 .../init_skill.py` when the scaffold script is not directly executable [Task 3]

# Task Group: solo control-plane UI and on-demand resource authorization

scope: UI and docs work in `$HOME/workspace/solo` that pushes Solo toward an observability/control-plane surface with visual state, checkpoint-driven permissions, and less explanation-heavy copy.
applies_to: cwd=$HOME/workspace/solo; reuse_rule=safe for similar Solo product/UI direction work; treat component paths and validation commands as checkout-specific

## Task 1: Reframe Solo around task/run state and validate with lint/build

### rollout_summary_files

- rollout_summaries/2026-04-24T15-57-58-Nolw-solo_ui_on_demand_resources_visual_state_refactor.md (cwd=$HOME/workspace/solo, rollout_path=$HOME/.codex/sessions/2026/04/24/rollout-2026-04-24T23-57-58-019dc036-50b2-7700-b2e9-f1fdbef0ec8e.jsonl, updated_at=2026-04-24T16:30:31+00:00, thread_id=019dc036-50b2-7700-b2e9-f1fdbef0ec8e, main UI refactor)

### keywords

- Solo, control plane, observability-first, task/run state, runtime summary, src/App.jsx, src/App.css, SettingsModal, WorkspaceModal, npm run lint, npm run build

## Task 2: Move resource access to on-demand checkpoint requests

### rollout_summary_files

- rollout_summaries/2026-04-24T15-57-58-Nolw-solo_ui_on_demand_resources_visual_state_refactor.md (cwd=$HOME/workspace/solo, rollout_path=$HOME/.codex/sessions/2026/04/24/rollout-2026-04-24T23-57-58-019dc036-50b2-7700-b2e9-f1fdbef0ec8e.jsonl, updated_at=2026-04-24T16:30:31+00:00, thread_id=019dc036-50b2-7700-b2e9-f1fdbef0ec8e, resource model corrected)

### keywords

- resource authorization, checkpoint, on-demand permission, should ask when needed, WorkspaceModal, CONTEXT.md, docs/solo-surface-design.md, attach resource

## Task 3: Replace explanation-heavy copy with visual state and compact cues

### rollout_summary_files

- rollout_summaries/2026-04-24T15-57-58-Nolw-solo_ui_on_demand_resources_visual_state_refactor.md (cwd=$HOME/workspace/solo, rollout_path=$HOME/.codex/sessions/2026/04/24/rollout-2026-04-24T23-57-58-019dc036-50b2-7700-b2e9-f1fdbef0ec8e.jsonl, updated_at=2026-04-24T16:30:31+00:00, thread_id=019dc036-50b2-7700-b2e9-f1fdbef0ec8e, visual-state pass)

### keywords

- 文字提示还是有点多, EmptyVisual, runtime-summary-signal, visual indicators, badges, status dots, compact status surfaces, aria-label, show state not speeches

## User preferences

- the user later said not to start preview services and preferred using `npm run tauri dev` themselves -> do not auto-start local preview/dev servers unless explicitly requested [Task 1]
- when the user said "应该在agent需要的时候再向用户询问访问资源的权限，而不是一上来就需要用户添加文件目录等资源" -> default to ask-at-need resource authorization, not upfront setup [Task 2]
- when the user said "文字提示还是有点多，应该尽量把文字提示转换成图像元素" and "一个好的ui应该是一眼就能让人看出功能，而不是通过文字提示" -> future Solo UI work should prefer state projection over prose [Task 3]
- repeated corrections favored checkpointed, human-controlled intervention and opposed autonomous-feeling setup flows -> keep Solo human-in-the-loop even when reducing narration [Task 2][Task 3]

## Reusable knowledge

- `npm run lint` and `npm run build` are the reliable validation pair after Solo UI refactors [Task 1][Task 3]
- The target mental model in `CONTEXT.md` and `docs/solo-surface-design.md` is task/run/event/resource/artifact/checkpoint observability, not chat-first presentation [Task 1]
- `WorkspaceModal` should stay minimal and function as a chooser/authorization surface, not an explanation panel [Task 2]
- Small visual primitives such as `EmptyVisual`, signal bars, badges, and compact cards can replace many empty-state/helper paragraphs while preserving accessibility via `aria-label` or `title` [Task 3]
- The durable direction is "show state, not speeches" and "resource access at need, not startup" [Task 2][Task 3]

## Failures and how to do differently

- Symptom: the UI technically works but still feels wrong. Cause: overexplaining and drifting toward resource-first setup. Fix: project task/run/resource state visually and move resource access into checkpointed permission requests [Task 1][Task 2]
- Symptom: "less text" pass still does not satisfy the user. Cause: only shortening copy instead of converting explanation into visual state. Fix: replace prose with badges, dots, blocks, cards, and layout relationships whenever possible [Task 3]
- Symptom: time wasted on local preview. Cause: starting a Vite dev server by default. Fix: validate with lint/build unless the user explicitly asks for a live preview [Task 1]

# Task Group: solo design toolchain clarification

scope: Choosing the correct design toolchain for Solo when similar names like `OpenPencil`, `Pencil.dev`, and `open-pencil` create ambiguity.
applies_to: cwd=$HOME and $HOME/workspace/solo; reuse_rule=safe for future Solo design-toolchain clarification; treat installed binaries/packages as machine-specific and time-sensitive

## Task 1: Clarify `pencil` naming and keep the existing `@zseven-w/openpencil` workflow

### rollout_summary_files

- rollout_summaries/2026-04-25T17-43-15-Nl1z-openpencil_toolchain_clarification_kept_existing_setup.md (cwd=$HOME, rollout_path=$HOME/.codex/sessions/2026/04/26/rollout-2026-04-26T01-43-15-019dc5bd-10d5-7ea1-892f-ef277a3e25cd.jsonl, updated_at=2026-04-25T17:54:17+00:00, thread_id=019dc5bd-10d5-7ea1-892f-ef277a3e25cd, toolchain choice clarified)

### keywords

- openpencil, pencil.dev, open-pencil, op CLI, .op, @zseven-w/openpencil, command -v op, command -v pencil, npm list -g --depth=0, solo-ui-contract.md

## User preferences

- when the user said "我说的pencil是https://www.pencil.dev/" -> do not assume `pencil` means the local code artifact or some generic tool; verify the exact product identity first [Task 1]
- when the user concluded "不用了，我们就保持现有的@zseven-w/openpencil方案" -> treat `@zseven-w/openpencil` as the current default Solo design toolchain unless the user explicitly reopens the choice [Task 1]

## Reusable knowledge

- The current Solo design chain is `OpenPencil skill + @zseven-w/openpencil + op CLI + .op artifacts` [Task 1]
- `OpenPencil skill` expects `op`/`.op` semantics and should not be mixed with `Pencil.dev` assumptions [Task 1]
- The Solo design contract treats OpenPencil artifacts as directional design inputs, not pixel-perfect production-code sources [Task 1]
- At the time of the rollout, `op` existed locally, `pencil` did not, and global npm included `@zseven-w/openpencil@0.7.4` [Task 1]

## Failures and how to do differently

- Symptom: toolchain advice is wrong or confusing. Cause: collapsing `openpencil.dev`, `open-pencil/open-pencil`, `Pencil.dev`, and local `@zseven-w/openpencil` into one concept. Fix: verify product identity, installed binary, and file-format assumptions before recommending a workflow [Task 1]
- Symptom: assistant keeps debating alternatives after the user chooses one. Cause: not treating "保持现有方案" as a stop signal. Fix: confirm the selected chain and stop re-evaluating unless asked [Task 1]
