# agent-long-run-pingan-v1 审查记录

## 版本与位置

- 版本：v1
- PPT 文件：`outputs/agent-long-run-pingan-v1.ppt`
- 本地可下载 PPTX：`outputs/agent-long-run-pingan-v1.pptx`
- 审查文件：`outputs/agent-long-run-pingan-v1-review.md`
- 目标分支：`ppt-v1`

## 内容来源

- `README.md`：项目定位、目标听众、最终 PPT 主题与推荐流程。
- `AGENTS.md`：目录约定、质量门槛、PPT 页级规则、agent 执行规则。
- `ideas/01-long-task-is-engineering-system.md`
- `ideas/02-context-working-radius-attention-and-ffn.md`
- `ideas/03-task-contract-and-human-in-the-loop.md`
- `ideas/04-prd-kanban-dag-and-vertical-slices.md`
- `ideas/05-agent-loop-evidence-and-stop-conditions.md`
- `ideas/06-subagents-swarms-and-shared-state.md`
- `ideas/07-memory-handoff-and-doc-lifecycle.md`
- `ideas/08-harness-hooks-and-verification.md`
- `ideas/09-agent-readable-codebase-and-deep-modules.md`
- `ideas/10-common-failure-modes-and-antidotes.md`

## 主体结构审查

PPT 共 14 页：

1. 封面：明确主题、版本 v1、受众范围和项目来源。
2. 引入页：解释长程任务失败动机，建立“链路护栏”问题。
3. 总框架页：用 `Agent = LLM × Harness × Memory × Tools` 总领全篇。
4. Idea 01：长程 Agent 是工程系统。
5. Idea 02：上下文有稳定工作半径。
6. Idea 03：先做任务契约。
7. Idea 04：PRD/Kanban/DAG 与垂直切片。
8. Idea 05：Agent Loop、外部证据与停止条件。
9. Idea 06：子代理、swarm 与共享状态。
10. Idea 07：文件化记忆、交接与文档生命周期。
11. Idea 08：Harness、Hook、verification/evaluation 与权限阶梯。
12. Idea 09：agent-readable repo、深模块、测试边界。
13. Idea 10：常见失败方式与对应防法。
14. 行动建议：从一个高频任务开始，用 1 个对齐段 + 3 个 AFK issue 落地。

每个 idea 均对应独立页；每页保留一个中心论点、一个关键机制、若干落地要点，适合 8 分钟左右演讲时按“动机 → 方法 → 风险边界”展开。

## 完整性审查

- 已覆盖 README 中的核心主题：知识工作者如何使用 agent 加速工作、如何协作、如何设计 agent 完成长程任务。
- 已覆盖 AGENTS.md 中的质量门槛：具体角色/任务、检查点、验收标准、风险边界、算法概念服务实践理解。
- 已覆盖 10 个 confirmed ideas 的主张、方法论和失败模式。
- 已加入总框架页、引入页和行动建议页，避免只有单页 idea 堆叠。
- 版本标识已写入封面与文件名。

## 逻辑与动机审查

整体叙事顺序为：

1. 先说明长程任务失败原因。
2. 再给出总框架：模型、harness、memory、tools 协同。
3. 接着进入可操作方法：任务契约、上下文管理、PRD/DAG、loop、子代理、交接、hook、代码仓设计。
4. 最后用失败模式页和行动建议页收束，提示高风险组合和落地路径。

该顺序符合普通工作者的认知路径：先看到问题，再理解机制，最后拿到行动清单。

## 视觉审查

- 采用橙色、白色、浅灰与少量绿色作为主视觉，参考中国平安商务演示常见的高对比橙白风格。
- 每页采用统一页眉、标签、卡片和留白结构。
- 已在本地将 PPTX 渲染为 PDF 并生成总览图检查；未发现图形重叠或遮挡。
- 文字密度控制在可演讲范围；页面内容用于提示，详细解释留给讲者。

## 需要后续人工确认的事项

- 该版本用于方法论演示，未嵌入中国平安官方 logo、字体或受版权保护的模板资产。
- 若需要正式对外使用，建议由品牌/法务确认色值、logo、页脚与版权声明。
