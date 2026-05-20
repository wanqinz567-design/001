# CLAUDE.md

此文件为 Claude Code (claude.ai/code) 在此仓库中工作时提供指导。

## 项目概述

单文件番茄钟网页应用——直接用浏览器打开 `pomodoro.html` 即可。无构建工具、无依赖、无包管理器。

## 运行应用

```
start pomodoro.html
```

## 架构

所有代码都在 `pomodoro.html` 中——HTML 结构、CSS 变量主题系统和原生 JS。JS 遵循以下简单模式：

- **State** — `state` 对象保存当前模式（`work`/`shortBreak`/`longBreak`）、剩余秒数、运行标志、已完成番茄数、当前轮次会话数（0–3）和定时器 ID。通过 `localStorage` 键 `pomodoro` 持久化。
- **History** — 每日番茄数存储在 `localStorage` 键 `pomodoro_history` 中，以 `YYYY-MM-DD` 为键。用于渲染历史弹窗（摘要卡片、14 天 Canvas 图表、日期列表、连续打卡天数计算）。
- **Timer** — `setInterval(tick, 1000)` 驱动倒计时。工作时间结束后：递增会话圆点，记录番茄到历史，自动切换到短休/长休（每完成 4 个番茄触发长休）。休息结束后：切回工作模式。
- **Sound** — 使用 Web Audio API 在计时结束时播放 4 音符上行琶音。无音频文件。
- **Notifications** — 使用 Web Notification API（首次启动时请求权限）。
- **Keyboard** — 空格键开始/暂停，R 键重置，Esc 键关闭历史弹窗。

CSS 自定义属性（`--work`、`--short-break`、`--long-break`、`--bg`、`--card`、`--text`、`--muted`）控制主题。圆形进度环使用 SVG `stroke-dasharray`/`stroke-dashoffset`，周长为 `2π × 116`。

# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.
