# 先定验收：让长程 Agent 跑几小时仍不丢主线

## 状态

candidate

## 一句话主张

长程 Agent 开始执行前，应先把“完成”写成可检查契约，再让独立评估角色用真实工具验收，并把 trace 留作下一轮改进依据。

## 适用角色

跨角色，尤其适合程序员、数据分析师、平台运营工程师，也可迁移到税务管理员的政策口径整理、算法工程师的实验复盘。

## 工作场景

数据分析师让 agent 连续数小时完成一份月度异常分析报告：读取多张表、统一指标口径、找出异常波动、生成图表、写出结论和风险提示。

开工前，人先要求 agent 把任务拆成一组验收契约，例如：

- 数据来源、时间范围、口径定义必须列清。
- 每个异常结论必须能追溯到表格、SQL、图表或计算过程。
- 图表必须标明单位、口径、样本范围和异常阈值。
- 报告必须列出无法确认的数据缺口。
- 涉及经营判断、税务合规或生产系统动作时必须交回人类确认。

这像开工前先签一张“验收单”。验收单越清楚，agent 后面跑得越久，越不容易把半成品误报成完成品。

## Agent 协作方式

可以把流程压成：

\[
P \rightarrow (G \leftrightarrow E \text{ on } C) \rightarrow A \rightarrow T
\]

其中：

- \(P\) 是 Planner，负责把一句目标变成高层方向和阶段边界。
- \(G\) 是 Generator / Builder，负责执行、生成、修改和整理。
- \(E\) 是 Evaluator / Critic，负责按证据验收、指出失败、要求重跑。
- \(C=\{c_1,c_2,\ldots,c_n\}\) 是 Contract，也就是提前谈好的验收断言集合。
- \(A\) 是最终工件，例如报告、代码、图表、材料清单或工单复盘。
- \(T\) 是 trace，记录 agent 看到什么、做了什么、怎样判断、在哪里失败。

人的角色是设目标、定边界、确认高风险判断、复盘失败样本。Agent 的角色是按契约执行、留下证据、接受独立评估。

## 长程任务设计要点

1. 先定义 Done。把“做一份好报告”“修好这个功能”“整理政策口径”改写成可观察、可检查的断言。
2. 拆分 Builder 与 Evaluator。Builder 容易相信自己刚做的东西；Evaluator 应有独立上下文、独立提示词和挑错职责。
3. 让 Evaluator 使用真实工具。前端任务要打开页面、点击路径、看 console 和 network；数据任务要复算样本、检查口径、抽查来源；文档任务要检查引用和未确认项。
4. 用文件交接状态。contract、feedback、result、trace 可以落到 Markdown、JSON、表格或日志中，避免长会话把状态搅混。
5. 把 Rubric 写细。Rubric 是评价量规，把主观质量拆成可讨论标准。例子：准确性、完整性、可追溯性、边界说明、可执行建议。
6. 给 Evaluator 重启权。若方向已经偏了，继续补丁会把坏结构越修越厚；Evaluator 应能要求重新开局。
7. 读失败 trace。trace 像 agent 任务的“判断轨迹”。它比普通日志更重要，因为长程任务的核心故障常出在模型把弱证据当成强证据。
8. 七要素映射：目标=Done，资料=数据来源/引用证据，计划=Planner 分阶段，检查点=Evaluator 每轮验收，记忆=contract/feedback/result/trace 文件，工具=真实复算/浏览器/日志，验收=contract criteria 与 rubric。

## 来源依据

- `resources/build-agents-that-run-for-hours-without-losing-the-plot.pdf`

PDF 明确内容：

- 第 1 章说明长程 Agent 会因上下文、规划、自我判断三类问题丢线；“完成感”可能早于“完成证据”出现。
- 第 3 章提出 Generator / Builder 与 Evaluator / Critic 分离，Evaluator 需要真实使用产品，例如用 Playwright 打开页面、点击流程、截图和反馈。
- 第 4 章强调 Generator 与 Evaluator 在写代码前先协商 Done 的定义，把用户故事转成 \(C=\{c_1,c_2,\ldots,c_n\}\) 这样的 contract criteria，并通过磁盘 Markdown 文件交接。
- 第 5 章说明细粒度 criteria 能让反馈可执行，trace debugging 用来定位模型判断与人类判断分叉的位置。
- 第 9 章总结为三层结构：模型能力层、Harness 层、证据层，并给出 \(P \rightarrow (G \leftrightarrow E \text{ on } C) \rightarrow A \rightarrow T\) 的流程表达。

可核图示/结构：

- 图 2 将失控原因可视化为 context、planning、verification 三类。
- 图 12 展示 Planner、Generator、Evaluator 三角色流程。
- 图 13 展示 sprint contract 如何把 user story 转成可验证行为。
- 图 17 和图 18 展示 Evaluator 抓到具体失败，以及 27 条 contract criteria 带来的可行动反馈。
- 图 19 展示 trace debugging 的闭环：读 evaluator logs，定位判断分叉，更新 prompt 后重跑。
- 图 25 展示三层可信结构：模型能力、Harness、证据。

分析推断：

- PDF 示例偏软件和网页应用，本 idea 将“先谈 Done、独立验收、真实证据、trace 复盘”迁移到普通知识工作场景，例如数据分析报告、政策口径整理、运维排障复盘。
- 对普通工作者来说，“验收契约”比 “Harness” 更容易理解；PPT 可用“开工前先写验收单”作主类比。
- 该 idea 适合补足 PPT 中“长程任务方法”一页，因为它能把上下文、检查点、工具、验收标准和人类介入串成一个实际工作流程。

## 风险与边界

- 契约太粗会制造弱反馈。“报告要专业”“页面要好看”这类标准无法指导 agent 修正。
- Evaluator 若继承 Builder 的全部解释，容易被生成器的思路污染，应主要看产物、contract、handoff 和外部证据。
- 工具证据必须真实执行。只看 diff、摘要或最终报告，仍可能漏掉交互路径、数据口径、边界条件和异常分支。
- 长程运行成本高。PDF 中 retro game maker harness 示例约 6 小时、约 200 美元，说明这种模式应优先用于高价值任务。
- 涉及税务、合规、隐私、生产系统操作时，agent 只能辅助整理和检查，人类仍承担最终判断与责任。
- Trace 可能包含敏感数据、内部路径或业务判断，保存和复盘时需要权限边界。
- Brownfield 存量系统更难。既有代码、指标口径、审批流程和组织约束必须进入 contract 与 rubric。

## PPT 使用建议

适合放在第 9 页“长程任务方法”，也可作为第 3 页“人机分工框架”的核心机制。

建议页标题：**先签验收单，再让 Agent 长跑**

页面图示可画成一条流程：

目标 -> 验收契约 \(C\) -> Builder 执行 -> Evaluator 用工具验收 -> Trace 复盘 -> 更新契约

讲述方式：

- 用数据分析师月报作主例子，避免听众误以为这只适合程序员。
- 用一句硬话收束：没有验收契约的长程 agent，跑得越久，越可能把过程当成果。
- 术语统一成“验收单 Contract、独立检查员 Evaluator、判断轨迹 Trace”，让普通听众先抓住工作含义，再理解英文术语。

## 待确认问题

- PPT 是否优先采用“数据分析师月报”作为跨角色示例，还是采用“程序员做功能并用浏览器验收”的原始技术示例？
- “Contract / 契约”这个词是否保留，还是在面向普通工作者的版本中统一改成“验收单”？
- 这一页是否需要与已有 idea“交接检查点”“状态外化”“上下文预算”合并，避免最终 PPT 方法页过密？
