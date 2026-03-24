# Codex Global Memories

这个目录用于给 Codex 提供跨任务可复用的全局记忆。

## 文件分工

- `PROFILE.md`
  - 长期稳定的用户画像、沟通偏好、工作偏好
- `ACTIVE.md`
  - 每次新任务前都值得优先读取的高优先级规则
- `LEARNINGS.md`
  - Lessons Learned、用户纠正、可复用技巧、最佳实践
- `ERRORS.md`
  - 环境坑、调试经验、易复现的失败模式
- `FEATURE_REQUESTS.md`
  - 当前 Codex / 工作流缺失但值得长期跟踪的能力
- `AGENTS_SNIPPET.md`
  - 建议粘贴到全局 `~/.codex/AGENTS.md` 的接线片段
- `NIGHTLY_REVIEW_PROMPT.md`
  - 夜间精炼 / 定期复盘 prompt

## 建议工作流

1. 新任务开始前，先读 `PROFILE.md` 和 `ACTIVE.md`
2. 任务中如果出现高价值经验：
   - 经验/技巧 -> `LEARNINGS.md`
   - 错误/排障 -> `ERRORS.md`
   - 能力缺口 -> `FEATURE_REQUESTS.md`
3. 周期性回顾这些文件，把稳定规则提升到 `ACTIVE.md`
4. 只有当某条规则已经足够稳定、且需要成为顶层硬规则时，才考虑写入 `AGENTS.md`
