# 08 状态文件与文档设计：让 Agent 读到当前事实

## 状态

confirmed

## 一句话主张

长程任务一定会暂停、换会话、换模型或换执行者；可靠交接依靠 `state.md`、`AGENTS.md`、模块文档、运行手册和生命周期状态标签。

## 适用角色

跨角色：多人协作、跨天任务、长代码修改、长报告生成、政策口径追踪、实验复盘和运维排障。

## 工作场景

一个 agent 白天完成了资料梳理和前半段代码修改，晚上换另一个 agent 接手。如果关键信息只在聊天记录里，下一棒会像失忆上岗；如果状态、决策、证据和下一步都写进文件，新 agent 可以从清楚起点继续。

## Agent 协作方式

人类负责定义仓库文档入口和状态规范；agent 每完成阶段就更新状态文件、引用证据、记录验证结果和下一步动作。新 agent 开始任务时先读 README、AGENTS、state file 和相关模块文档，再进入执行。

## 长程任务设计要点

- `state.md` 最少包含：Goal、Current Status、Decisions、Evidence、Verification、Next Best Action、Risks。
- `AGENTS.md` 做规则入口，像给新同事的入职手册。
- 模块文档写接口、测试、目录结构和常见陷阱。
- Runbook 写生产操作、回滚步骤、权限边界和告警处理。
- 历史文档需要状态标签：`active`、`superseded`、`archived`、`closed`。
- 文档生命周期本身就是上下文质量的一部分。

## 来源依据

- `resources/context-graphs-decision-traces-of-ai-agents.pdf`: 本次讨论中用于提炼状态文件、决策轨迹、跨会话恢复和文档生命周期的素材。
- `README.md`: 指向 `AGENTS.md`、`resources/`、`ideas/` 的项目入口约定。
- `AGENTS.md`: 明确 agent 开始任务时先读 README 和 AGENTS，并盘点 `resources/` 与 `ideas/`。

## 风险与边界

陈旧文档会把旧信息伪装成当前事实。长程任务中，过期 PRD、旧 issue、旧计划和真实系统逐渐分叉后，对 agent 的误导性很强。解决方式是给文档状态、归档位置和更新时间，而不是让历史文件继续混在主路径里。

## PPT 使用建议

**页面标题**：交接与代码仓：让 Agent 读到当前事实

**页面主文案**：

- `state.md`：目标、已确认决定、开放问题、证据、测试结果、下一步。
- `clear`：回干净状态。
- `compact`：临时续航。
- 新会话 / 新模型：读文件继续。
- 面向 agent 的仓库文档：`AGENTS.md`、层级索引、模块文档、Runbook。
- 状态标签：active、superseded、archived、closed。

**讲法**：长程任务的交接像值班换班。口头交代会丢信息，标准交接表能保留当前事实。Agent 读取文件的能力越强，文件状态越要清楚。

## 待确认问题

- 是否需要在仓库中新增一个 `state-template.md` 或 `docs/agent-index.md`，供后续任务复用？
