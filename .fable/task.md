# Fable 任务文件

> 已完成任务段滚动归档于 `.fable/history.md`（任务 1–4 在彼处；滚动规则见 templates/task-file.md，v1.6 起）。

# 任务 5：v1.6 task.md 滚动归档（2026-07-19）

**来源**：用户发现「调用 skill 后 `.fable/task.md` 还是无限 append」，问抗膨胀机制是否已有覆盖 → 诊断：v1.5 抗膨胀出口只覆盖规则层（references 行数预算、CHANGELOG 滚动），漏了各项目工作文件层。

## 目标（一句话）

把 v1.5 的滚动归档原则推广到工作文件层：task.md 只留进行中任务，完成段按原文滚入同目录 `.fable/history.md`。

## 验收标准

- 规则主体落在单一事实源 templates/task-file.md；SKILL.md / FABLE_CORE #3 / agents-md-section / ~/.codex/AGENTS.md 同步
- evolution.md 有 v1.6 条目，且按滚动规则 v1.4 原文移入 HISTORY.md
- 本仓库 task.md 从 176 行降到只含本任务；任务 1–4 原文在 .fable/history.md
- README / 手册 / 记忆版本同步 v1.6；push 成功、泄漏检查通过

## 需求清单（append-only）

- [x] 1 templates/task-file.md 使用规则补滚动条款（含 append-only 语义澄清）
- [x] 2 SKILL.md 计划规则行 + FABLE_CORE #3 + agents-md-section + ~/.codex/AGENTS.md 同步
- [x] 3 evolution.md v1.6 条目 + v1.4 滚入 HISTORY.md
- [x] 4 本仓库 .fable/task.md 首次滚动（任务 1–4 → history.md）
- [x] 5 README / FABLE_OPERATING_SYSTEM.md 版本与结构同步
- [x] 6 记忆更新（fable-operating-system-skill.md + MEMORY.md → v1.6）
- [x] 7 git commit + push + 泄漏检查

## 决策日志

- 滚动而非删除：task.md 是验证模式第一证据源，直接截减会毁证据；沿用 CHANGELOG→HISTORY 的原文归档模式（四问守恒闸：既有原则推广，不新增规则类别）。
- 滚动时机定为「开新任务时」：完成刚收尾的任务下个会话可能还要续写证据；开新任务时上一个必然已结束，且操作者正好在编辑该文件。
- 软预算 ~200 行：单个进行中任务通常 <60 行，超 200 行说明有完成段未滚动或单任务过度铺陈。

## 验证证据

- 滚动生效：task.md 176 行 → 38 行；history.md 182 行；`diff` 抽查任务 2 段与 git HEAD 原文一致（按原文归档）
- 规则同步：`grep -c history.md` 在 task-file.md / SKILL.md / FABLE_CORE.md / agents-md-section.md / ~/.codex/AGENTS.md 各命中 1
- 系统净瘦身：evolution.md 69 行 → 66 行（v1.6 条目加入的同时 v1.4 滚入 HISTORY.md，43 行）
- symlink 实读：`$HOME/.claude/skills/.../templates/task-file.md` 经链接 grep 到滚动条款；SKILL.md 改动触发本会话技能列表实时刷新
- push：见交付汇报（commit 哈希与泄漏检查结果）
