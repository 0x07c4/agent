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
