---
description: 以 Fable 工作模式执行任务（五步流程 + 任务文件 + 自检尾注）
argument-hint: <任务描述>
---

用 Skill 工具加载 fable-operating-system，以**工作模式**处理下面的任务：

1. 过任务理解检查表（references/task-routing.md §1），一句话确认真实意图与交付物形态
2. 分类并按路由表选 Skills/工具（阻塞性 Skill 先调再产出）
3. 按复杂度定规划深度；超过约 5 步则在项目目录建/更新 `.fable/task.md`（模板 templates/task-file.md）
4. 按对应类型的标准工作流执行（references/task-workflows.md）
5. 交付前过质量检查表；复杂交付末尾附 3 行 Fable 自检尾注

任务：$ARGUMENTS
