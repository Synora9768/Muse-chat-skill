# 对 Claude 三问的决策（2026-10-06，Meta-Bot 整理，主人拍板）

## 问题 1：有没有"通用版" model-loop-prompt？
**决策：没有，删掉悬空引用。**
- 现状：只有 Meta-Bot 专属实例（写死 Meta-Bot/bc.py/--bot Meta-Bot）
- diaomao 用 Python wspoll，gemini 用原生 WS，三者架构不同，无法共用一份
- v25 已删掉"见通用版说明"，改为"其他 bot 另行参照各自实例化 prompt"
- 若将来要做通用版，另起任务设计，不在本轮解决

## 问题 2：pending 确认机制（核心分歧点）
**决策：废除显式确认环节，不用 pending_confirm.json。**
理由：
1. 两段式建账的第②步"群里播报 *ID/工期/负责人"本身就是否决窗口——
   广播发出后，群里任何人有异议可直接提出，无需再问一遍"确认执行？"
2. 若建完单还要等一句"确认"才开工，自主智能体退化成人工审批流，
   违背"7x24 无人值守"的设计初衷（gemini/diaomao 一致意见）
3. 避免跨轮状态追踪包袱（pending_confirm.json 的读写/匹配/清理全是复杂度）

**最终规则（一句话）：**
> assignee=自己 且 status=pending → 建账广播后直接拉起子 Agent 开工，
> 不做"已确认/未确认"二分，不维护 pending_confirm.json。

## 问题 3：pending→in_progress 谁来切？
**决策：只由子 Agent 在「阶段 1：开工认领」时切，主 Agent 永不碰。**
- 主 Agent 职责：建账广播 → 拉起子 Agent（仅拉起）
- 子 Agent 职责：阶段 1 接单时 CLI 切 in_progress + 写回 agent-id
- 杜绝"状态已 in_progress 但子 Agent 未启动"的假态（单责任人原则）
