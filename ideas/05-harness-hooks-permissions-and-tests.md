# Harness、Hook 和测试：把可靠性放在系统里，不放在模型承诺里

## 状态

confirmed

## 一句话主张

模型负责理解、生成和权衡；确定性检查、权限控制、日志审计、回滚点和验收流程，应该由 harness、hook、测试和人工评估承担。

## 适用角色

程序员、算法工程师、平台运营工程师；税务、数据和内容生产场景也可以转化为来源校验、口径检查和人工复核流程。

## 工作场景

agent 要改代码、写报告、生成文件、跑脚本、更新配置、操作浏览器或连接外部工具。用户希望它自动完成，但又不能允许它无边界地写文件、删数据、改生产系统或伪造验证结果。

## 讲给 PPT agent 的内容说明

这一页应该讲“信任来自机制，不来自语气”。agent 说“我检查过”不等于真的通过验收；系统跑过测试、保留日志、留下 diff、通过人工 QA，才是可审查的证据。

可以把机制分成三层：第一层是 harness，负责包住模型，管理工具、循环、预算、状态、权限和日志；第二层是 hook，在关键节点自动触发规则，例如编辑后跑测试、提交前检查 diff、危险操作前请求确认；第三层是 verification 与 evaluation，前者是机械验证，后者是人类或专家判断。

## 应掌握的概念和方法论

- Harness：模型外层的执行框架，负责工具、权限、预算、状态和日志。
- Hook：在 before/after edit、pre-commit、release 等节点触发检查或拦截。
- Permission ladder：read-only first -> sandbox -> staging -> production，逐级开放权限。
- Red-green-refactor：先写失败测试，再让 agent 写最小实现，最后重构。
- Verification vs evaluation：测试、typecheck、build 是验证；业务合理性、合规风险、审美和用户体验是评估。
- Rollback point：小提交、快照或分支，让错误能撤回。

## 常见失败方式

第一种失败是把自动接受编辑当作信任模式。权限打开只是减少交互摩擦，不会保证结果正确。

第二种失败是实现后再让 agent 自己补测试。这样测试很容易迎合当前实现，而不是约束真实需求。

第三种失败是没有审计链。没有日志、diff、测试输出、截图和人工审查记录，后续无法判断 agent 到底做了什么。

第四种失败是把 verification 和 evaluation 混为一谈。测试通过只是必要条件，业务是否可接受仍需要人判断。

## 来源依据

- `resources/real-engineers-ai-agent-coding-workflow.pdf`: 支撑 TDD、fresh context review、manual QA、小 PR、verification/evaluation 分工。
- `resources/build-agents-that-run-for-hours-without-losing-the-plot.pdf`: 支撑长程运行需要 harness、hook、权限、预算和日志来避免跑偏。
- `resources/software-engineering-fundamentals-in-the-ai-era.pdf`: 支撑 AI 时代仍要依靠测试、模块边界、review 和工程纪律。
- `AGENTS.md`: 要求每个核心论点有来源或推理链，长程任务必须包含检查点和验收标准。

## PPT 使用建议

适合做“质量与安全”页。画面可以画四个关口：Before Start、After Edit、Pre-commit、Release，每个关口放一个 hook。核心表达是：模型负责生成，系统负责约束，人负责判断。
