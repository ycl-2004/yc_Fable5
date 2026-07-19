# yc_Fable5 —— 个人 Agent 操作系统（Fable OS）

给 AI agent 装一个「操作系统」：**工作规范 + 验证器 + 跨 agent 契约**。让 agent 的行为稳定、可预测、可审计——不管任务是代码、写作、研究还是设计。

这是一套真实在用的个人系统（作者：[@ycl-2004](https://github.com/ycl-2004)，运行在 Claude Code 上，治理范围含 Codex），公开出来作为「如何给自己的 agent 建操作系统」的完整参考实现。个人隐私层已剥离（见 [PRIVACY.md](PRIVACY.md)）。

## 它解决什么问题

- agent 每次任务的做法不一致，质量靠运气
- 说「完成了」但拿不出证据
- 中途追加的需求把原始需求挤掉
- 多个 agent（Claude Code、Codex、子代理）各干各的，没有统一的验收标准
- 犯过的错下次照犯，经验不沉淀

## 三种运行模式

| 模式 | 触发 | 做什么 |
|------|------|--------|
| **工作模式** | 复杂任务自动 / `/fable` | 理解→分类→规划→执行→交付五步；>5 步任务建 `.fable/task.md` 工作文件；交付带 3 行自检尾注 |
| **验证模式** | 「用 Fable 验证」/ `/fable-audit` | 7 维 rubric（理解/路由/范围/需求/证据/沟通/授权）逐项判定并引用证据，0-3 计分，结论三档，报告归档 `.fable/audits/` |
| **复盘模式** | 「Fable 复盘」/ `/fable-retro` | 失误标准记录 → 四问（含守恒闸：能并入已有规则就不新增）→ 升级或并入规则 → 记 CHANGELOG |

## 仓库结构

```text
fable-operating-system/     # 核心 Skill（装进 ~/.claude/skills/）
├── SKILL.md                # 三模式总入口
├── FABLE_CORE.md           # 十条可移植契约 + 思考内核四原则（注入任何 agent 的规范文件/委派 prompt）
├── references/ ×9          # 规则库：理解路由 / 规划 / 沟通 / 能力地图 / 质检 / 17 类工作流 / 审计 rubric / 跨 agent / 进化回路
├── templates/ ×3           # 任务文件 / 审计报告 / AGENTS.md 注入段
├── examples/ ×6            # 六类任务示范（含一次敢给自己 FAIL 的真实审计）
├── HISTORY.md              # 版本史归档（CHANGELOG 只留最近两版，更早在此）
└── personal/               # 私有层（.gitignore 排除，本仓库不含）
commands/                   # 四个 slash commands（装进 ~/.claude/commands/）
.fable/                     # 工件示例：任务文件 + 审计归档（真实历史，dogfooding）
FABLE_OPERATING_SYSTEM.md   # 手册（导览层；细则的单一事实源在 references/）
FABLE_REVIEW_AND_ROADMAP.md # 多维评分与优化路线
VALIDATION_TESTS.md         # 6 项验证（5 模拟 + 1 真实执行）
```

## 安装（Claude Code）

```bash
git clone https://github.com/ycl-2004/yc_Fable5.git
cd yc_Fable5
cp -R fable-operating-system ~/.claude/skills/
cp commands/*.md ~/.claude/commands/
```

进阶：如果打算持续修改这套系统，用 symlink 替代 cp——改仓库即生效，没有「忘记同步安装副本」问题（作者自 v1.4 采用此方式）：

```bash
ln -s "$(pwd)/fable-operating-system" ~/.claude/skills/fable-operating-system
for f in commands/*.md; do ln -s "$(pwd)/$f" ~/.claude/commands/"$(basename "$f")"; done
```

然后两个可选接入点：

1. **管住主 agent**：把加载指令写进 `~/.claude/CLAUDE.md`（示例见手册 §15.2）。
2. **管住其他 agent**：把 `fable-operating-system/templates/agents-md-section.md` 的内容粘到该 agent 的规范文件末尾（如 Codex 的 `~/.codex/AGENTS.md`）——注意模板末尾的 Scope note 声明了本契约不修改任何既有权限边界，别删。

按自己的环境改掉文件里的绝对路径与能力地图（`references/skill-tool-map.md` 是作者的技能盘点，请替换成你自己的）。

## 日常口令

| 你说 | 发生什么 |
|------|----------|
| （直接布置复杂任务） | 自动进工作模式 |
| `/fable <任务>` | 强制工作模式 |
| `/fable-audit <对象>` | 7 维审计报告 + 归档 |
| `/fable-retro` | 把刚才的失误变成永久规则 |
| `/fable-check` | 体检：接入点 / 副本同步 / 地图保鲜 |
| （简单问题） | 什么都不启用——防过度工程也是规则 |

## 设计原则（为什么长这样）

1. **入口契约 + 出口审计**：没有办法让另一个 agent「内在地」守规则，可靠的是注入契约（入口）+ 审计验收（出口），中间过程不做微观控制。
2. **证据优先**：「完成」只能出现在可观察证据之后；工作文件 `.fable/task.md` 既防上下文丢失，又是审计的第一证据源。
3. **进化回路（有出口）**：每次失误走复盘模式沉淀为规则；同时用规则守恒四问、版本收缩审查、CHANGELOG 与 task.md 滚动归档、reference 行数预算防止系统无限膨胀（见 `references/evolution.md`）。
4. **审己如审人**：验证模式对自己的产出与对其他 agent 一视同仁——仓库里归档的第一份审计就是对作者自己任务的 FAIL 判定。

## 版本与许可

当前 **v1.6**（2026-07-19）；最近两版见 `fable-operating-system/references/evolution.md`，更早版本史归档在 `fable-operating-system/HISTORY.md`。MIT License。
