# Fable OS 版本史归档

`references/evolution.md` 的 CHANGELOG 只保留最近两个版本，更早条目按原文归档于此（滚动归档规则见 evolution.md 保鲜例行，v1.5 起）。

> 注：v1.3 的标题行曾在 v1.4 编辑中丢失，其条目一度被并入 v1.4 名下——迁移至此时已恢复（发现于 2026-07-17 十维审计后的归档作业，详见 v1.5 CHANGELOG）。

## v1.4 — 2026-07-15

- **契约 #6 双向化**（对齐 Fable 5 运行时的自主推进规范）：高风险先授权之外，补「可逆且在原始请求范围内的直接做完再汇报，不中途请示；回合结束前最后一段不能是未执行的计划或承诺」。planning-framework 新增 §回合完整性；communication-style 反模式清单 +1。三件套（FABLE_CORE / agents-md-section / `~/.codex/AGENTS.md`）已同步。
- **活水条款**（FABLE_CORE §边界 + SKILL.md）：本系统是运行时规范的快照，与 agent 运行时规范冲突时按更严格/更新者执行并走复盘更新契约；运行时已内建保证的机制（临时目录位置、记忆文件格式等）不复制进契约，避免多一份漂移源。
- **symlink 替代 rsync**：安装位置（skills 目录 + 4 个 commands）改为指向源目录的 symlink，消除「忘记同步」整类失误；可行性依据：全部 lark-* skill 一直以 symlink 形式正常被加载。体检第 3 项从 diff 改为链接健康检查。
- **手册瘦身（单一事实源）**：FABLE_OPERATING_SYSTEM.md 与 references 重复的细则段（原 §3–§10、§14）替换为规则库导览与指针，保留独有内容（角色定位、核心原则、自诊断史、改进规则、架构叙事）。起因：双写已产生数字漂移（「19 类路由」实为 18）。
- **quality-checks 证据闸门**：改系统状态（重启/删除/改配置）前核对证据支持该根因——模式匹配 ≠ 诊断。
- **bug 修复**：/fable-check 示例命令引号内 `~` 不展开（改 `$HOME`，否则照跑必失败）；体检盲区——commands 装在 `~/.claude/commands/` 但同步检查只覆盖 skills 目录，已补；audit-report 模板总分分母写死 x/21 与「N/A 不计入分母」矛盾，改 x/(计入维度×3)。
- **能力地图保鲜**：lark-mail / lark-approval / lark-okr / lark-attendance / lark-apps / lark-skill-maker 六个 skill 已随 lark 套件更新下架（`~/.claude/skills/` 残留悬空 symlink，已报告用户待清理），从能力地图与路由表移除；盘点日期更新 2026-07-15。
- 失误沉淀：v1.3 把不可执行的命令写进了体检文件且漏掉 commands 同步面——规则升级为「体检/命令文件里的示例命令必须实际跑通一次再入库」。

## v1.3 — 2026-07-12

- 新增 **slash commands 一键入口**：`/fable`（工作模式）、`/fable-audit`（验证模式）、`/fable-retro`（复盘模式）、`/fable-check`（体检）。源文件在仓库 `commands/`，安装于 `~/.claude/commands/`。触发从概率性升级为确定性。
- 新增**审计历史库与计分**：报告归档 `.fable/audits/YYYY-MM-DD-<对象>.md`；每维 0-3 分、总分百分比看趋势（validation-rubric.md）。首例归档 2026-07-11-v1-self-check.md（18/21）。
- **隐私拆层**（发布前置）：communication-style §7 改为纯通用方法；个人档案指针、概念清单移入 `fable-operating-system/personal/deep-dive-pointers.md`（.gitignore 排除，仅存本机与安装副本）。
- agent-integration.md 增加 share-memory 桥：有 `AI_MEMORY/` 的项目把 task.md 关键状态同步过去。
- **公开发布**：GitHub `ycl-2004/yc_Fable5`（public），新增 README.md、LICENSE（MIT）、PRIVACY.md、.gitignore。
- 未做与原因：SessionStart hook（沿用既定决策——触发不稳时再装）；Codex 服从率实测（待下一个真实委派任务）；体检自动化（先用 `/fable-check` 手动，稳定后再上 schedule）。

## v1.2 — 2026-07-11

- communication-style.md 新增 **§7 深度人生对话（导师模式）**：十条方法（档案盲区切入、一次一线、问题少而重、证伪恐惧、退出口常设、小步实验、结论写回档案保留原话等），源于同日一次人生深挖会话（用户明确认可）。原则：方法进 Skill 全局复用，个人事实只留在私有档案与项目记忆（Skill 只存指针）。
- SKILL.md 触发词加入人生深聊类；`~/.claude/CLAUDE.md` 增加对应指令（深聊前先读私有层指针所列的档案与记忆）。

## v1.1 — 2026-07-11

- 新增**验证模式**：7 维 rubric（validation-rubric.md）、审计报告模板与 3 行自检尾注（templates/audit-report.md）、真实审计示范（examples/audit-example.md，对 v1.0 自检任务的审计，C 维如实 FAIL）。
- 新增**跨 agent 协议**：FABLE_CORE.md 可移植十条契约、agent-integration.md（入口注入+出口审计）、agents-md-section.md 注入模板；实装两个接入点——新建 `~/.claude/CLAUDE.md` 全局指令、`~/.codex/AGENTS.md` 追加 Fable Process Contract 段（显式不削弱 Claude CLI Access Boundary）。
- 新增**工作文件机制**：`.fable/task.md`（templates/task-file.md），>5 步任务强制；来源：v1.0 自检任务审计的 C 维 FAIL 整改。
- 新增**复盘模式与 evolution.md**（进化回路、版本规则、保鲜例行）。
- quality-checks.md 新增错误恢复规则：基础设施故障→换等价工具。来源实例：2026-07-11 Bash 分类器临时故障，改用 Read 完成了同样的存在性检查。
- SKILL.md 升级为三模式入口，触发词加入「按 Fable 流程」「用 Fable 验证」「Fable 审计」「Fable 复盘」。

## v1.0 — 2026-07-11

- 初版：五步核心流程、6 个 references、5 个 examples、FABLE_OPERATING_SYSTEM.md 手册、5 项模拟验证。
