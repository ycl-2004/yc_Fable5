---
description: 以 Fable 复盘模式把失误变成规则（失误记录 → 规则 → CHANGELOG）
argument-hint: <要复盘的失误/事件；留空则复盘本会话最近一次失误或被纠正的点>
---

用 Skill 工具加载 fable-operating-system，以**复盘模式**处理：

1. 按 references/evolution.md 的标准格式记录失误：表现 / 场景 / 原因（诚实归因）/ 影响 / 处置
2. 三问判断是否升级为规则：会重复发生吗？跨任务类型普适吗？能写成「当…时，我应该…，因为…」吗
3. 升级则把规则写进它所属的 reference 文件，并在 evolution.md CHANGELOG 记一行
4. 跨会话有价值的写入持久记忆
5. 同步 `~/.claude/skills/fable-operating-system/` 安装副本（rsync 源目录过去）

复盘对象：$ARGUMENTS
