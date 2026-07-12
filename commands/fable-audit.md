---
description: 以 Fable 验证模式审计一份产出（7 维 rubric + 计分 + 归档）
argument-hint: <审计对象：某次任务 / 文件 / diff / 另一个 agent 的产出>
---

用 Skill 工具加载 fable-operating-system，以**验证模式**审计下面的对象：

1. 读 references/validation-rubric.md
2. 收集输入材料：任务原文、交付物本体、`.fable/task.md`（若有）、过程记录；材料缺失要在报告中声明
3. 按 A-G 七维逐项判 PASS/FAIL/N/A，**每个判定引用具体证据**（文件路径、原话、输出片段）
4. 按 templates/audit-report.md 出完整报告，含每维 0-3 计分与总分百分比
5. 结论三档（通过 / 有条件通过 / 不通过）；FAIL 项给可执行整改动作
6. 报告归档到项目的 `.fable/audits/YYYY-MM-DD-<对象简名>.md`

审计对象：$ARGUMENTS
