# Agent Loop 必须接地：每轮都要证据、预算和停止条件

## 状态

confirmed

## 一句话主张

长程 agent 执行不是“让模型一直想下去”，而是让它在有限预算内循环：计划、行动、观察、修正，并在每轮从环境拿到 ground truth。

## 适用角色

程序员、平台运营工程师、数据分析师；也适合任何需要多轮检索、修改、检查、复盘的任务。

## 工作场景

用户把已经拆好的 backlog 交给 agent，希望它按优先级完成 AFK task。比如处理本地 issue、修改代码、跑测试、提交总结；或者让 agent 对多个资料文件逐步提炼 ideas、写入 markdown、检查来源完整性。

## 讲给 PPT agent 的内容说明

这一页要解释 Agent Loop 的骨架：

1. Plan：根据任务契约和当前状态决定下一步。
2. Act：调用工具，读文件、改文件、跑命令、查询数据、生成中间产物。
3. Observe：读取工具返回、测试结果、日志、diff、截图、数据库状态。
4. Revise：根据观察修正计划，继续或停止。

关键在 Observe。没有观察的 agent loop 会变成自嗨推理；模型会解释自己为什么应该成功，却没有证据证明它真的成功。

可以加入一个简单调度公式：

`t* = arg max score(t_i), t_i ∈ B`

意思是从 backlog `B` 中选择当前分数最高且已解锁的任务。这里不需要真的讲优化算法，只要表达“agent 要从任务池里按规则拿下一项，而不是凭感觉乱跑”。

## 实际例子

课程笔记里有 Ralph once.sh 的例子：脚本读取所有本地 issue，读取最近五个 commits，把这些信息拼进上下文，再启动 Claude Code，并允许它直接接受编辑权限。它的 prompt 约束很关键：只处理 AFK issues；如果所有 AFK tasks 都完成，输出固定文本 `no more tasks`；按优先级选择下一项。

这个例子说明，loop 的价值不在 prompt 辞藻，而在调度、上下文入口、权限边界、停止信号和结果审查。一个朴素的顺序循环能先暴露任务拆得太大、测试反馈不足、权限过宽、提交太散等问题。

## 应掌握的概念和方法论

- Ground truth：来自环境的证据，不是模型自己声明。
- Stop condition：`no more tasks`、循环次数、时间预算、成本预算、错误次数上限。
- Backlog scheduler：从已解锁任务中按优先级取下一项。
- Small self-contained PR：每次交付可读、可审查、可回滚。
- Trace / log：记录每轮做了什么、为什么、结果是什么。
- Error to issue：失败不是继续瞎修，而是回流成新 issue。

## 常见失败方式

第一种失败是无限循环。agent 没有明确完成条件，工具失败后反复重试，或者在同一条错误路径上自我修补。

第二种失败是无证据推进。agent 说“已完成”“已修复”“看起来没问题”，但没有测试、日志、diff 或人工 QA 支撑。

第三种失败是权限过早放开。accept-edits 或写权限只减少交互摩擦，并不等于代码正确。若任务模糊、测试薄弱、没有回滚点，开权限只是把风险放大。

## 来源依据

- `resources/why-agentic-ai-fails-infinite-loops-planning-errors-and-more.pdf`: 支撑无限循环、规划错误、停止条件缺失等失败模式。
- `resources/agent-frontline-practice-implementation-experience-and-know-how.pdf`: 支撑一线落地时从小循环开始，而不是直接上复杂自动化。
- `resources/how-claude-code-works-in-large-codebases.pdf`: 支撑 agent 在代码任务中读取 backlog、项目文件和最近提交再执行。
- `resources/real-engineers-ai-agent-coding-workflow.pdf`: Ralph once.sh、AFK issues、`no more tasks`、小 PR 和审查责任。

## PPT 使用建议

适合做“执行循环”页。画面可以是一圈 Plan → Act → Observe → Revise，旁边放三道刹车：预算、权限、停止条件。强调模型不是闭眼跑马拉松，而是每一步都踩到地面。