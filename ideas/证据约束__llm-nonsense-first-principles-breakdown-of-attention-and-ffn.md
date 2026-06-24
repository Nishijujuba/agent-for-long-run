# 证据约束：让 Agent 的回答能被验收

## 状态

candidate

## 一句话主张

大模型的回答来自上下文加权、内部知识变换和 next-token 续写；普通工作者使用 agent 做长程任务时，要把关键结论绑定到检索证据、工具结果、结构化校验和人工检查点上。

## 适用角色

跨角色。PPT 中建议以数据分析师为主例，税务管理员、程序员、平台运营工程师可作为旁例。

## 工作场景

数据分析师让 agent 生成一份“本月经营指标异常解释”报告。材料包括指标口径文档、数据库 schema、历史报表、SQL 查询结果、业务规则和会议记录。

该场景为基于 PDF 中 RAG、Tool Use 与 SQL 生成/校验结构的应用转译，PDF 原文没有直接给出“本月经营指标异常解释”案例。

如果 agent 直接生成报告，它可能写出顺畅解释，也可能引用错口径、生成错 SQL、把旧结论当成新事实。更稳的做法是让 agent 先检索口径与 schema，再生成只读 SQL，执行或校验查询结果，最后输出带证据编号、查询记录和不确定性说明的报告草稿。

## Agent 协作方式

- 人类定义目标、证据范围、敏感数据边界和验收标准，例如“每个异常解释必须对应指标口径、SQL 结果或会议记录 ID”。
- Agent 先用 RAG 检索相关口径、历史结论和业务规则，形成证据清单。
- Agent 生成查询计划和只读 SQL，并通过语法检查、schema 对齐、样例结果检查来减少错误。
- Agent 把输出整理成结构化结果，例如 `claim / evidence_id / query_id / confidence / needs_human_review`。
- 人类在关键节点介入：确认口径、批准查询计划、抽查结果样本、判断业务解释是否成立。
- 高风险结论进入人工复核，例如税务、合规、医疗、生产系统操作和对外发布内容。

## 长程任务设计要点

- 目标：把“让 agent 回答问题”改成“让 agent 交付可追踪的证据链”。
- 资料：将 PDF、制度文件、数据库 schema、日志、会议记录和历史报告登记为可引用来源，保留文件名、页码、时间和权限边界。
- 计划：先做证据检索，再做工具调用，随后写报告，最后做人工验收。避免让 agent 一步跳到最终结论。
- 检查点：口径检查、查询检查、证据检查、结论检查、风险检查。
- 记忆：把确认过的口径、数据版本、查询 ID、人工判断写入可恢复记录；过期数据需要失效标记。
- 工具：优先使用只读查询、搜索、计算、格式校验、schema 校验和引用检查。写入、发送、删除、发布、部署需要审批。
- 验收标准：每个核心结论都有来源 ID；SQL 与 schema 匹配；工具结果可复现；结构化输出能通过校验；不确定结论被明确标注；高风险内容有人类签字。
- 直观公式：

\[
A_{\text{可信交付}} =
f(E_{\text{检索证据}}, T_{\text{工具结果}}, S_{\text{结构校验}}, H_{\text{人工判断}})
\]

含义是：agent 交付物的可信度来自证据、工具、结构和人的共同约束。模型本身生成的是概率续写，概率高的句子并不自动等于事实。

## 来源依据

- `resources/llm-nonsense-first-principles-breakdown-of-attention-and-ffn.pdf` - PDF 明确内容：封面主题为“拆解 Attention + FFN，3 分钟看懂幻觉本质”，并标注 RAG / Agent 补缺逻辑与第一性原理推导。
- `resources/llm-nonsense-first-principles-breakdown-of-attention-and-ffn.pdf` - PDF 明确内容：Transformer 部分用 Beats Music 与 Apple 的例子解释 Attention 与 FFN/MLP 的分工；Attention 公式为 \(y_i=\sum_j a_{ij}v_j\)，以及 \(\operatorname{Attention}(Q,K,V)=\operatorname{softmax}(QK^\top/\sqrt{d_k})V\)。
- `resources/llm-nonsense-first-principles-breakdown-of-attention-and-ffn.pdf` - PDF 明确内容：FFN/MLP 部分给出 \(FFN(x)=W_2\sigma(W_1x+b_1)+b_2\)，并把 FFN/MLP 与知识提取、向量变换联系起来。
- `resources/llm-nonsense-first-principles-breakdown-of-attention-and-ffn.pdf` - PDF 明确内容：Next-token prediction 部分讨论 token、logits、softmax、概率分布、greedy decoding、sampling 和 temperature，说明生成过程以预测下一个 token 为核心。
- `resources/llm-nonsense-first-principles-breakdown-of-attention-and-ffn.pdf` - PDF 明确内容：RAG 被展开为 Retrieval-Augmented Generation，结构包含 retrieval、reranking、generation、embedding index、vector database，并给出 top-k 检索公式 \(r^*=\arg\operatorname{topk}_{r\in D} sim(e(q),e(r))\)。
- `resources/llm-nonsense-first-principles-breakdown-of-attention-and-ffn.pdf` - 可核图示/结构：RAG 图示写明“用外部证据约束生成”，流程为用户问题、查询向量、向量库、召回 Top-k、重排、证据注入、模型生成带来源回答，目标是减少新知识缺口与事实幻觉。
- `resources/llm-nonsense-first-principles-breakdown-of-attention-and-ffn.pdf` - 可核图示/结构：Tool Use 页面写明工具模式让大模型调用外部插件或工具，用于计算、搜索和对接系统等任务。
- `resources/llm-nonsense-first-principles-breakdown-of-attention-and-ffn.pdf` - PDF 明确内容：Agent 部分给出 `Agent = LLM + memory + planning + tools`，并讨论 API、memory、planning、tool use、context window 等要素。
- `resources/llm-nonsense-first-principles-breakdown-of-attention-and-ffn.pdf` - 可核图示/结构：Agent 落地短板图列出可控性弱、格式不稳、状态缺失、上下文有限、效率偏低、成本偏高，并把工程约束指向日志、重试、结构化输出、评测集与成本预算。
- `resources/llm-nonsense-first-principles-breakdown-of-attention-and-ffn.pdf` - 可核图示/结构：SQL 生成图把任务拆成训练样本、schema 输入、SQL 生成、语法检查、执行校验和结果复核，适合转译为数据分析场景的验收流程。
- `resources/llm-nonsense-first-principles-breakdown-of-attention-and-ffn.pdf` - 可核图示/结构：医疗问答 RAG 图强调高风险边界，价值来自证据接入，风险控制来自审计、出处、拒答与人工复核。
- `resources/llm-nonsense-first-principles-breakdown-of-attention-and-ffn.pdf` - 分析推断：面向普通工作者时，最适合进入 PPT 的提炼是“证据约束”。它把 Attention、FFN、next-token prediction、RAG、Tool Use、Agent 和 SQL 校验串成一个实践方法：让 agent 的关键产出可引用、可复现、可验收。

## 风险与边界

- RAG 能降低无依据生成风险，也可能检索到过期、片面或权限不当的材料。检索结果需要来源、时间和适用范围。
- 工具调用能提升可验证性，也会引入权限风险。生产系统、税务合规、客户数据、医疗建议和对外发布必须设置审批。
- 结构化输出只能保证格式较稳定，无法单独保证事实正确。JSON schema 通过后，仍要检查证据和业务含义。
- SQL 生成需要只读权限、schema 校验、样例结果检查和人工复核。错误 SQL 可能带来错误结论，写操作还可能造成业务事故。
- 长程任务中，agent 可能丢失早期约束、复用过期记忆或忽略反例。关键结论需要证据 ID 和检查点。
- 这个 idea 适合讲工作方法。Attention、FFN、softmax 公式只作为“为什么回答顺畅也要验收”的直觉支撑，PPT 不宜展开模型架构细节。

## PPT 使用建议

适合放在第 3 页“人负责目标、判断和责任；agent 负责检索、生成、执行和自检”，也可放在第 9 页“长程任务方法”。

建议一页只讲一个主论点：会说和可验收是两件事。

页面结构可以这样设计：

左侧用简图解释模型生成链路：

上下文与内部知识 -> Attention/FFN -> next-token prediction -> 顺畅回答

右侧画“证据约束链”：

检索证据 -> 工具执行 -> 结构化输出 -> 人工检查点 -> 可验收交付

讲解例子采用数据分析师月报：agent 先找指标口径，再生成只读 SQL，再把每条解释绑定到证据和查询结果。类比可以用“报销单”：没有票据的金额不能直接入账；没有证据 ID 的结论也不能直接进入报告。

## 待确认问题

- PPT 主例是否采用数据分析师月报，还是换成税务管理员的政策口径整理？
- “证据约束”这个中文标题是否足够直观，或改成“带票据的回答”“可验收回答”？
- Attention 和 FFN 公式是否放进 PPT 正文，还是只放进 speaker notes？
- 高风险边界是否单独展开一页，覆盖税务、合规、隐私和生产系统操作？
