# 07 质量与安全：Hook / Harness 把确定性放到模型外

## 状态

confirmed

## 一句话主张

可靠的长程 agent 系统会把可程序化的规则放进 hook、harness、测试、日志和权限控制，让模型负责生成与判断建议，让系统负责拦截、记录和验证。

## 适用角色

跨角色：代码修改、数据处理、税务材料整理、算法实验、运维排障、平台自动化任务。

## 工作场景

程序员让 agent 修改代码。Agent 可以写实现、补文档、解释 diff；系统在 After Edit 自动跑 lint、typecheck、unit test，在 Pre-commit 检查敏感文件、权限范围和回滚点。人类最后做 code review、业务 QA 和上线判断。

## Agent 协作方式

Agent 是生成器和执行者；harness 是轨道、仪表和刹车；人类是最终判断者。确定性检查尽量交给脚本、测试、schema、CI、权限策略和日志审计，减少模型凭感觉自检。

## 长程任务设计要点

- Before Start：检查范围、权限、资料来源、工作目录和禁止操作。
- After Edit：自动运行测试、lint、typecheck、build、迁移检查、浏览器 smoke check。
- Pre-commit：检查 diff 可读性、敏感信息、回滚点、提交粒度和状态文件更新。
- 质量上限可用直觉公式表达：`QAI <= f(V, E)`。
- `V = Verification`：测试、类型检查、构建、迁移、日志、返回值。
- `E = Evaluation`：人工 QA、taste、风险判断、code review、业务验收。

## 来源依据

- `resources/context-graphs-decision-traces-of-ai-agents.pdf`: 本次讨论中用于提炼 hook、harness、trace、验证闭环和决策痕迹的素材。
- `README.md`: 仓库要求内容优先解释工作方法、协作方式、风险边界和可落地流程。
- `AGENTS.md`: 质量门槛要求涉及税务、合规、隐私、权限和生产系统操作时写清 agent 边界。

## 风险与边界

自动测试通过仍可能漏掉真实用户路径、业务口径和异常场景。Hook 也不能无限扩大权限；生产工具接入要从只读、沙箱和 staging 开始，所有写操作应有日志、审批和回滚机制。

## PPT 使用建议

**页面标题**：Hook / Harness：把确定性放到模型外

**页面主文案**：

- Before Start：范围、权限、资料。
- Agent：生成与修改。
- After Edit：测试、lint、diff。
- Pre-commit：审查、回滚点。
- `QAI <= f(V, E)`：Verification 加 Evaluation 决定质量上限。
- 模型负责生成；系统负责约束；人负责判断。

**讲法**：把 agent 类比成高性能驾驶员，harness 是车道线、仪表盘和刹车系统。长程任务要靠外部证据推进：测试结果、日志、diff、截图、返回值、人工 QA。

## 待确认问题

- 是否需要为税务和数据报告场景补充非代码类 hook 示例，例如来源引用检查、敏感字段扫描、口径变更确认？
