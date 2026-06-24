# 上下文预算：让 Agent 长任务不丢线

## 状态

candidate

## 一句话主张

长程任务要先设计 agent 的“上下文预算”：哪些材料放在当前窗口，哪些材料用 ID 存到可恢复记忆，哪些重活交给子 agent，这样才能减少遗漏、重复失败和越修越乱。

## 适用角色

跨角色。尤其适合数据分析师、平台运营工程师、程序员，以及需要处理大量政策、日志、会议记录、工单、代码或实验结果的知识工作者。

## 工作场景

平台运营工程师让 agent 分析一次线上故障：材料包括告警记录、日志片段、工单对话、监控截图、历史变更记录和复盘模板。若所有材料直接塞进一次聊天，agent 很容易遗漏关键证据；若每次失败后继续追加更多日志，还会形成“越重试，材料越多，越容易超出上下文”的循环。

## Agent 协作方式

- 人类先给出任务目标、证据范围、保密边界和验收标准，例如“找出最可能的故障链路，并列出每个结论对应的日志 ID”。
- Agent 先盘点材料，建立上下文预算：当前窗口只放目标、规则、关键摘要和本轮必须看的证据。
- 大块材料进入可恢复记忆或文件索引，保留 ID、来源、时间和用途；后续需要时按 ID 或查询取回。
- 搜索、日志筛选、长文比对等重任务交给子 agent 或独立步骤，主对话只接收结论、证据 ID 和待确认问题。
- 人类在关键检查点介入：确认材料范围、确认候选原因、复核证据链、批准最终输出。

## 长程任务设计要点

- 目标：把“让 agent 分析一堆材料”改成“让 agent 管理一张可追踪的证据地图”。
- 上下文窗口：可以类比为一张工作台。工作台上只能放当前推理必须用的资料，档案柜里保存可取回资料，临时计算结果放缓存。
- 上下文预算：PDF 图示给出公式 \(B = B_{sys} + B_{hist} + B_{tools} + B_{ret} + B_{ans}\)。含义是系统规则、历史对话、工具结果、检索材料和最终回答都在争用同一块空间。
- 可恢复记忆：被移出当前窗口的内容要带 ID，后续可以通过 \(R(q, M)\) 从记忆 \(M\) 中取回；这像给资料贴档案号。
- 失败循环：PDF 图示用 \(D_{t+1}=D_t+\Delta trace(failure_t,retry_t)\) 表示失败与重试会继续增加追踪数据；若用于 PPT，可简化讲成“失败重试会把材料堆得更厚”。直观说，失败重试会产生更多日志和解释材料，下一轮 agent 更难看清重点。
- 检查点：材料清单检查、上下文选择检查、证据 ID 检查、结论复核检查、最终报告验收。
- 验收标准：每个关键结论都有来源 ID；关键反例已检查；敏感数据未进入长期记忆；子 agent 输出能回溯到原始证据；最终答案没有把缓存或过期记忆当成事实。

## 来源依据

- `resources/solving-context-management-in-agents.pdf` - 可核英文图示/公式：PDF 内嵌图示写出“Context engineering = choosing what the model sees”，说明上下文工程的核心是选择模型能看到什么。
- `resources/solving-context-management-in-agents.pdf` - 可核英文图示/公式：PDF 的“Self-amplifying failure loop”图说明失败与重试会继续增加 trace/span 数据，相关公式为 \(D_{t+1}=D_t+\Delta trace(failure_t,retry_t)\)。
- `resources/solving-context-management-in-agents.pdf` - 可核英文图示/公式：PDF 对“Naive Truncation”的结果描述为 agent 会忘掉内容，后续追问像新对话。
- `resources/solving-context-management-in-agents.pdf` - 可核英文图示/公式：PDF 对“LLM summarization as compression”的问题标注为不稳定、难控制重要性、可靠性不足。
- `resources/solving-context-management-in-agents.pdf` - 可核英文图示/公式：PDF 给出“Smart Truncation + Memory”：保留 head 和 tail，中间截断内容用 ID 存入 memory，可按 ID 取回，并强调保护 system prompts 和 recent tool results。
- `resources/solving-context-management-in-agents.pdf` - 可核英文图示/公式：PDF 给出“Sub-Agents”：把重任务外包给子 agent，让主对话只保留轻量上下文和结果。
- `resources/solving-context-management-in-agents.pdf` - 待视觉复核的结构摘要：PDF 的“Context Management Roadmap”把当前系统概括为智能截断、记忆存储、子 agent 隔离大块数据；压力点包括大输入仍会撞限制、选择仍偏启发式、新会话丢历史；下一阶段需要长期记忆、原则化上下文预算和上下文质量评估。
- `resources/solving-context-management-in-agents.pdf` - 待视觉复核的结构摘要：PDF 的“Storage and Visibility Layers”区分 live context、retrievable memory、long-term memory、cache，并列出失败模式：关键证据漏看、召回差或取错、事实过期或隐私边界问题、缓存污染。
- 分析推断：面向普通知识工作者时，最有 PPT 价值的提炼是“上下文预算 + 可恢复证据 ID”，因为它能直接转化为材料管理、检查点和验收标准。

## 风险与边界

- 上下文预算如果由 agent 自行决定，可能把关键证据移出当前窗口；人类需要抽查被移出的材料清单。
- 可恢复记忆会带来隐私和权限问题。税务材料、客户数据、生产日志、实验数据不能默认进入长期记忆。
- 总结压缩会丢细节，尤其会丢少数异常、反例、限制条件。重要证据要保留原文 ID。
- 子 agent 能隔离重任务，也会制造新的盲区；主 agent 必须带回证据 ID、失败信息和未解决问题。
- 缓存能省时间，也可能复用过期结果；涉及生产系统、合规判断或实验评估时，缓存需要失效规则。
- 这份 idea 适合讲工作方法，算法细节只保留公式直觉，避免展开模型结构。

## PPT 使用建议

适合放在第 9 页“长程任务方法”。一页只讲一个图：左边画“工作台、档案柜、外包助理”三格；右边放三条操作规则：当前窗口只放本轮必需证据、移出窗口的材料必须带 ID、重任务交给子 agent 并带回证据链。讲解时用平台故障复盘或数据分析报告作为例子。

## 待确认问题

- 该 idea 在最终 PPT 中使用哪个角色案例最有说服力：平台运营工程师故障复盘、数据分析师报告生成，还是程序员代码修改？
- 是否需要把“上下文预算”固定成一个团队模板，例如“目标、规则、当前证据、可取回证据、输出预算、验收标准”六栏？
- 涉及长期记忆时，是否需要单独设置一页隐私与权限边界？
