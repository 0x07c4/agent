thread_id: 019dc036-50b2-7700-b2e9-f1fdbef0ec8e
updated_at: 2026-04-24T16:30:31+00:00
rollout_path: $HOME/.codex/sessions/2026/04/24/rollout-2026-04-24T23-57-58-019dc036-50b2-7700-b2e9-f1fdbef0ec8e.jsonl
cwd: $HOME/workspace/solo
git_branch: main

# Solo UI was pushed toward a more visual, less text-heavy control plane, and the user corrected the resource-authorization model.

Rollout context: The work happened in `$HOME/workspace/solo`. The user was steering Solo’s product direction and UI shape, with repeated emphasis on keeping the interface readable at a glance and avoiding unnecessary local previews or dev-server startup.

## Task 1: Reframe Solo toward task/running-state UI and keep settings/validation intact

Outcome: success

Preference signals:
- The user asked to continue the Solo project, then later clarified they did **not** want the assistant to start a preview server and preferred using `npm run tauri dev` themselves. That suggests future work should avoid automatically starting local preview servers unless explicitly requested.
- The user described Solo’s target as a product where UI should be instantly legible rather than explanation-driven, indicating a preference for functional surfaces over verbose text.

Key steps:
- The assistant first read `~/.codex/memories/PROFILE.md`, `ACTIVE.md`, and Solo notes (`CONTEXT.md`, `docs/solo-surface-design.md`, `~/workspace/notes/solo-runtime-boundary.md`) to re-anchor on the product direction.
- The main UI was migrated toward a control-plane framing: top bar metrics for workstreams/exceptions/resources, command-bar quick actions, resource/exception/task groupings, and runtime detail panes.
- A regression was found where the settings entry and connection/provider controls could have been lost during the migration; those were restored before validation.
- `npm run lint` and `npm run build` were run successfully. A local Vite dev server was started once, but the user objected; the server was then stopped/left unreferenced, and the later approach was to avoid starting preview services altogether.

Failures and how to do differently:
- The first pass overemphasized explanatory text and leaned on “resource attached” as a default UI cue. The user later corrected that direction.
- The assistant also accidentally tried to start a Vite dev server; future Solo UI work should default to non-starting verification unless the user explicitly asks for a live preview.

Reusable knowledge:
- In this repo, `npm run lint` and `npm run build` are reliable validation steps after UI refactors.
- The project has a strong control-plane direction in `CONTEXT.md` and `docs/solo-surface-design.md`: tasks/runs/events/resources/artifacts/checkpoints are the target mental model.
- The UI already has enough structure to support top-bar metrics, a runtime summary grid, task lanes, and inspector tabs without needing a separate preview app flow.

References:
- [1] `npm run lint` and `npm run build` both passed after the main refactor.
- [2] The assistant initially started Vite (`npm run dev -- --host 127.0.0.1`) and hit `Error: listen EPERM: operation not permitted 127.0.0.1:5173`, then switched to escalated execution to start it; the user later said not to start services at all.
- [3] The key edited files were `src/App.jsx` and `src/App.css`, with support changes in `src/components/SettingsModal.jsx` and related UI surfaces.

## Task 2: Rework resource access so it is requested on-demand, not preconfigured

Outcome: success

Preference signals:
- The user explicitly said: “应该在agent需要的时候再向用户询问访问资源的权限，而不是一上来就需要用户添加文件目录等资源” — this is a strong default preference for **on-demand resource requests** rather than forcing users to pre-add directories.
- The user also said the current UI has too much text and that a good UI should be “一眼就能让人看出功能，而不是通过文字提示”, which implies future UI should prefer visual affordances over explanatory copy.

Key steps:
- The assistant revised the previous resource-first stance and updated the implementation so resource access is not framed as a mandatory initial step.
- The command bar and status surfaces were trimmed to remove prompts that nudged the user to add resources up front.
- `CONTEXT.md` and `docs/solo-surface-design.md` were updated so the product direction explicitly says resource access should be requested when needed, ideally at a checkpoint, rather than required at startup.
- A lightweight memory note was added to capture the rule that UI should stay self-evident and not depend on explanatory text.

Failures and how to do differently:
- The first iteration still used “attach resource” language too prominently and treated resource presence as a normal starting condition. The user rejected that model.
- Future UI changes should treat resource access as a checkpoint/permission flow, not as a default setup flow.

Reusable knowledge:
- `docs/solo-surface-design.md` now records the intended flow: agent requests a resource, the system projects that as a checkpoint, and the user grants access; the paperclip remains only as a shortcut, not as the required entrypoint.
- `CONTEXT.md` now also records that resource access should be on-demand and that UI should rely on state and objects rather than explanatory paragraphs.

References:
- [1] `CONTEXT.md` gained the rule that resource access should be requested at need, and that good UI should be understandable at a glance.
- [2] `docs/solo-surface-design.md` now says resource attach is a checkpoint-driven flow and not a startup requirement.
- [3] `src/components/WorkspaceModal.jsx` was shortened to a bare directory-picker flow, with most explanatory copy removed.

## Task 3: Reduce on-screen text and convert explanatory surfaces into visual indicators

Outcome: success

Preference signals:
- The user said: “文字提示还是有点多，应该尽量把文字提示转换成图像元素” — this is a durable UI preference for visual signals over prose.
- Earlier, the user also said the UI should be obvious at a glance, not explained through text, reinforcing that the next default should be compact, symbolic, and state-driven.

Key steps:
- The assistant introduced an `EmptyVisual` component and used it to replace many explanatory empty states with a small three-block visual indicator.
- Several text-heavy helper lines were removed or demoted: task board explanations, decision hints, preview footers, tab descriptions, resource captions, and some inspector/subtitle text.
- Runtime summary cards were changed from “label + value + detail text” to “label + value + visual signal bars”; the extra detail is preserved in `title` for accessibility and hover help instead of always-on prose.
- Left-rail empty states, resource states, file-tree states, runtime-event states, and proposal/preview states were visually compressed.
- The assistant also updated `LEARNINGS.md` to record the rule: prefer state dots, counts, badges, progress blocks, icon buttons, and layout relationships over explanatory copy.

Failures and how to do differently:
- The first “less text” pass still left too many short explanation lines in place. The user wanted a stronger shift: text should become status shapes and visual cues, not just shorter sentences.
- Future UI work should treat any new helper copy as suspicious unless it is essential for action or error recovery.

Reusable knowledge:
- The repo already has enough primitives to support visual states: badges, chips, runtime summary cards, task lanes, inspector panels, and compact empty states.
- A small SVG-like state widget (`EmptyVisual`) can replace many explanatory paragraphs while keeping accessibility via `aria-label`.
- The repo’s own notes now explicitly favor “visualized decision flow” and observability-first UI over chat-first UI.

References:
- [1] `src/App.jsx` now includes `EmptyVisual`, and multiple `panel-collapsed-note` / `proposal-empty` / task-board empty states were switched to it.
- [2] `src/App.css` was updated to support `EmptyVisual` and `runtime-summary-signal` bars, and several helper-text styles were removed.
- [3] `~/workspace/notes` and the Solo docs continue to anchor the higher-level product direction, but the immediate UI preference is now clearly: fewer words, more visual state.

## Task 4: Keep the interaction model human-controlled and checkpoint-driven

Outcome: success

Preference signals:
- Across the rollout, the user repeatedly pushed against autonomous/assumptive flows and toward “ask when needed” behavior.
- The user’s direction implies future agents should default to checkpointed, user-authorized intervention rather than front-loading configuration or acting as if the user wants a verbose guided tour.

Key steps:
- The assistant kept Solo’s existing human-in-the-loop posture intact while making the interface more compact.
- The assistant did not remove the core human-control surfaces; instead it trimmed the explanatory shell around them.
- The assistant validated after each major pass with lint/build, which is a reliable pattern for future UI changes.

Failures and how to do differently:
- The main failure mode was overexplaining. The fix is not to add more micro-copy; it is to replace copy with state projection, and only surface text when it is actionable or error-related.

Reusable knowledge:
- The product direction now has explicit support in `CONTEXT.md`, `docs/solo-surface-design.md`, and `LEARNINGS.md`: Solo should behave like a control plane, not a chatty guide.
- The user’s preferred default for similar future work is now: minimal text, clear state, and permission requests only when an agent actually needs them.

References:
- [1] `npm run lint` and `npm run build` both passed after the final visual-state refactor.
- [2] The latest edited files were `CONTEXT.md`, `docs/solo-surface-design.md`, `src/App.css`, `src/App.jsx`, and supporting component files (`ChatPane`, `InspectorPane`, `SettingsModal`, `Sidebar`, `WorkspaceModal`).

