# 记忆和交接要落在文件里：聊天历史只能当草稿纸

## 状态

confirmed

## 一句话主张

长程任务一定会暂停、换会话、换模型、换执行者；可靠交接不靠聊天历史，而靠状态文件、仓库入口文档和有生命周期的项目资料。

## 适用角色

跨角色，尤其适合需要跨天、跨人、跨工具完成的复杂任务。

## 工作场景

一个 agent 今天读完资料、生成候选 idea，明天另一个 agent 要继续生成 PPT。若关键判断只留在聊天记录里，下一轮 agent 很可能看不到、看不全，或读到过期摘要。若关键判断进入 `ideas/`、`state.md`、PRD、issue、测试和代码，后续就能从干净状态继续。

## 讲给 PPT agent 的内容说明

这一页要讲“记忆不是把所有历史都带着走”。对长程任务来说，真正可靠的记忆应分层：

- 工作记忆：当前会话里正在处理的信息。
- 状态文件：当前任务最小必要事实，给下一轮接手。
- 长期记忆：稳定规则、模板、项目约定。
- 检索记忆：需要时再取的原始资料、历史 issue、日志和文档。

可以用 Memento 类比：每次清空上下文后，agent 会回到 base state。坏处是临时线索会丢；好处是只要关键事实写进文件，下一轮 agent 能从干净状态重新开始，减少历史噪声。

## 实际例子

本仓库的 `AGENTS.md` 就是面向 agent 的入口文档。它告诉后续 agent 先读 README 和 AGENTS，盘点 `resources/` 与 `ideas/`，每个 idea 一个 markdown 文件，不得编造 PDF 内容，无法确认时标为推断或待确认。

课程笔记中的 doc rot 例子也很关键：一个 gamification PRD 最初可能准确，但一个月后代码结构、命名、用户反馈和需求都变了。如果旧 PRD 还躺在 repo 里且没有状态标记，Claude 或其他 agent 可能把它当成当前权威文档。文档越像权威，腐烂后越危险。

## 应掌握的概念和方法论

- State file：记录目标、已确认决策、开放问题、证据、测试结果、下一步、风险。
- AGENTS.md / CLAUDE.md：面向 agent 的规则入口，不应塞满所有细节，而应索引到更深文档。
- Clear vs compact：clear 回到干净状态；compact 只作短期续航，不能替代文件化记忆。
- Document lifecycle：active、superseded、archived、closed，给文档加状态信号。
- Handoff packet：交接包要短、准、可验证，不能只写“已经差不多完成”。

## 常见失败方式

第一种失败是“聊天历史依赖”。人类以为 agent 记得前面所有讨论，实际上换会话、清空上下文、压缩历史都会丢信息。

第二种失败是“文档腐烂”。旧 PRD、旧计划、旧 issue 没有状态标记，后续 agent 误把历史材料当当前事实。

第三种失败是“状态文件写成流水账”。好的状态文件不是记录所有过程，而是记录下一轮继续任务必须知道的最小事实和证据。

## 来源依据

- `resources/solving-context-management-in-agents.pdf`: 支撑状态、记忆、上下文压缩、检索和交接机制。
- `resources/how-claude-code-works-in-large-codebases.pdf`: 支撑仓库入口文档、项目规则和代码导航资料对 agent 的作用。
- `resources/real-engineers-ai-agent-coding-workflow.pdf`: Memento、clear、compact、fresh context、doc rot、GitHub issue closed 状态信号。
- `AGENTS.md`: 明确目录约定、idea 模板、来源标注、不编造原材料、执行规则。

## PPT 使用建议

适合做“状态与文档设计”页。画面可以是一条交接链：聊天草稿 → state.md → ideas/ → PRD/issue → PPT agent。旁边用红色提醒：陈旧文档没有状态信号时，会把旧信息伪装成当前事实。