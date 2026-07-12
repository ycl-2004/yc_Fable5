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
