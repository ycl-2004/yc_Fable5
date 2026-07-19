# Fable 任务归档

> `.fable/task.md` 只保留进行中任务；已完成任务段按原文滚动至此（规则见 templates/task-file.md，v1.6 起）。本文件 append-only，不进日常 context。

---

# Fable 任务文件

- **任务**：把 fable-operating-system 升级为完整的「Fable」系统（v1.1）
- **日期**：2026-07-11
- **来源**：用户指令「把这个 skills 完整整流成之后可以使用的 fables……使用这个 skills 相当于用 fables 做验证……确保之后我的 agent 跟随 fables 流程」+「continue doing the task and fix P1 to P3」

## 目标（一句话）

让 Fable 具备三重身份：工作规范（已有）、验证器（新）、跨 agent 协议（新），并接好进化回路和可靠加载。

## 验收标准

- 验证模式可用：有 rubric、审计报告模板，且有一份真实审计示范
- 跨 agent 可注入：FABLE_CORE.md 可移植、~/.claude/CLAUDE.md 与 ~/.codex/AGENTS.md 两个接入点已装
- 进化回路成文：版本号 + CHANGELOG + 失误→规则流程
- 手册与安装副本同步、验证报告补第 6 测、记忆更新

## 需求清单（append-only）

- [x] P1-1 references/validation-rubric.md（7 维 rubric + 证据标准 + 三档判定）
- [x] P1-2 templates/task-file.md
- [x] P1-3 templates/audit-report.md
- [x] P1-4 SKILL.md 三模式改造 + 「Fable」触发词
- [x] P1-5 examples/audit-example.md（真实审计上一轮自检任务）
- [x] P2-1 FABLE_CORE.md（可移植核心契约）
- [x] P2-2 references/agent-integration.md
- [x] P2-3 templates/agents-md-section.md
- [x] P2-4 新建 ~/.claude/CLAUDE.md 加载指令
- [x] P2-5 ~/.codex/AGENTS.md 追加 Fable 段（不得削弱 Claude CLI Access Boundary）
- [x] P3-1 references/evolution.md（v1.1 + CHANGELOG + 复盘流程）
- [x] P3-2 quality-checks.md 增补「基础设施故障→换等价只读工具」规则
- [x] P3-3 FABLE_OPERATING_SYSTEM.md 手册同步 v1.1（新增第 15 节）
- [x] P3-4 VALIDATION_TESTS.md 补测试 6（验证模式，真实执行）
- [x] P3-5 同步安装到 ~/.claude/skills/fable-operating-system/
- [x] P3-6 更新持久记忆

## 决策日志

- 2026-07-11 用户确认「按推荐来」（continue and fix P1 to P3）：加载强度 = CLAUDE.md 指令 + 触发词、不装 hook；复杂交付附 3 行自检尾注、完整审计仅在「用 Fable 验证」时出；命名照用「Fable」。
- 2026-07-11 Codex AGENTS.md 为英文规范文件 → Fable 段用英文写，并显式声明不修改 Claude CLI Access Boundary，冲突时从严。
- 2026-07-11 FABLE_CORE.md 放在 skill 目录内（随 skill 一起分发），用纯契约表述、不含 Claude Code 专属工具名。

## 验证证据

- 验证模式可用 → validation-rubric.md / audit-report.md 已创建；真实审计示范 examples/audit-example.md（结论：有条件通过，C 维 FAIL 及整改）
- 跨 agent 接入 → `~/.claude/CLAUDE.md` 已新建（5 条指令）；`~/.codex/AGENTS.md` 末尾已有 "# Fable Process Contract" 段（Edit 成功，含 Scope note 保护 Claude CLI Access Boundary）
- 安装同步 → rsync 后 `find ~/.claude/skills/fable-operating-system -type f` 列出 20 个文件（v1.0 为 12 个），含全部新增文件
- 进化回路 → evolution.md CHANGELOG 有 v1.0/v1.1 两条；quality-checks.md 新增基础设施故障规则（含 2026-07-11 Bash 故障实例）
- 手册/验证 → FABLE_OPERATING_SYSTEM.md 第 15 节 + 版本标注 v1.1；VALIDATION_TESTS.md 测试 6（真实执行）
- 记忆 → fable-operating-system-skill.md 更新至 v1.1，MEMORY.md 索引行同步

## 状态：✅ 全部完成（2026-07-11）

---

# 任务 2：v1.3 完整修缮 + 发布 GitHub（2026-07-12）

**来源**：「好的完整修完 然后 upload on https://github.com/ycl-2004/yc_Fable5 create a git repo once its done」

## 需求清单（append-only）

- [x] P0-1 四个 slash commands（/fable /fable-audit /fable-retro /fable-check），装入 ~/.claude/commands/ 并入库 commands/
- [x] P0-3 审计历史库：.fable/audits/ + 首例归档（v1 自检审计，18/21）+ rubric 计分与归档规则
- [x] P1-4 隐私拆层：§7 通用化，个人指针移入 fable-operating-system/personal/（gitignore）
- [x] P2-8 计分器（轻量版：0-3/维 + 总分百分比）
- [x] P2-9 share-memory 桥（agent-integration.md 一条规则）
- [x] 手册同步 v1.3（§15.4）+ evolution CHANGELOG v1.3
- [x] README / LICENSE(MIT) / PRIVACY / .gitignore
- [x] git init + 推送 ycl-2004/yc_Fable5
- [ ] （待真实场景）P0-2 真实任务回填验证 ×2；P1-6 Codex 服从率实测；P2-7 体检自动化

## 决策日志

- 仓库已存在且为 PUBLIC → 必须先做隐私拆层再推送（个人指针留本机 personal/，泄漏 grep 复检通过后才 git add）。
- SessionStart hook 沿用既定决策不装；体检先用 /fable-check 手动。

## 验证证据

- slash commands 生效：本会话可用列表出现 fable / fable-audit / fable-retro / fable-check
- 泄漏检查：敏感词 grep 仅命中 personal/（已 gitignore）；`git ls-tree -r main | grep personal` 为空
- 推送成功：root-commit 1952e6a，33 files，`gh repo view` pushedAt=2026-07-12T04:37Z，branch main
- 安装副本已 rsync（含 personal/ 私有层）

---

# 任务 3：v1.4 全面优化（bug 修复 + 单一事实源 + 对齐 Fable 5 运行时）（2026-07-15）

**来源**：用户「帮我看一下这个 skill 有没有可以优化的点，确保跟 Fable 越接近越好」→ 12 条提案全部确认「do all changes」

## 目标（一句话）

修掉真 bug、消灭多副本漂移（手册瘦身 + symlink 根治同步）、补齐 Fable 5 运行时的自主推进行为与活水条款。

## 验收标准

- /fable-check 的命令可直接执行（无引号内 ~）且覆盖 commands 同步
- 手册细则段替换为指向 references 的指针，独有内容（角色/自诊断史/架构）保留
- ~/.claude/skills/fable-operating-system 与 4 个 command 文件为指向源目录的 symlink，通过 symlink 路径可读
- FABLE_CORE #6 双向（授权闸门 + 自主推进）+ 活水条款，三件套（FABLE_CORE / agents-md-section / ~/.codex/AGENTS.md）同步
- evolution.md 有 v1.4 CHANGELOG；记忆已更新；已 push GitHub

## 需求清单（append-only）

- [x] 1 fable-check：引号内 ~ 修为 $HOME；适配 symlink 检查
- [x] 2 fable-check + evolution 保鲜例行：补 commands 同步/链接检查
- [x] 3 audit-report 模板：x/21 → x/(计入维度×3)
- [x] 4 数字漂移修正（roadmap「19 类」→18）
- [x] 5 手册瘦身：细则段指针化，保留角色/原则/自诊断史/示例/架构；版本号改指向 evolution（原 §11+§12 合并为一张带「现行位置」列的表）
- [x] 6 symlink 替代 rsync（skills 目录 + 4 个 command 文件）+ 可读性验证 + 回退方案记录（evolution 保鲜例行）
- [x] 7 FABLE_CORE #6 双向 + planning §回合完整性 + communication 反模式 +1；三件套同步（含 ~/.codex/AGENTS.md 实装段）
- [x] 8 活水条款（FABLE_CORE §边界 + SKILL.md 目的节 + agents-md-section Living-standard note）
- [x] 9 quality-checks 补证据闸门（高风险操作闸门节首）
- [x] 10 不抄运行时机制（写入 FABLE_CORE 活水条款正文 + CHANGELOG）
- [x] 11 核实 6 个 lark skill 不可见原因（悬空 symlink，lark 套件更新下架）→ skill-tool-map 盘点 2026-07-15 + task-routing/task-workflows 联动清理
- [x] 12 README 版本 v1.4 + 手册描述更新 + 安装说明补 symlink 选项
- [x] 13 evolution CHANGELOG v1.4；.fable/task.md 回填；记忆同步；git push

## 决策日志

- 2026-07-15 用户确认 12 条全做（含结构两条：手册瘦身、symlink）。
- symlink 若 Claude Code 不跟随（下会话实测），回退：`rm` 链接后 `cp -R` 恢复 rsync 制（已写入 evolution 保鲜例行）。
- 悬空的 6 个 lark symlink：报告用户待清理，不擅删（最小范围契约）。
- 手册原 §12 八条改进规则与 §11 自诊断表内容重叠 → 合并为一张表，每条标注现行 reference 位置，不丢历史。

## 验证证据

- symlink 生效：readlink 5 个链接全部指向源目录；经链接路径 grep 到新增「活水条款」（SKILL.md、FABLE_CORE.md 各 1 处）；personal/ 经链接完好；可行性先例：全部 lark-* skill 一直以 symlink 形式被正常加载（本会话在用）
- 三件套一致：FABLE_CORE #6 / agents-md-section / ~/.codex/AGENTS.md 三处均含「可逆直接推进 + 回合完整性」与活水条款（Edit 逐处成功）
- 下架 skill 清理后 grep 复核：lark-mail 等 6 名仅剩「已下架」标注行与 CHANGELOG 记录，无活引用
- 手册瘦身：320 行 → 约 120 行导览层，「19 类」漂移点随细则段一并消除
- git push：commit d7283eb（16 files，+167/−288），cac958f..d7283eb 推送 origin/main 成功；泄漏检查通过（personal/ 在 ignored，git ls-files 含 personal 计数 0）
- symlink 运行时实证：改完 fable-check 描述后，本会话技能列表实时刷新为新描述「安装链接」——Claude Code 确认经 symlink 加载

---

# 任务 4：v1.5 抗膨胀整改 + 思考内核（2026-07-17）

**来源**：十维自审（77/100，`.fable/audits/2026-07-17-skill-longterm-risk-tenscore.md`）后用户确认「yes 优化 然后确保所有有调用这个 skill 的地方都更新了」

## 目标（一句话）

给进化回路装上「出口」（规则守恒 + 滚动归档 + 行数预算），把思考内核四原则成文进契约，并同步全部接入点。

## 需求清单（append-only）

- [x] 1 evolution.md 复盘四问守恒闸 + 行数预算入第 3 步
- [x] 2 evolution.md 保鲜例行「收缩审查」+ CHANGELOG 滚动规则
- [x] 3 CHANGELOG v1.5 条目；v1.0–v1.3 移 HISTORY.md（修复 v1.3 标题丢失）
- [x] 4 FABLE_CORE §思考内核（四原则）
- [x] 5 三件套同步：agents-md-section.md + ~/.codex/AGENTS.md 英文版
- [x] 6 SKILL.md 核心流程后一行指针 + References 行更新
- [x] 7 README：四问 / 结构树 + HISTORY / v1.3 漂移修正 / 版本 v1.5
- [x] 8 手册：四问 / 一核四件树 + HISTORY.md
- [x] 9 全仓「三问」引用统一 + 追加发现：fable-retro.md 残留已废弃的 rsync 步骤（v1.4 漏改），一并修复
- [x] 10 行数预算合规检查（references ≤150）
- [x] 11 记忆更新（fable-operating-system-skill.md + MEMORY.md → v1.5；顺带修正已过时的「悬空 symlink 待清理」记述）
- [x] 12 体检（symlink 经链接读新内容 + CLAUDE.md / AGENTS.md 段确认）
- [x] 13 git commit + push + 泄漏检查

## 决策日志

- 思考内核放 FABLE_CORE（跨 agent 可携带），SKILL.md 只留一行关键词指针——权衡：自用场景多一跳，换单一事实源不双写。
- 收缩审查只写保鲜例行一处，版本规则节不重复——守恒条款首次自我应用。
- 本次收缩审查结论：十条契约暂不退役任何条目——对 Claude 冗余的对 Codex 仍必要，契约层从严保留。
- 归档时发现 v1.3 标题行在 v1.4 编辑中丢失（条目被吞入 v1.4），随滚动归档修复。

## 验证证据

- 行数预算合规：`wc -l references/*.md` 全部 ≤150（最大 task-workflows 145；evolution 78→69 行，瘦身来自 CHANGELOG 滚动）
- symlink 经链接实读新内容：`$HOME/.claude/skills/.../SKILL.md` grep「思考内核」×2、FABLE_CORE ×1、HISTORY.md 可读；`$HOME/.claude/commands/fable-retro.md` 经链接 grep「四问」×1
- 三件套一致：FABLE_CORE §思考内核（中文）+ agents-md-section.md 与 ~/.codex/AGENTS.md 各含「Thinking core」bullet ×1（同文）
- 接入点：~/.claude/CLAUDE.md 含 fable-operating-system 指令 ✓；4 个 command symlink readlink 全部指向源目录
- 「三问」残留仅 evolution.md 两处合法行文（「前三问皆是才升级」+ CHANGELOG 历史描述）；ROADMAP 为历史评审快照，按最小范围不追改
- 泄漏检查：`git ls-files | grep -c personal` = 0
- 运行时实证：改 fable-retro.md 后本会话 skills 列表实时刷新（fable-operating-system 与 fable-retro 出现在增量通知中）——symlink 加载链路继续有效
- push：见 git log（commit 与推送结果记录在交付汇报）
