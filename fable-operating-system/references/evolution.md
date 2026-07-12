# 复盘模式与进化回路

Fable 不是写完就冻结的文档——出错、被纠正、发现新模式时，通过本文件的回路把经验变成规则。

## 复盘模式流程

触发：我出错被用户纠正后、任务复盘时、用户说「Fable 复盘」。

1. **记录失误**（标准格式见下）——先如实记，不急着改规则。
2. **判断是否升级为规则**，三问：会重复发生吗？跨任务类型普适吗？能写成「当…时，我应该…，因为…」的可执行形式吗？三问皆是才升级；否则只留记录观察。
3. **改对应 reference**：规则写进它所属的文件（路由类→task-routing，质检类→quality-checks，沟通类→communication-style……），不是都堆在这里。
4. **记 CHANGELOG**（本文件底部）：一行写清改了什么、源于哪次失误。
5. **跨会话有价值的**同时写入持久记忆。
6. 改动大时同步版本号、FABLE_OPERATING_SYSTEM.md 手册和 `~/.claude/skills/` 安装副本。

## 失误记录格式

```markdown
### <日期> <一句话概括>
- 表现：我做了/没做什么
- 场景：什么任务、什么触发条件下
- 原因：为什么会发生（诚实归因，不找借口）
- 影响：造成了什么后果
- 处置：只记录观察 / 已升级为规则（写明改了哪个文件哪条）
```

## 保鲜例行

- **新装 Skill**（Claude 侧或 Codex 侧）→ 更新 `references/skill-tool-map.md` 并改盘点日期。
- **每次对 skill 内容的修改** → 同步 `~/.claude/skills/fable-operating-system/` 安装副本（源目录整体覆盖过去），否则线上跑的是旧版。
- **大版本升级** → 同步 FABLE_OPERATING_SYSTEM.md 手册 + 持久记忆索引；必要时给 VALIDATION_TESTS.md 补真实测试。
- **接入点体检**（偶尔）：确认 `~/.claude/CLAUDE.md` 的 Fable 指令和 `~/.codex/AGENTS.md` 的 Fable Process Contract 段还在、未被其他改动覆盖。

## 版本规则

- 主版本（v1→v2）：架构变化（新模式、新身份）。
- 次版本（v1.0→v1.1）：新增规则、模板、接入点。
- 微调（措辞、修错字）不记版本，只改文件。
- 当前版本声明以本文件 CHANGELOG 最新条目为准。

## CHANGELOG

### v1.3 — 2026-07-12

- 新增 **slash commands 一键入口**：`/fable`（工作模式）、`/fable-audit`（验证模式）、`/fable-retro`（复盘模式）、`/fable-check`（体检）。源文件在仓库 `commands/`，安装于 `~/.claude/commands/`。触发从概率性升级为确定性。
- 新增**审计历史库与计分**：报告归档 `.fable/audits/YYYY-MM-DD-<对象>.md`；每维 0-3 分、总分百分比看趋势（validation-rubric.md）。首例归档 2026-07-11-v1-self-check.md（18/21）。
- **隐私拆层**（发布前置）：communication-style §7 改为纯通用方法；个人档案指针、概念清单移入 `fable-operating-system/personal/deep-dive-pointers.md`（.gitignore 排除，仅存本机与安装副本）。
- agent-integration.md 增加 share-memory 桥：有 `AI_MEMORY/` 的项目把 task.md 关键状态同步过去。
- **公开发布**：GitHub `ycl-2004/yc_Fable5`（public），新增 README.md、LICENSE（MIT）、PRIVACY.md、.gitignore。
- 未做与原因：SessionStart hook（沿用既定决策——触发不稳时再装）；Codex 服从率实测（待下一个真实委派任务）；体检自动化（先用 `/fable-check` 手动，稳定后再上 schedule）。

### v1.2 — 2026-07-11

- communication-style.md 新增 **§7 深度人生对话（导师模式）**：十条方法（档案盲区切入、一次一线、问题少而重、证伪恐惧、退出口常设、小步实验、结论写回档案保留原话等），源于同日一次人生深挖会话（用户明确认可）。原则：方法进 Skill 全局复用，个人事实只留在私有档案与项目记忆（Skill 只存指针）。
- SKILL.md 触发词加入人生深聊类；`~/.claude/CLAUDE.md` 增加对应指令（深聊前先读私有层指针所列的档案与记忆）。

### v1.1 — 2026-07-11

- 新增**验证模式**：7 维 rubric（validation-rubric.md）、审计报告模板与 3 行自检尾注（templates/audit-report.md）、真实审计示范（examples/audit-example.md，对 v1.0 自检任务的审计，C 维如实 FAIL）。
- 新增**跨 agent 协议**：FABLE_CORE.md 可移植十条契约、agent-integration.md（入口注入+出口审计）、agents-md-section.md 注入模板；实装两个接入点——新建 `~/.claude/CLAUDE.md` 全局指令、`~/.codex/AGENTS.md` 追加 Fable Process Contract 段（显式不削弱 Claude CLI Access Boundary）。
- 新增**工作文件机制**：`.fable/task.md`（templates/task-file.md），>5 步任务强制；来源：v1.0 自检任务审计的 C 维 FAIL 整改。
- 新增**复盘模式与本文件**（进化回路、版本规则、保鲜例行）。
- quality-checks.md 新增错误恢复规则：基础设施故障→换等价工具。来源实例：2026-07-11 Bash 分类器临时故障，改用 Read 完成了同样的存在性检查。
- SKILL.md 升级为三模式入口，触发词加入「按 Fable 流程」「用 Fable 验证」「Fable 审计」「Fable 复盘」。

### v1.0 — 2026-07-11

- 初版：五步核心流程、6 个 references、5 个 examples、FABLE_OPERATING_SYSTEM.md 手册、5 项模拟验证。
