# Fable OS 完整评估与路线图

日期：2026-07-12（评估基于 v1.2）。本文档回答七个问题：①它是不是 Skill ②现有模式 ③怎么发 GitHub ④多维打分 ⑤优化路线 ⑥体系拆解与日常用法 ⑦与 Codex 插件的协同。

---

## ① 它是不是一个 Skill？

**是，而且已经在运行。** 判定依据：`~/.claude/skills/fable-operating-system/SKILL.md` 带合法 YAML frontmatter（name + description），Claude Code 每个会话都会把它的 description 放进可用技能列表，命中触发词或复杂任务时加载——本周它已被真实触发多次。

但它同时**超出**了普通 Skill 的范畴。普通 Skill 是「一个领域的做法说明」；Fable 是以 Skill 为核心载体的**操作系统**，还包括四个 Skill 之外的部件：手册（FABLE_OPERATING_SYSTEM.md）、接入点（~/.claude/CLAUDE.md、~/.codex/AGENTS.md）、工件（各项目的 .fable/）、持久记忆。发布时要把这个「一核四件」的结构讲清楚。

## ② 现在有哪些模式

| 模式 | 触发 | 做什么 | 产物 |
|------|------|--------|------|
| **工作模式**（默认） | 复杂任务自动；「按 Fable 流程」 | 五步：理解（10 项检查表）→分类（18 类路由）→规划（三档深度）→执行（17 类标准工作流）→交付（质检清单）。>5 步任务建 `.fable/task.md` | 交付物 + 3 行自检尾注 |
| **验证模式** | 「用 Fable 验证/审计」；验收其他 agent 产出 | 7 维 rubric 逐项判 PASS/FAIL/N/A，每个判定引用证据 | 审计报告，结论三档：通过/有条件通过/不通过；FAIL 给整改项 |
| **复盘模式** | 「Fable 复盘」；出错被纠正后 | 失误标准格式记录→三问（会重复？普适？可执行？）→升级为规则→改对应 reference | CHANGELOG 条目 + 规则更新 |

7 维 rubric：A 理解对齐 / B 路由合规 / C 范围与计划 / D 需求完备 / E 验证证据 / F 沟通合规 / G 授权与安全。关键失败（需求静默缺失、无证据的「完成」、未授权不可逆动作、交付物错位）直接不通过。

另有一个**沟通场景**（不是运行模式）：深挖导师模式（communication-style §7，v1.2 加入）——人生深聊时叠加在工作模式上，先读档案再开口、一次一线、结论写回 vault。

## ③ 发上 GitHub 怎么做

### 第一步（最重要）：先拆隐私层，否则不能公开

当前版本含大量个人内容，直接 push 会泄露：

| 文件 | 个人内容 |
|------|----------|
| communication-style.md §7 | 人生档案路径、私人概念名、会话文件名（2026-07-12 已移入 `personal/` 私有层，§7 只留通用方法） |
| skill-tool-map.md | 你的全部私人 Skill 清单（飞书、写作 IP、视觉 IP）——无隐私风险，保留 |
| agent-integration.md / agents-md-section.md | 你机器的绝对路径、你的 Codex 配置细节——低风险，保留并在 PRIVACY.md 声明 |
| FABLE_CORE.md | 用户名；examples/ 真实任务细节——低风险，保留 |

**方案：拆两层。** `fable-os`（开源版：五步流程、三模式、rubric、模板、通用示例——个人内容全部换成占位符和「填入你自己的」指引）+ `fable-personal`（私有层：§7、skill-tool-map、个人路径，永不上传）。开源价值点恰恰是「教别人怎么给自己的 agent 建这套系统」，所以占位符化不减价值反而是卖点。

### 第二步：仓库骨架与必备文档

```text
fable-os/
├── README.md              # 必备，见下
├── LICENSE                # 推荐 MIT
├── CHANGELOG.md           # 从 evolution.md 抽出通用部分
├── skills/fable-operating-system/   # 脱敏后的完整 skill
│   ├── SKILL.md
│   ├── FABLE_CORE.md
│   ├── references/  templates/  examples/
└── docs/FABLE_OPERATING_SYSTEM.md   # 通用版手册
```

README 必备段落：一句话定位（"给你的 AI agent 装一个操作系统：工作规范 + 验证器 + 跨 agent 契约"）→ 解决什么问题（agent 行为不稳定、说完成没证据、多 agent 无法统一验收）→ 三模式说明 → 安装（`git clone` 后 `cp -R skills/fable-operating-system ~/.claude/skills/`，或说明软链方式）→ 触发词表 → `.fable/task.md` 工件说明 → 跨 agent 注入方法（AGENTS.md 段模板）→ 版本与许可。

### 第三步：两种分发形态（可以都做）

1. **纯 Skill 仓库**（上面的骨架）：门槛最低，用户手动 cp。
2. **Claude Code 插件**：加 `.claude-plugin/marketplace.json`，用户就能 `/plugin marketplace add yourname/fable-os` + `/plugin install` 一键装。你机器上有现成结构可参考：`~/.claude/plugins/marketplaces/openai-codex/`（就是 codex 插件的仓库结构，含 marketplace.json + plugins/ 目录）；你还装了 `skill-creator` 插件可辅助打包。

### 第四步：git 化

`~/Desktop/AI Agent/Fable` 目前**不是 git 仓库**。发布流程：脱敏拆层 → `git init` → `.gitignore`（排除 `.fable/`、个人层文件）→ 首个 commit → GitHub 建 repo → push。

## ④ 多维打分（8 维，满分 10）

| # | 维度 | 分 | 依据 | 主要短板 |
|---|------|----|------|----------|
| 1 | 规范完整度 | **9** | 五步流程、17 类工作流、8 条行为规则全部成文且有示例 | 「简单任务」边界仍靠判断，无硬指标 |
| 2 | 验证器可操作性 | **8** | rubric 有证据标准和三档判定；有一次敢给自己 FAIL 的真实审计 | 只有 1 次真实审计，无历史积累 |
| 3 | 触发可靠性 | **6.5** | CLAUDE.md 确定性指令 + 触发词双保险 | 无 hook 硬保证；长会话中 CLAUDE.md 指令可能被稀释；靠我自觉 |
| 4 | 跨 agent 强制力 | **6** | 入口契约已装进 AGENTS.md + 出口审计机制成文 | Codex 实际服从率 0 次实测；子代理注入靠每次手动 |
| 5 | 工件与证据体系 | **7.5** | task.md 模板 + 首例完整跑通（勾选+证据回填） | 仅 1 例；建文件靠自觉，无自动化 |
| 6 | 进化回路 | **7.5** | CHANGELOG 两天三版真实运转；失误→规则有真实案例（Bash 故障） | 体检例行没有自动化；失误库还很薄 |
| 7 | 可移植/可发布性 | **4** | FABLE_CORE 做到了无工具名 | 其余强绑定个人环境；公开必须先拆层——**最大短板** |
| 8 | 日常易用性 | **7** | 口令齐全、手册完整 | 你得记口令；没有 slash command 一键入口；没有速查卡 |

**总评 ≈ 7.0/10**：骨架和闭环是真实的（不是纸面框架），两大弱点是「强制力的硬度」（3、4）和「可分发性」（7）。

## ⑤ 优化路线（按 ROI 排序）

**P0 —— 高价值低成本，建议先做**

1. **建 slash commands**：`~/.claude/commands/fable.md`、`fable-audit.md`、`fable-review.md` → 你敲 `/fable` 就确定性进入，完全绕开概率触发。这是日常易用性 7→9 的最短路径。
2. **真实任务回填验证**（v1.0 遗留）：接下来 2 个真实任务跑完各审一次，把报告存进 `.fable/audits/`。
3. **审计历史库**：`.fable/audits/YYYY-MM-DD-<任务>.md` 归档制，验证器从「单次判定」变成「有纵向数据的质量档案」。

**P1 —— 结构性升级**

4. **拆层发布**：按 ③ 做 fable-os 开源版 + marketplace.json 插件化 → 可移植性 4→8。
5. **SessionStart hook（可选开关）**：会话开始自动注入一行 Fable 提醒，解决触发稀释；用 update-config skill 装，嫌吵随时卸 → 触发可靠性 6.5→9。
6. **Codex 服从率实测**：真派 1 个任务给 codex:rescue，验收时专门审「它守没守契约」（写没写 task.md、带没带证据），用数据回来修 AGENTS.md 措辞 → 跨 agent 强制力有实证。

**P2 —— 锦上添花**

7. **定期体检自动化**：用 /loop 或 schedule 每周检查接入点还在不在、skill-map 盘点日期是否过期。
8. **审计计分器**：A-G 各 0-3 分加权，出趋势图（走 dataviz）。
9. **多 agent 记忆桥**：把 `.fable/` 与 share-memory skill 的 `AI_MEMORY/` 打通，Codex 和我读写同一份任务状态。

## ⑥ 体系拆解 + 日常使用

### 一核四件

```text
┌─ 核心：fable-operating-system Skill（运行时在 ~/.claude/skills/，源码在 ~/Desktop/AI Agent/Fable/）
│   ├─ SKILL.md            三模式总入口
│   ├─ FABLE_CORE.md       宪法：十条可移植契约
│   ├─ references/ ×9      规则库（理解路由/规划/沟通/能力地图/质检/17类流程/rubric/跨agent/进化）
│   ├─ templates/ ×3       task-file / audit-report / agents-md-section
│   └─ examples/ ×6        六类示范（含真实审计示范）
├─ 件1 手册：FABLE_OPERATING_SYSTEM.md（15 节，人类可读版）
├─ 件2 接入点：~/.claude/CLAUDE.md（管我，全局）＋ ~/.codex/AGENTS.md Fable 段（管 Codex）
├─ 件3 工件：各项目 .fable/task.md（任务状态）＋ 未来的 .fable/audits/（审计档案）
└─ 件4 记忆：各项目 memory/（Fable 项目的系统记忆、个人档案项目的记忆——指针在私有层）
```

### 你的日常口令表

| 你说 | 发生什么 |
|------|----------|
| （直接布置复杂任务） | 自动进工作模式：理解核对→路由→计划→建 task.md→交付带尾注 |
| 「按 Fable 流程」 | 强制工作模式（任何任务） |
| 「用 Fable 验证 X」 | 7 维审计报告（X 可以是我的产出、Codex 的产出、一个 diff、一次汇报） |
| 「Fable 复盘」 | 刚才的失误 → 规则 + CHANGELOG |
| 「深挖」「用那种深度跟我聊」 | 导师模式：先读你的档案，一次一线往深挖，结论写回 vault |
| （简单问题） | 什么都不启用，直接答——防过度工程也是规则 |

### 建议的使用节奏

- **布置任务时**：不用想 Fable，正常说话（复杂度够就自动启用）。
- **验收时**：对要紧的交付补一句「用 Fable 验证」——尤其是 Codex 或子代理做的。
- **我出错你纠正后**：加一句「Fable 复盘」，把这次错变成永久规则。
- **每隔几周**：说「Fable 体检」——查接入点、更新 skill-map 盘点日期（P2-7 自动化前先手动）。

## ⑦ 与 Codex 的协同（以 Fable 为前提）

### 现状：不是「之后接上」，是已经接上了

你给的 `openai/codex-plugin-cc` **已装在你机器上**（`~/.claude/plugins/marketplaces/openai-codex/`），提供：`/codex:review`（只读审查）、`/codex:adversarial-review`（对抗式审查）、`/codex:rescue`（委派任务）、`/codex:transfer`（转移会话）、`/codex:status` `/codex:result` `/codex:cancel`（后台任务管理）、`codex-rescue` 子代理。加上你自建的 partner-skill（搭子），Claude→Codex 的通道有两条。Fable 的治理层也已就位（AGENTS.md 契约段是昨天装的）。

### 协同架构：Fable 当总控 + 验收，Codex 当执行者

```text
你 → 我（Fable 工作模式：理解/规划/拆分）
        → 委派 Codex（入口：AGENTS.md 契约 + prompt 注入；要求更新同一份 .fable/task.md）
        → Codex 后台执行（/codex:status 监控）
        → 我验收（Fable 验证模式 7 维审计；不过审打回）
        → 交付给你（带自检尾注 + 如实报告审计结论）
```

### 任务分派矩阵

**适合派给 Codex**：大规模代码实现/重构（长上下文优势）、测试补全、独立后端模块、我卡住 2-3 轮后的二次诊断（codex:rescue 本来就是这个用途）、要第二意见时的对抗式 review、额度紧张时的后台长任务。

**必须留在我这**：需求理解与规划、任务拆分、UI/交互打磨、**最终 Fable 审计（验收权不外包）**、一切涉及飞书生态/写作 IP/视觉 IP/你个人档案的任务（Codex 没有这些 skills 和上下文）、高风险操作（发送/发布/删除——授权链不跨 agent 传递）。

### 边界提醒

反方向有你自己定的 Claude CLI Access Boundary：Codex 不能主动调用我，只有你说「搭子/Partner」才行。Fable 契约段已显式声明不修改这条边界——所以协同永远是「我派 Codex」单向发起，或你显式启动搭子工作流。

---

*本评估建议先做 P0 三项；做完拆层发布（P1-4）就可以上 GitHub。*
