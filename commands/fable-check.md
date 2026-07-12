---
description: Fable 体检：接入点、安装副本同步、能力地图与审计保鲜
---

用 Skill 工具加载 fable-operating-system，按 references/evolution.md 的保鲜例行逐项体检，每项给 PASS/FAIL 与证据，发现漂移直接修复并汇报：

1. `~/.claude/CLAUDE.md` 的 Fable 指令段还在且未被其他改动覆盖？
2. `~/.codex/AGENTS.md` 末尾的 "# Fable Process Contract" 段还在？Scope note（不削弱 Claude CLI Access Boundary）完整？
3. 安装副本与源目录一致？（`diff -rq "~/Desktop/AI Agent/Fable/fable-operating-system" ~/.claude/skills/fable-operating-system`）
4. references/skill-tool-map.md 盘点日期距今 >30 天，或有新装 Skill 未入图？
5. `.fable/audits/` 最近一次审计距今多久？超过一个月提醒补一次真实审计
6. 私有层 `fable-operating-system/personal/` 仍被 .gitignore 排除、未被误提交？（`git -C "~/Desktop/AI Agent/Fable" status --ignored -s | head`）
