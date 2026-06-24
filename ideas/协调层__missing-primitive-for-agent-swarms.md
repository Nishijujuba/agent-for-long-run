# 协调层：让多个 agent 有调度台和交接本

## 状态

candidate

## 一句话主张

长程任务里，多个 agent 要稳定协作，关键是设计一层“协调层”：记录状态、分配子任务、交接证据、执行门禁、触发人工审批，让 agent 群像一支有调度台的团队。

## 适用角色

跨角色。PPT 中建议以平台运营工程师为主例，程序员、数据分析师、算法工程师和税务管理员可作为旁例。

## 工作场景

平台运营工程师处理一次跨系统安全修复：一个父 agent 收到 CVE、告警或工单后，把影响范围分析、代码或配置检查、测试验证、PR 说明、发布风险清单分给多个子 agent。协调层负责记录每个子任务处于哪个状态、证据来自哪里、下一步归谁、哪些门禁已通过、哪些动作必须等待人类批准。

这个场景适合普通工作者理解，因为它像一张项目调度台：有人收件、有人分派、有人追踪状态、有人检查质量、负责人在关键节点拍板。

## Agent 协作方式

- 人类设定目标、风险等级、资料范围、权限边界和验收标准。
- 父 agent 把大任务拆成微步骤，例如盘点影响范围、检查代码、运行测试、写 PR、整理发布说明。
- 子 agent 分别执行低风险、可追踪的子任务，并带回证据、状态和待确认问题。
- 协调层保存共享状态：谁在做什么、做到哪一步、证据 ID 是什么、是否遇到 CI 失败、合并冲突、评审阻塞或权限限制。
- 门禁负责拦住高风险动作，例如安全门、合规门、评审门、CI 门、合并门。
- 人类在门禁节点介入，尤其是生产变更、对外发布、合规判断和责任归属相关动作。

## 长程任务设计要点

- 目标：把“让几个 agent 一起干活”改成“让一组 agent 在可追踪流程中交付一个可验收结果”。
- 资料：输入包括工单、PR、告警、CI 日志、代码差异、评审意见、发布记录和合规要求，让每个子任务都有可追溯依据。
- 触发器：任务可由 PR、工单、告警、webhook、排期任务或人工指令触发。
- 微步骤：避免只写“计划、开发、测试、评审、发布”五个大词；要拆到可执行、可检查的步骤，例如“检查最后一次 CI 失败”“解决合并冲突”“补充 reviewer 要求的证据”。
- 状态机：可用 \(M=(S,s_0,\Sigma,\delta,F)\) 表示流程。\(S\) 是状态集合，\(s_0\) 是起点，\(\Sigma\) 是事件，\(\delta\) 是状态转移规则，\(F\) 是终点状态。PPT 中可简化为“状态清单 + 触发事件 + 下一步规则”。
- 耐久执行：长程任务需要保存 durable state。聊天上下文会变长，真正有效的信息占比会下降，可用 \(\rho_t=\frac{|E_t|}{|C_t|}\) 表示“有效上下文占比”。协调层要把关键结论写入文件、工单或状态表，减少 context rot。
- 工具：GitHub 或代码仓库、CI、工单系统、状态表、审批记录和日志检索工具共同构成协调层的执行与追踪基础。
- 门禁：CI gate、review gate、security gate、compliance gate、merge gate 分别对应自动检查、人工复核、安全规则、合规规则和最终合并。
- 交接：每个子 agent 输出必须包含任务 ID、证据来源、当前状态、阻塞原因、下一责任方和建议动作。
- 验收标准：所有关键结论有证据；所有高风险动作有审批记录；所有失败状态有处理路径；最终交付物包含已完成项、阻塞项、风险项和人类确认项。

## 来源依据

- `resources/missing-primitive-for-agent-swarms.pdf` - PDF 明确内容：标题为 “The Missing Primitive for Agent Swarms”，目录围绕 swarm、fleet、event/trigger、coordination layer、runtime、orchestration、state machine、durable execution、gate、CLI gateway 和 graph workflow 展开。
- `resources/missing-primitive-for-agent-swarms.pdf` - PDF 明确内容：Runtime、Orchestration、Coordination Layer 被并列讨论；其中 orchestration 包含触发、排队、扩缩容、重试、超时、环境清理和状态报告。
- `resources/missing-primitive-for-agent-swarms.pdf` - PDF 明确内容：Coordination Layer 包含 message passing、task handoff、shared state、gates、human intervention、CI status、merge conflict、review ownership 等协作控制点。
- `resources/missing-primitive-for-agent-swarms.pdf` - 可核图示/结构：第 13 页图示把 Coordination 标成缺失层，职责包括决定工作应如何发生、追踪进度、执行门禁、管理状态。
- `resources/missing-primitive-for-agent-swarms.pdf` - 可核图示/结构：第 25 页图示说明 SDLC 会被拆成 micro steps，截图中出现 final CI check、resolve conflicts、merge PR 等细粒度步骤。
- `resources/missing-primitive-for-agent-swarms.pdf` - 可核图示/结构：第 26 页用 context rot 相关公式表达有效上下文与总上下文的比例问题，说明长程执行需要 durable state 和外部状态记录。
- `resources/missing-primitive-for-agent-swarms.pdf` - 可核图示/结构：第 28 页给出状态机 \(M=(S,s_0,\Sigma,\delta,F)\)，示例状态包括 TaskReceived、PlanReady、TestsRunning、MergeConflictDetected、EscalatedToHuman。
- `resources/missing-primitive-for-agent-swarms.pdf` - 可核图示/结构：第 29 页图示把 state machines、durable execution、gates、CLI packaging 作为接近可用答案的一组原语。
- 分析推断：面向普通工作者时，“coordination layer”最适合翻译成“调度台和交接本”，因为它把抽象工程概念转成状态追踪、任务交接、质量门禁和人工审批。
- 分析推断：平台运营安全修复场景是对 PDF 中 agent swarm、Ona automation、VM fleet、CI、PR、review、merge conflict 等工程语境的本项目化改写。

## 风险与边界

- 协调层只能降低混乱，不能保证 agent 输出正确。关键事实仍要回到证据、日志、代码、工单、政策原文或人工判断。
- 多 agent 会增加成本、延迟和协调复杂度。简单任务使用单 agent 加检查清单就足够。
- 状态机设计过粗会漏掉关键风险，设计过细会让流程变重。普通工作者只需要掌握“状态、事件、下一步、终点”四件事。
- 涉及生产系统、税务、合规、隐私和对外发布时，agent 只能辅助整理、检查和建议；最终业务判断由人类或制度授权负责人承担。
- 若共享状态表保存了敏感信息，需要明确权限、脱敏和留存周期。

## PPT 使用建议

适合放在第 9 页“长程任务方法”，也可作为第 3 页“人和 agent 分工框架”的补充页。

页面主句建议：多个 agent 要稳定协作，需要一张调度台：状态、交接、门禁、审批都要看得见。

建议图示：左侧是触发器（工单、PR、告警、排期），中间是协调层（状态机、共享状态、门禁、人工审批），右侧是多个子 agent 和最终交付物。类比可以用“调度台”：任务从入口进入，调度台分派给不同同事，每一步留下交接记录，负责人只在关键关口签字。

PPT 正文建议只保留“状态、事件、下一步、终点”四件事；状态机公式和 \(\rho_t=\frac{|E_t|}{|C_t|}\) 更适合放在 speaker notes。

选择该 idea 的原因：现有候选稿已经覆盖上下文预算、三道护栏和 harness；这份 PDF 的独特价值在于解释多个 agent 同时工作时为什么需要协调层。替代方案是讲 VM runtime 或 Ona demo，代价是更偏工程基础设施，普通听众的直接行动感较弱。

## 待确认问题

- 中文术语采用“协调层”，还是采用更口语化的“任务调度台”？
- PPT 主例采用平台运营安全修复，还是换成税务政策更新、数据报告生产、程序员代码修改？
- 状态机公式 \(M=(S,s_0,\Sigma,\delta,F)\) 放在正文、页脚，还是只放 speaker notes？
- 该 idea 是否单独成页，还是并入“长程任务方法”页，与上下文预算和门禁一起呈现？
