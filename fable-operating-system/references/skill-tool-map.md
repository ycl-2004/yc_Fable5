# Skills 与工具能力地图

盘点日期：2026-07-15。来源：`~/.claude/skills`（42 个）+ 插件/内建技能 + 核心工具 + MCP 服务器。新装/移除 Skill 后本文件需要更新。

> 2026-07-15 盘点变更：lark-mail、lark-approval、lark-okr、lark-attendance、lark-apps、lark-skill-maker 六个 skill 已随 lark 套件更新下架（残留的悬空 symlink 已于同日经用户确认清理）。这些领域的需求改走 lark-openapi-explorer 原生 API（邮件另有 Gmail MCP）。

## 一、核心工具（无需路由）

| 工具 | 用途 | 纪律 |
|------|------|------|
| Read / Write / Edit | 文件读写改 | 改前必读；Edit 的 old_string 必须唯一；不用 cat/sed 替代 |
| Bash | 命令、git、测试 | 有专用工具时不裸用 shell；后台任务用 run_in_background |
| WebSearch / WebFetch | 时效信息、抓网页 | 知识截止 2026-01 之后的事必搜；结果要交叉验证 |
| AskUserQuestion | 用户裁决 | 只用于我无法推导的决定；给 2-4 个具体选项 |
| Agent（子代理） | Explore 搜索 / Plan 规划 / general-purpose / fork / codex-rescue | 用户不要求不主动 spawn；fork 继承上下文 |
| Artifact | 发布私有网页 | 写页面前必读 artifact-design；自包含（无外部资源）；主题双适配 |
| Skill | 调用技能 | 只调存在的名字；命中即阻塞 |
| ToolSearch | 加载延迟工具 | 一次批量取回全部所需 schema，不逐个取 |
| TaskCreate/TaskList 等 | 任务追踪 | 多步任务可视化进度 |
| CronCreate / ScheduleWakeup | 定时/自唤醒 | 配合 schedule / loop Skill 用 |
| 记忆系统 | `~/.claude/projects/.../memory/` | 一事一文件 + MEMORY.md 索引；不存 repo 已记录的事 |

## 二、飞书生态（21 个 lark-*）

**总路由规则**：按资源 URL 路径模式和 token 类型路由，不看域名（doubao.com 的 /docx/ /sheets/ /wiki/ 也走 lark-*）。认证问题一律先 lark-shared。CLI 没覆盖的原生 API 走 lark-openapi-explorer。

| 资源/动作 | Skill | 边界备注 |
|-----------|-------|----------|
| 文档 /docx/ /wiki/ 内容读写 | lark-doc | 文档里嵌的表格/Base/画板先取 token 再切对应 Skill |
| 电子表格 /sheets/ | lark-sheets | 按名搜表格文件走 lark-drive |
| 多维表格 /base/ | lark-base | 文件导入转 lark-drive |
| 云盘文件/上传下载/导入 | lark-drive | Markdown 原生读写走 lark-markdown |
| Markdown 文件 | lark-markdown | 导入为在线文档走 lark-drive |
| 知识库空间/节点 | lark-wiki | 编辑内容切 lark-doc 等 |
| 消息/群/卡片 | lark-im | 发送前确认 |
| 邮件 | Gmail MCP（见七） | lark-mail 已下架；飞书邮箱需求走 lark-openapi-explorer |
| 日历/日程/会议室 | lark-calendar | 历史会议走 lark-vc |
| 任务/清单/任务智能体 | lark-task | 审批类（lark-approval 已下架）走 lark-openapi-explorer |
| 历史会议/纪要/逐字稿 | lark-vc | 实时入会走 lark-vc-agent |
| 会中实时 | lark-vc-agent | 已结束会议走 lark-vc |
| 妙记/音视频转写 | lark-minutes | 本地音视频转纪要优先走它，不用 whisper |
| 会议纪要 note_id 直查 | lark-note | 仅已知 note_id 时 |
| 通讯录解析 open_id | lark-contact | 部门树遍历走 openapi-explorer |
| 幻灯片 /slides/ | lark-slides | 内嵌画板属本 Skill；独立画板走 lark-whiteboard |
| 画板 | lark-whiteboard | — |
| 实时事件订阅 | lark-event | bot、流式消费 |
| 会议纪要周报工作流 | lark-workflow-meeting-summary | — |
| 日程待办摘要工作流 | lark-workflow-standup-report | — |
| 认证/授权/scope | lark-shared | 所有 lark-cli 认证问题的入口 |
| 原生 API 挖掘 | lark-openapi-explorer | 现有 Skill 都不满足时 |

## 三、写作与内容 IP

| Skill | 用途 | 不适合 |
|-------|------|--------|
| carl-writer | 公众号/长文/技术叙事，卡尔+卡兹克风格，默认用户本人身份 | 视频脚本（走 carl-script-writer） |
| carl-script-writer | 短视频/口播/B站脚本，继承实测感与口语节奏 | 图文长文 |
| always | 管理 ~/.always/sentences.json 的常用 prompt 片段 | 一般写作 |

## 四、视觉与设计

| Skill | 用途 | 关键规则 |
|-------|------|----------|
| yc-ip | YC 风格中文 IP 配图（红发 chibi、白底手绘） | 必须真生成图，不能停在方案；默认 Standard 风格 |
| ian-xiaohei-illustrations | 小黑 IP 中文正文配图（白底手绘+红橙蓝批注） | 与 yc-ip 按 IP 形象区分 |
| yc-design | 个人品牌前端交付：教程页、Landing、卡片、16:9 Slides | 个人主页不归它 |
| ui-ux-pro-max | 通用 UI/UX 知识库：风格、配色、字体、UX 准则 | 与 yc-design 冲突时看是否个人品牌场景 |
| dataviz | **画任何图表前必读**（阻塞性） | 所有媒介的 chart/plot/dashboard |
| artifact-design | 发布 Artifact 前必读（阻塞性） | — |
| grid-split | 宫格图/拼图 sheet 按 R×C 切分 | 依赖 Pillow |
| pencil MCP | .pen 设计文件读写 | .pen 只能用 pencil 工具碰，禁 Read/Grep |
| Canva MCP | Canva 认证接入 | — |

## 五、工程方法（多为阻塞性触发）

| Skill | 触发时机 | 性质 |
|-------|----------|------|
| brainstorming | **任何创意工作之前**：新功能、新组件、行为修改 | 阻塞性，最易漏调 |
| test-driven-development | 实现 feature/bugfix 之前 | 阻塞性 |
| verify | 提交非平凡改动前，端到端驱动真实流程 | 内建 |
| run | 启动/驱动项目应用看效果 | 内建 |
| code-review / review / security-review / simplify | 审查 diff / PR / 安全 / 简化 | 内建 |
| init | 初始化 CLAUDE.md | 内建 |
| idea-king | 第一性原理推演 + 对抗式审查方案 | 方案压力测试 |
| partner-skill | Claude Code + Codex 双向分工 | 说"搭子"/"让 codex 做"时 |
| codex:rescue / codex:setup 等 | 卡住时二次诊断、委派 Codex | — |

## 六、Agent 工程知识库（参考型，非执行型）

设计 LLM 系统时按主题查：project-development（项目形态/管线）、tool-design（工具接口层）、evaluation（评估体系）、context-compression / context-degradation / filesystem-context（上下文工程）、harness-engineering（自治 harness）、share-memory（Claude+Codex 项目共享记忆，说"更新记忆"时触发）。

## 七、平台与自动化

| Skill/工具 | 用途 | 关键规则 |
|-----------|------|----------|
| update-config | hooks/权限/env——"以后每次 X"类需求 | 自动行为必须落 settings.json，记忆做不到 |
| schedule | 定时云 agent（cron） | 一次性定时也算 |
| loop | 会话内循环/轮询 | 一次性任务不用 |
| keybindings-help | 快捷键定制 | — |
| fewer-permission-prompts | 生成权限白名单 | — |
| claude-api | Claude API/模型/定价咨询——**提到 Claude/Anthropic/LLM 选型时先读，不凭记忆答** | 阻塞性 |
| claude-in-chrome | 浏览器自动化 | 先 tabs_context；不复用旧 tab ID；防触发 alert |
| notebooklm MCP | NotebookLM 笔记本/研究/音视频产物 | 认证问题跑 `nlm login` |
| Gmail MCP | Gmail 搜索/草稿/标签 | 发送类动作先确认 |

## 八、组合场景速查

- 调研 + 写文 + 配图：研究流程 → carl-writer → yc-ip / ian-xiaohei-illustrations
- 写代码全周期：brainstorming → TDD → 实现 → verify → code-review
- 数据分析 + 汇报：Bash(python) 或 lark-sheets → dataviz → Artifact 或 lark-doc
- 飞书内容生产：lark-doc 写 → lark-im 发 → lark-task 跟进
- 方案设计：idea-king 对抗审查 → partner-skill 分工执行
