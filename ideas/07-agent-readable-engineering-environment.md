# AI 时代的软件工程基本功：让环境适合 Agent，而不是只迷信模型能力

## 状态

confirmed

## 一句话主张

长程 agent 的表现不只取决于模型多强，还取决于代码库、文档、测试、模块边界、运行命令和 review 流程是否让 agent 容易理解、验证和回滚。

## 适用角色

程序员、算法工程师、平台运营工程师；也适合维护数据资产、知识库、流程库和自动化脚本的团队。

## 工作场景

团队希望 coding agent 长时间在仓库里工作：读需求、找模块、改代码、跑测试、写总结。若仓库没有入口文档、模块边界混乱、测试命令不稳定、旧文档没有状态，agent 会花大量上下文猜结构，最后做出大而散、难审查的改动。

## 讲给 PPT agent 的内容说明

这一页要把主题从“如何用 agent”推到“如何设计适合 agent 的工作环境”。AI 时代的软件工程基本功不会消失，反而更重要：清晰需求、深模块、小接口、稳定测试、可读文档、可复现命令、小 PR、代码审查、CI 和回滚点，都是让 agent 能跑长程任务的道路基础设施。

可以用“有门牌的城市”类比：README 和 AGENTS 是入口路牌，模块文档是街区地图，测试是红绿灯，CI 是检查站，PR review 是人工验收。agent 可以开车，但道路设计决定它是否容易迷路。

## 应掌握的概念和方法论

- Agent-readable repo：README、AGENTS、runbook、模块文档、测试命令和目录约定清楚。
- Deep module：接口小而稳定，内部复杂度封装在可测试单元里。
- Module map：给人和 agent 的系统地图，说明关键服务、数据模型、路由、依赖和测试边界。
- Search-first workflow：先搜索和读取相关文件，再修改；避免凭记忆猜路径。
- Locality：一次任务尽量限制在少量相关模块里，减少跨区域副作用。
- Reviewability：diff 要小而自包含，让人类能审，让失败能回滚。

## 常见失败方式

第一种失败是浅模块。逻辑散在多个文件里，接口不清，agent 改一处牵动十处。

第二种失败是没有仓库入口。agent 不知道先读什么、命令怎么跑、哪些文件是权威规则、哪些只是历史材料。

第三种失败是不可审查 diff。agent 一次改太多模块，提交摘要又笼统，人类 review 成本爆炸。

第四种失败是旧文档没有状态。文档越像权威，腐烂后越危险；后续 agent 可能把过期 PRD 或旧计划当成当前事实。

## 来源依据

- `resources/software-engineering-fundamentals-in-the-ai-era.pdf`: 支撑 AI 时代仍需要模块化、接口、测试、可维护性和工程纪律。
- `resources/real-engineers-ai-agent-coding-workflow.pdf`: 支撑 deep modules、TDD、小 PR、fresh context review 和人工 QA。
- `resources/build-agents-that-run-for-hours-without-losing-the-plot.pdf`: 支撑长时间 agent 需要稳定环境、可恢复状态和可审查轨迹。
- `README.md`: 提供人类入口、项目目标和 `resources/`、`ideas/` 的快速说明。
- `AGENTS.md`: 提供 agent 入口、目录约定、质量门槛、执行规则和不得编造来源的边界。

## PPT 使用建议

适合做“落地条件”或“行动建议”页。画面可以画一个 agent-friendly repo 地图：README/AGENTS 在入口，模块文档分层，测试和 CI 作为检查站，PR review 和 state file 作为交接点。
