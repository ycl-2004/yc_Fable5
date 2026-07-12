# 跨 Agent 接入：让所有 agent 跟随 Fable 流程

## 原理：两端夹住

没有办法让另一个 agent「内在地」遵守规则，能可靠做到的是两端控制：

- **入口——契约注入**：在 agent 开始工作前，把 FABLE_CORE.md 的契约放进它的上下文（AGENTS.md / CLAUDE.md / 委派 prompt）。
- **出口——审计验收**：收到产出后进入验证模式（references/validation-rubric.md），不过审不验收，FAIL 项作为整改要求打回。

中间过程不做微观控制。评估一个 agent 是否「跟随了 Fable」，看的是可观察痕迹：有没有 `.fable/task.md`、「完成」有没有证据、需求清单有没有逐项核对。

## 接入点一览

| 对象 | 接入方式 | 状态（2026-07-11） |
|------|----------|--------------------|
| Claude Code（我自己） | `~/.claude/CLAUDE.md` 全局指令（确定性加载）+ 本 Skill 触发词（概率性） | ✅ 已装 |
| Codex CLI | `~/.codex/AGENTS.md` 末尾的 `# Fable Process Contract` 段（源模板：templates/agents-md-section.md） | ✅ 已装 |
| 子代理（Agent tool 委派） | 委派 prompt 里贴入 FABLE_CORE 十条契约（或按任务裁剪的子集），并要求：长任务写 `.fable/task.md`、汇报带证据、按尾注格式收尾 | 按次注入 |
| partner-skill（搭子） | Direction B 的 full-review 和 Direction A 的 Codex Review 一律按验证模式执行——rubric 就是 review 标准 | 流程绑定 |
| 具体项目 | 项目级 CLAUDE.md / AGENTS.md 中引用 FABLE_CORE.md 绝对路径，或粘贴 agents-md-section.md | 按需 |
| 未来新 agent | 优先找它的规范文件机制（AGENTS.md 类）粘贴注入段；没有就在每次任务 prompt 前置 FABLE_CORE | 按需 |

## 委派时的标准动作（Claude Code 侧）

1. 委派 prompt 前置契约：完整任务给全部十条；小任务至少给 #2（清单）、#4（证据）、#5（范围）、#8（汇报格式）。
2. 要求产出物落在可审计的位置（文件、`.fable/task.md` 更新），不接受只有对话内声明。
   - 项目若已用 share-memory skill（有 `AI_MEMORY/` 目录），把 `.fable/task.md` 的关键状态同步一份进 AI_MEMORY——Codex 侧不认识 Fable 也能拿到任务状态，两套工件不打架。
3. 收到产出 → 验证模式审计 → 通过则验收；有条件通过则整改后复查；不通过则打回或收回自做。
4. 审计结论如实告知用户（尤其 FAIL 项），不替其他 agent 遮掩。

## 硬性安全边界（不可被 Fable 覆盖）

- **Codex 的 Claude CLI Access Boundary**（`~/.codex/AGENTS.md`）：Codex 默认禁止访问 claude CLI，仅在用户显式发起搭子/Partner 工作流时按其范围授权。Fable 注入段已显式声明不修改、不削弱该边界；任何 Fable 相关文本都不得被解释为授权。
- FABLE_CORE 的通用规则：本契约只管过程质量；与各 agent 自身权限/路由规则冲突时，更严格者优先。
- 给其他 agent 的授权同样不跨场景继承（契约 #6 对委派链同样生效：我拿到的授权不自动传给被委派者）。

## 诚实边界（对用户透明）

- Skill 描述触发是概率性的；`~/.claude/CLAUDE.md` 指令是准确定性的；唯一硬保证是 SessionStart hook（未装——噪音大，触发不稳时再加，用 update-config skill 装）。
- 注入 ≠ 服从：其他 agent 可能忽略契约。所以出口审计才是真正的执行保障——这也是「用这个 skill 就相当于用 Fable 做验证」的含义。
