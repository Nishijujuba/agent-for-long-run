# 常见失败方式：按症状、根因和防法管理 Agent 风险

## 状态

confirmed

## 一句话主张

长程 agent 的失败大多不是偶发事故，而是可预见的系统性故障：目标漂移、上下文污染、循环失控、规划错误、权限过宽、验证缺失和交接腐烂。

## 适用角色

跨角色。适合作为最终行动建议页，也适合作为两页 PPT：第一页讲失败方式，第二页讲防法。

## 工作场景

团队开始把 agent 用于长程任务：让它读资料、写代码、生成报告、改流程、排障、做研究。早期 demo 看起来顺畅，进入真实工作后会出现：越做越偏、越修越乱、无限重试、成本飙升、产物难审、旧文档误导新 agent。

## 讲给 PPT agent 的内容说明

这一页应把失败方式讲成“症状 → 根因 → 防法”，避免抽象吓人。可以组织为 8 类：

1. 目标漂移：开工前没有任务契约，agent 在执行中不断扩张目标。防法是写清目标、非目标、验收和权限。
2. 上下文污染：所有历史、日志、旧计划、失败修补都堆进同一会话。防法是切小任务、clear、状态落盘、按需检索。
3. 规划当执行：agent 一边猜产品意图，一边改代码或写报告。防法是先 PRD，再 Kanban/DAG，再 AFK 执行。
4. 横向切片：先做所有数据库，再做所有 API，再做所有前端，集成反馈拖到最后。防法是垂直切片和 traceable bullet。
5. 无限循环：没有预算、错误上限、停止条件和失败回流。防法是 loop 加 timeout、cost budget、`no more tasks`、error-to-issue。
6. 工具误用和权限过宽：一上来给写权限、生产权限、删除权限。防法是 read-only first、sandbox、staging、最小权限、审计日志。
7. 伪验证：agent 说“已检查”，但没有测试、diff、日志、截图、人工 QA。防法是 verification + evaluation 双层机制。
8. 文档腐烂：旧 PRD、旧 issue、旧计划没有生命周期状态。防法是 active、closed、superseded、archived 状态标签。

## 实际例子

Cadence gamification 案例里，若 agent 没有人类在环，可能会把“积分系统”扩展成视频观看计分、排行榜、徽章、等级、通知、管理后台。视频观看事件看起来容易埋点，却噪声大且容易被刷；是否回填历史 lesson progress 又会牵涉公平感、迁移脚本、发布说明和测试范围。这说明目标扩张和隐藏决策必须在需求阶段拦住。

垂直切片案例里，模型最初建议先创建 gamification service，这会让数据库、服务、前端分层推进，反馈太晚。更稳的 ticket 是“lesson completion 触发积分，并在 dashboard 可见”。它很窄，却能暴露真实链路问题。

Ralph loop 案例里，prompt 明确只处理 AFK issues，完成后输出 `no more tasks`，并按优先级取下一项。这说明 agent loop 要有调度规则和停止信号。否则 agent 很容易在“继续优化”“继续修复”“再检查一下”中循环。

Doc rot 案例里，旧 gamification PRD 一个月后可能已经与真实代码分叉。若没有 closed、archived 或 superseded 状态，新 agent 会把旧计划当成当前事实。

## 应掌握的概念和方法论

- 任务契约：防目标漂移。
- Human-in-the-loop：防价值取舍外包。
- Smart Zone / Dumb Zone：防上下文过载。
- PRD / Kanban / DAG：防计划不可执行。
- Vertical slice / traceable bullet：防反馈过晚。
- Agent Loop + Stop condition：防无限循环。
- Harness / Hook / Permission ladder：防工具误用。
- Verification / Evaluation：防伪验证。
- Fresh context review：防实现上下文自我合理化。
- State file / doc lifecycle：防交接失败和文档腐烂。

## 常见失败方式

最危险的组合是：模糊目标 + 大上下文 + 高权限 + 无测试 + 无停止条件。这个组合会让 agent 以很高速度生产难以审查的偏差。长程任务中，流畅输出反而会掩盖偏差，因为人类更容易相信一份结构完整、语气自信的产物。

另一个高频组合是：多 agent 并行 + 无共享状态 + 无合并协议。多个 agent 会重复读取、重复修改、互相覆盖，最后父代理面对一堆摘要和冲突文件，协调成本超过节省的时间。

## 来源依据

- `resources/why-agentic-ai-fails-infinite-loops-planning-errors-and-more.pdf`: 支撑无限循环、规划错误、工具误用和停止条件等失败方式。
- `resources/missing-primitive-for-agent-swarms.pdf`: 支撑多 agent 协调、共享状态和合并协议的风险。
- `resources/solving-context-management-in-agents.pdf`: 支撑上下文污染、状态文件、检索和记忆边界。
- `resources/llm-nonsense-first-principles-breakdown-of-attention-and-ffn.pdf`: 支撑长上下文与注意力关系压力的底层解释。
- `resources/agent-frontline-practice-implementation-experience-and-know-how.pdf`: 支撑一线实践中的权限、验证、人工复核和落地边界。
- `resources/how-claude-code-works-in-large-codebases.pdf`: 支撑大代码库中的入口文档、任务拆分、工具导航和可审查 diff。
- `AGENTS.md`: 支撑本仓库对来源、idea、验收标准、角色场景和 agent 执行规则的要求。

## PPT 使用建议

适合做收束页或双页。第一页展示 8 类失败方式，第二页展示对应防法。讲法要直接：失败不是因为 agent “笨”，而是流程把它推到了无边界、无证据、无刹车的状态。最后给一句行动建议：选一个高频长程任务，先切成 1 个对齐段 + 3 个 AFK issue，并给每个 issue 配验证方式和停止条件。