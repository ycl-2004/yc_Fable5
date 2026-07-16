---
description: Fable 体检：接入点、安装链接、能力地图与审计保鲜
---

用 Skill 工具加载 fable-operating-system，按 references/evolution.md 的保鲜例行逐项体检，每项给 PASS/FAIL 与证据，发现漂移直接修复并汇报：

1. `~/.claude/CLAUDE.md` 的 Fable 指令段还在且未被其他改动覆盖？
2. `~/.codex/AGENTS.md` 末尾的 "# Fable Process Contract" 段还在？Scope note（不削弱 Claude CLI Access Boundary）完整？条目与 templates/agents-md-section.md 模板一致（改契约时三件套要同步）？
3. 安装链接健康？`~/.claude/skills/fable-operating-system` 和 4 个 `~/.claude/commands/fable*.md` 都应是指向 `"$HOME/Desktop/AI Agent/Fable/"` 下源文件的 symlink 且目标可读（`readlink` 逐个确认 + 通过链接路径读一个文件实测）。若发现是普通目录/文件（symlink 回退状态），改用 `diff -rq "$HOME/Desktop/AI Agent/Fable/fable-operating-system" "$HOME/.claude/skills/fable-operating-system"` 与逐文件 diff commands 查漂移并同步
4. references/skill-tool-map.md 盘点日期距今 >30 天，或实际 Skill 集合有变化未入图？（对照 `ls -la ~/.claude/skills/`，注意识别悬空 symlink——目标被删的链接会在列表里但不可用）
5. `.fable/audits/` 最近一次审计距今多久？超过一个月提醒补一次真实审计
6. 私有层 `fable-operating-system/personal/` 仍被 .gitignore 排除、未被误提交？（`git -C "$HOME/Desktop/AI Agent/Fable" status --ignored -s | head`，并确认 `git -C "$HOME/Desktop/AI Agent/Fable" ls-files | grep personal` 为空）
