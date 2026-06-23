# PRD 是目的地，Kanban/DAG 是路线：用垂直切片换早反馈

## 状态

confirmed

## 一句话主张

长程任务不能停留在漂亮的线性计划；要把目标压缩成 PRD，再拆成可抓取、可阻塞、可并行、可验收的 ticket，并优先使用垂直切片获得早反馈。

## 适用角色

程序员、平台运营工程师、数据分析师，也适合任何需要多阶段交付的知识工作者。

## 工作场景

团队已经通过追问确认了目标：例如要做一个 gamification 功能，或要完成一份跨资料的报告。下一步不是让 agent “按阶段完成所有内容”，而是把目标拆成能被 agent 独立拿走的小工作项，每个工作项都有验收标准、依赖关系和回滚边界。

## 讲给 PPT agent 的内容说明

这一页可以用 R → D → P → I 的主线解释：

- R：Requirements，需求澄清和关键决策。
- D：Destination，PRD 描述完成后的形状，包括用户故事、Definition of Done、范围外事项、测试决策。
- P：Plan，Kanban 或 DAG 描述抵达路径，把任务拆成可抓取 ticket。
- I：Implementation，目标和边界清楚后才进入 agent 执行。

PRD 像导航软件里的终点地址；Kanban/DAG 像路线图和施工排期。两者混在一起时，agent 会一边猜目的地，一边铺路，偏差会越来越大。

## 实际例子

课程笔记里的 gamification 案例说明了 horizontal slice 的问题。模型最初倾向于先“create the gamification service first”，这很整齐，却太横向：先做数据库或服务，用户看不到真实行为，集成反馈来得很晚。

更好的第一张 ticket 是“award points for lesson completion visible on dashboard”。它从用户完成 lesson 出发，穿过 schema changes、service、新逻辑和 dashboard 最小展示，让团队能尽早看到端到端结果。它只是一条很窄的路径，却能暴露数据、后端、前端和体验问题。

## 应掌握的概念和方法论

- Destination document：说明完成后的产品或工作结果是什么。
- Journey document：说明如何拆分、排序、并行和回流。
- Vertical slice：一个 ticket 穿过必要层次，形成可运行、可观察、可验收的薄功能片。
- Traceable bullet：生产路径上的窄功能片，强调可累积、可反馈。
- Blocking relationship：ticket 之间的阻塞关系，决定 agent 什么时候能拿走任务。
- Independently grabbable issue：后续 agent 不需要理解整个世界，也能安全执行的任务。

## 常见失败方式

第一种失败是线性 phase list：第一阶段做所有数据库，第二阶段做所有 API，第三阶段做所有前端。每一层看起来“完成了一部分”，实际没有端到端反馈，前面的误解会被后续继续放大。

第二种失败是 backlog 不可调度：issue 里没有 `blocked by`、验收方式、任务类型、回滚边界，agent 不知道下一步拿哪项，也不知道何时停止。

第三种失败是 ticket 太大。一个 ticket 如果需要理解十几个模块、改很多文件、没有中间验收，它就不是 agent 友好的工作项。

## 来源依据

- `resources/how-claude-code-works-in-large-codebases.pdf`: 支撑大代码库任务需要 repo-aware planning 和可抓取 work item。
- `resources/agent-frontline-practice-implementation-experience-and-know-how.pdf`: 支撑从实践经验中拆出可执行任务的做法。
- `resources/real-engineers-ai-agent-coding-workflow.pdf`: R → D → P → I、PRD、Kanban/DAG、traceable bullets、vertical slice、gamification service 例子。
- `AGENTS.md`: 要求 ideas 和 PPT 内容落到具体任务、检查点和验收标准。

## PPT 使用建议

适合做“任务图”页。画面可以左边是 PRD，右边是 Kanban/DAG：Ticket A 无阻塞，Ticket B 需要人工确认，Ticket C blocked by A。中间突出一条垂直切片，从用户行为穿过数据、服务、界面到可见结果。