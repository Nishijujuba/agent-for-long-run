# Harness 和 Hook：把确定性从模型脑内搬到系统外

## 状态

confirmed

## 一句话主张

模型适合生成、归纳和权衡；确定性检查、权限控制、日志审计、回滚点和验收流程应交给 harness、hook 和工具系统。

## 适用角色

程序员、平台运营工程师、算法工程师；税务和数据场景也可转化为来源校验、口径检查、人工复核流程。

## 工作场景

agent 要改代码、跑脚本、更新报告、生成文件、操作浏览器或连接生产工具。用户希望它“自动完成”，但又不能让它无限制写文件、删数据、改配置、伪造验证结果。

## 讲给 PPT agent 的内容说明

这一页要讲“信任来自机制，不来自语气”。agent 说自己检查过，不能算验收；系统真正跑过测试、输出日志、留下 diff、通过人工 QA，才有证据价值。

可以把质量机制拆成两类：

- Verification：机械验证，如 tests、typecheck、build、migration、browser smoke check、schema validation。
- Evaluation：质量评估，如 code review、manual QA、taste 判断、风险判断、业务负责人确认。

两者都需要。自动测试通过后，真实浏览器路径、数据状态、边界场景仍可能出问题；人工 QA 不能被“测试全绿”替代。

## 实际例子

课程笔记里强调 TDD：先写一个失败测试，确认它真的失败，再让 agent 写最小实现，然后重构。这样可以先装仪表，再让 agent 开车。若实现后才让 agent 自己补测试，它可能写出迎合当前实现的测试。

另一个例子是 fresh context review：实现上下文负责生成改动；审查上下文只拿任务说明、diff、测试结果和审查标准。这样 reviewer 不背着实现阶段的探索噪声，更容易发现遗漏迁移、边界条件和不合理测试。

还可以讲人工 QA 抓到自动测试没覆盖的问题，例如真实浏览器路径、SQLite 迁移或数据状态问题。自动化验证决定“有没有明显坏掉”，人工评估决定“是否真的可接受”。

## 应掌握的概念和方法论

- Hook：在关键时点触发规则，例如编辑后跑测试、提交前检查 diff、危险操作前请求确认。
- Harness：包住模型的外层系统，管理工具、循环、权限、预算、日志、状态和失败回流。
- Permission ladder：read-only first → sandbox → staging → production，逐级开放。
- Red-green-refactor：先失败测试，再最小实现，再清理结构。
- Fresh reviewer context：新上下文审旧 diff。
- Rollback point：每轮小提交或快照，让错误能撤回。

## 常见失败方式

第一种失败是把 accept-edits 当成信任模式。权限打开只是减少交互摩擦，它不会保证代码正确。

第二种失败是没有最小权限。agent 一开始就能写生产数据、删除文件、改配置，风险远大于收益。

第三种失败是没有证据链。没有日志、测试输出、diff、截图和审查记录，后续无法判断 agent 到底做了什么。

第四种失败是把 verification 和 evaluation 混为一谈。测试通过只是必要条件，业务是否合理、风险是否可接受，仍要人判断。

## 来源依据

- `resources/agent-frontline-practice-implementation-experience-and-know-how.pdf`: 支撑一线使用中的权限、验证和人工复核经验。
- `resources/why-agentic-ai-fails-infinite-loops-planning-errors-and-more.pdf`: 支撑用预算、停止条件、hook 和日志抑制失败循环。
- `resources/how-claude-code-works-in-large-codebases.pdf`: 支撑 coding agent 工作流中的 diff、测试、权限与审查。
- `resources/real-engineers-ai-agent-coding-workflow.pdf`: TDD、fresh context review、verification/evaluation、manual QA、小 PR。

## PPT 使用建议

适合做“质量与安全”页。画面可以是 Before Start、After Edit、Pre-commit、Release 四个关口，每个关口放对应 hook。核心句子是：模型负责生成，系统负责约束，人负责判断。