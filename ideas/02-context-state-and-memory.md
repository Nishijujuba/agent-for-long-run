# 上下文、状态和记忆：长程 Agent 要靠文件续航，不靠聊天历史硬撑

## 状态

confirmed

## 一句话主张

长程任务一定会跨步骤、跨会话、跨工具甚至跨 agent；可靠续航靠状态文件、检查点和可检索资料，而不是把越来越长的聊天历史塞进上下文。

## 适用角色

跨角色，尤其适合需要跨天完成报告、代码改造、数据分析、运营排障或资料综述的人。

## 工作场景

一个 agent 先读 PDF，第二个 agent 继续整理 ideas，第三个 agent 再生成 PPT。或者一个 coding agent 上午澄清需求，下午修改代码，晚上由另一个上下文审查 diff。只要任务足够长，聊天历史就会变成不可靠的草稿纸：信息会被压缩、遗漏、过期或混进错误假设。

## 讲给 PPT agent 的内容说明

这一页要解释“长上下文不等于长期记忆”。上下文窗口像工作台，能放很多材料，但工作台越乱，越难判断当前最重要的事实是什么。真正的长期协作要把关键状态外化：写入 repo、issue、state file、test output、diff、decision log 和 `ideas/` 文档。

可以把记忆分成四层讲：第一层是当前会话里的工作记忆，只适合短期推理；第二层是状态文件，保存下一步必须知道的最小事实；第三层是项目规则，例如 README、AGENTS、runbook；第四层是原始资料和历史记录，需要时再检索，而不是全部塞进 prompt。

## 应掌握的概念和方法论

- Context window：模型一次能看到的信息范围，不是可靠数据库。
- Clear vs compact：clear 让任务从干净状态重新开始；compact 只适合临时续航，不能替代证据。
- State file：记录目标、当前进度、已确认决策、开放问题、证据、风险和下一步。
- Checkpoint：每到一个阶段，把中间结果落盘并验收。
- Handoff packet：交接给下一个 agent 的短包，包含事实、证据、未决事项和禁止重复踩坑。
- Document lifecycle：文档应有 active、superseded、archived、closed 等状态，避免旧文档伪装成当前事实。

## 常见失败方式

第一种失败是“上下文堆料”。用户把 PDF、聊天记录、旧计划、日志和代码全部塞给 agent，以为模型会自动综合。结果往往是目标漂移、证据丢失、旧结论覆盖新事实。

第二种失败是“摘要崇拜”。compact 后的摘要看起来更清楚，但它可能保留结论、丢掉反例；保留计划、丢掉证据；甚至把当时的猜测写成确定事实。

第三种失败是“状态文件写成流水账”。好的状态文件不是复述所有过程，而是让下一轮 agent 能从干净上下文恢复任务。

## 来源依据

- `resources/build-agents-that-run-for-hours-without-losing-the-plot.pdf`: 支撑长时间运行的 agent 需要可恢复状态、检查点和不丢主线的机制。
- `resources/real-engineers-ai-agent-coding-workflow.pdf`: 支撑 fresh context、clear/compact、doc rot 和交接审查。
- `resources/software-engineering-fundamentals-in-the-ai-era.pdf`: 支撑把知识沉淀到文档、模块边界、测试和可维护结构中。
- `README.md`: 指定 `resources/` 与 `ideas/` 是本仓库的材料和共识输出位置。
- `AGENTS.md`: 明确单次聊天记录只作为过程材料，关键 ideas 要写入文件。

## PPT 使用建议

适合做“为什么长程任务会丢主线”的解释页。画面可以用一张工作台：左边是干净的 state file 和少量证据，右边是堆满聊天历史和旧材料的混乱桌面。
