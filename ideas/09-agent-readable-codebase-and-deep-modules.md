# 让代码库适合 Agent：入口清楚、模块够深、测试边界可见

## 状态

confirmed

## 一句话主张

agent 在大代码库里的表现，不只取决于模型聪明程度，还取决于代码库是否给它清晰入口、清晰边界、稳定命令和可验证反馈。

## 适用角色

程序员、算法工程师、平台运营工程师；也适合维护复杂知识库、流程库和数据资产的团队。

## 工作场景

团队想让 Claude Code 或其他 coding agent 在大代码库中长期工作：读需求、找模块、改代码、跑测试、写总结。若仓库没有入口文档、模块边界混乱、测试难跑、命令不稳定，agent 会花大量上下文猜结构，最后产出大而散的改动。

## 讲给 PPT agent 的内容说明

这一页要强调：大代码库不能靠“把所有文件塞进 prompt”解决。更好的方式是让 agent 用工具导航，并让仓库本身变得可读：

- 根目录有 `README.md` 说明项目目标和快速入口。
- 根目录有 `AGENTS.md` 或类似文件说明 agent 执行规则、目录约定、质量门槛。
- 重要模块有局部文档，说明接口、边界、测试命令、常见坑。
- 代码结构有深模块：小接口、大内部，把复杂度封装在可测试单元里。
- 测试边界靠近真实行为，让 agent 能用测试结果校准自己。

可以把 deep module 比成“有清晰门牌的房间”。agent 不需要理解整栋楼每根管线，只要知道从哪扇门进去、接口是什么、测试怎么跑、行为是否满足契约。

## 实际例子

课程笔记中，architecture skill 会帮助列出可加深的模块候选，例如 quiz scoring、gamification service、lesson completion logic 的耦合关系、依赖类别和测试影响。它不是自动重构按钮，而是帮助工程师发现边界：哪些模块高度耦合，哪些组合适合被当作测试单元，哪里缺测试。

另一个例子是 video editor 这种复杂区域。若测试能从外部按用户动作触发，比如前端按下某个按钮，再一路观察它是否抵达后端并产生预期行为，agent 就能看见整个 flow，也能通过整个 flow 接收反馈。没有这种测试时，agent 很难稳定工作。

## 应掌握的概念和方法论

- Agent-readable repo：README、AGENTS、模块文档、runbook、测试命令都清楚。
- Deep module：接口小而稳定，内部复杂度可被封装和测试。
- Module map：给 agent 和人类看的系统地图，说明关键服务、路由、数据模型和测试边界。
- Search-first workflow：先搜索和读取相关文件，再修改；避免凭记忆猜路径。
- Locality：一次任务尽量限制在少量相关模块里，减少跨区域副作用。
- Reviewability：diff 要小而自包含，让人类能审。

## 常见失败方式

第一种失败是浅模块。逻辑散在多个文件里，接口不清，agent 改一处牵动十处。

第二种失败是没有仓库入口。agent 不知道先读什么、命令怎么跑、哪些目录是临时材料、哪些文件是权威规则。

第三种失败是测试边界不清。agent 可以改出看似合理的代码，却没有端到端或模块级证据证明行为真的对。

第四种失败是不可审查 diff。agent 一次改太多模块，提交摘要又写得笼统，人类 review 成本爆炸。

## 来源依据

- `resources/how-claude-code-works-in-large-codebases.pdf`: 支撑 Claude Code / coding agent 在大代码库中的导航、项目规则和上下文策略。
- `resources/agent-frontline-practice-implementation-experience-and-know-how.pdf`: 支撑一线实践中先整理入口、命令、边界，再让 agent 执行。
- `resources/real-engineers-ai-agent-coding-workflow.pdf`: deep modules、architecture skill、gamification service、lesson completion logic、video editor flow 测试。
- `README.md` 与 `AGENTS.md`: 本仓库已经把人类入口和 agent 入口分开，是 agent-readable repo 的小例子。

## PPT 使用建议

适合做“代码仓与长期协作”页。画面可以是一张 repo 地图：README/AGENTS 在入口，模块文档分层指向深模块，测试和 runbook 作为外部反馈。不要讲成“Claude Code 使用技巧”，而要讲成“让工作环境适合 agent”。