# Agent Loop 要接地：每轮都必须有证据、预算和停止条件

## 状态

confirmed

## 一句话主张

长程 agent 不是“让模型一直想”，而是在有限预算内循环执行：计划、行动、观察、修正；每一轮都要从环境拿到 ground truth，并知道何时停止。

## 适用角色

程序员、平台运营工程师、数据分析师；也适合多资料研究、报告生成、批量处理和流程自动化。

## 工作场景

agent 被交给一个 backlog：读资源、提炼 ideas、写 markdown、检查来源；或处理代码 issue、修改文件、跑测试、提交总结。它需要持续多轮行动，但不能无限尝试、无限修补、无限解释。

## 讲给 PPT agent 的内容说明

这一页要讲执行循环的骨架：Plan -> Act -> Observe -> Revise。关键不是 Plan，而是 Observe。没有环境反馈的 agent loop 会变成模型在脑内证明自己应该成功；有环境反馈时，agent 才能根据测试、日志、diff、截图、数据库返回、文件状态和人工审查修正方向。

还要讲停止条件。长程任务中，“继续努力”本身不是美德。好的 agent 应该知道什么时候输出 `no more tasks`，什么时候因为连续失败而停下，什么时候把失败回流成新 issue，什么时候请求人类判断。

## 应掌握的概念和方法论

- Plan / Act / Observe / Revise：长程执行的基本循环。
- Ground truth：来自外部环境的证据，不是模型自称“已完成”。
- Stop condition：任务完成信号、最大轮数、时间/成本预算、错误次数上限、人工确认点。
- Backlog scheduler：从已解锁任务中按优先级拿下一项，而不是凭感觉乱跑。
- Error-to-issue：失败不要无限重试，要变成可分析、可排期的新任务。
- Trace log：记录每轮做了什么、为什么做、结果是什么。

## 常见失败方式

第一种失败是无限循环。工具失败、测试失败或目标模糊时，agent 会不断换说法、改同一片代码、重跑同一条命令，却没有新的证据。

第二种失败是无证据推进。agent 写“已完成”“已修复”“应该可以”，但没有测试输出、文件 diff、日志或人工 QA 证明。

第三种失败是把权限打开当成信任。accept edits 或自动执行只降低摩擦，不会让 agent 的判断天然正确。

## 来源依据

- `resources/build-agents-that-run-for-hours-without-losing-the-plot.pdf`: 支撑长时间运行时需要循环控制、状态恢复、预算和停机信号。
- `resources/real-engineers-ai-agent-coding-workflow.pdf`: 支撑 AFK issue 调度、按优先级取任务、完成后给出固定停止信号、小步提交和人工审查。
- `resources/software-engineering-fundamentals-in-the-ai-era.pdf`: 支撑软件工程中的反馈循环、测试和小步可验证交付。
- `AGENTS.md`: 要求 agent 自检事实来源、逻辑、角色场景和是否服务最终 PPT。

## PPT 使用建议

适合做“执行循环”页。画面可以是一圈 Plan -> Act -> Observe -> Revise，旁边放三道刹车：预算、权限、停止条件。核心讲法是：agent 要跑长程任务，必须每一步都踩到地面。
