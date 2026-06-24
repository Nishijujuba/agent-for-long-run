# PRD、Backlog 和 DAG：把大目标拆成 agent 能安全领取的小任务

## 状态

confirmed

## 一句话主张

长程任务要先把“完成后的样子”写成 PRD，再把抵达路径拆成有依赖、有验收、有优先级的 backlog 或 DAG，优先用垂直切片获得早反馈。

## 适用角色

程序员、数据分析师、平台运营工程师，也适合任何要交付多阶段成果的知识工作者。

## 工作场景

用户希望 agent 完成一个复杂目标：实现一个功能、整理一套政策口径、生成一份跨资料 PPT、做一次数据分析。若只给一个大目标，agent 会同时猜需求、找资料、写计划、执行和自检，任务越长偏差越大。更好的做法是先定义 destination，再拆 journey。

## 讲给 PPT agent 的内容说明

这一页要把 PRD 和计划分开讲。PRD 说“终点是什么”：用户故事、范围、非目标、验收标准、质量门槛。Backlog 或 DAG 说“怎么走”：哪些任务先做、哪些被阻塞、哪些可以并行、每个任务如何验收。

可以用一个简单对比讲清楚：横向拆分是“先做所有数据库，再做所有 API，再做所有页面”，看起来有条理，但很晚才获得端到端反馈；垂直切片是“先做一条最窄的用户可见路径”，例如一个行为触发、穿过数据和服务、最后在界面或报告中可观察。agent 更适合做后者，因为每个小任务都有具体证据。

## 应掌握的概念和方法论

- PRD / destination document：描述完成后的形状，而不是执行步骤。
- Backlog / journey document：描述任务拆分、依赖、优先级、阻塞和回流。
- DAG：任务之间的有向依赖图，让 agent 知道哪些任务已解锁。
- Vertical slice：穿过必要层次的窄功能片或窄工作片，能尽早获得端到端反馈。
- Independently grabbable issue：后续 agent 不需要理解整个世界，也能安全领取和完成。
- Definition of Done：每个任务必须有可检查的完成标准。

## 常见失败方式

第一种失败是只写漂亮的 phase list。阶段清单容易给人确定感，但它往往没有真实反馈点，前一阶段的误解会被后一阶段继续放大。

第二种失败是 ticket 太大。一个 ticket 如果需要读十几个模块、改很多文件、没有中间验收，它就不是 agent 友好的工作项。

第三种失败是 backlog 不可调度。没有 blocked-by、优先级、验收方式和回滚边界，agent 只能凭感觉拿任务，长程执行会变得随机。

## 来源依据

- `resources/real-engineers-ai-agent-coding-workflow.pdf`: 支撑从需求澄清到 PRD、Kanban/DAG、AFK issues、垂直切片的工作流。
- `resources/build-agents-that-run-for-hours-without-losing-the-plot.pdf`: 支撑长时间 agent 需要可调度的任务池、检查点和进度恢复。
- `resources/software-engineering-fundamentals-in-the-ai-era.pdf`: 支撑需求拆分、模块边界、可测试单元和小步交付仍是 AI 时代的基础工程能力。
- `AGENTS.md`: 要求每个 idea 对应具体场景、检查点和验收标准。

## PPT 使用建议

适合做“任务设计方法”页。画面可以左侧画 PRD 作为终点，右侧画 DAG 或 Kanban，中间突出一条 vertical slice，从用户行为或业务问题一路通到可观察结果。
