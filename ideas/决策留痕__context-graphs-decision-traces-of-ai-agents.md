# 决策留痕：让 agent 的判断可追溯、可复用、可审计

## 状态

candidate

## 一句话主张

长程任务里，agent 需要把关键决策、依据、相似先例、工具调用、因果顺序和人工确认写成可检索的“决策轨迹”，让后续任务有据可查。

## 适用角色

跨角色；首选平台运营工程师、数据分析师，也适用于税务管理员的口径审核、程序员的代码评审记录、算法工程师的实验结论复盘。

## 工作场景

平台运营工程师处理复杂客户工单或风控告警：agent 读取 CRM、历史工单、内部规则和业务数据，找出相似案例，提出“升级处理、拒绝、补偿、人工复核”等建议，并留下完整决策轨迹。

场景来源说明：该场景由 PDF 的 financial services agent、support ticket system、CRM、Jessica reject/escalate demo 改写为本项目目标听众中的平台运营工单场景。

## Agent 协作方式

- Agent 负责检索材料、抽取实体和关系、寻找相似先例、生成建议、标注依据、记录工具调用与推理步骤。
- 人类负责定义领域规则，确认高风险节点，批准最终决策，并抽查 agent 留下的轨迹质量。
- “决策轨迹”可以理解成工单卷宗里的办案记录：谁基于哪些材料、参考哪些先例、经过哪些步骤、做出什么判断、结果是否可靠。

## 长程任务设计要点

- 目标：每个关键判断都能回答“依据是什么、先例在哪里、下一步谁负责”。
- 资料：CRM、支持工单、内部政策、历史决策、业务表格、聊天记录。
- 计划：定义 ontology 与抽取映射 -> 导入材料成上下文图 -> 检索相似先例 -> 生成建议与证据 -> 人工确认 -> 回写决策轨迹。
- 记忆：短期记当前工单上下文，长期保存已确认 decision traces、质量分、过期状态和人工确认结果。
- 工具：CRM/工单系统、Neo4j Context Graph/GDS、OpenAI embeddings、Claude/FastAPI agent，以及 `find_precedents`、`get_causal_chain`、`record_decision` 这类 MCP 工具。
- 上下文图：把客户、工单、规则、事件、决策、工具调用连成图，数学骨架可写作 \(G=(V,E)\)，其中 \(V\) 是节点，\(E\) 是关系。
- 检索方式：同时看文本含义和结构相似度。文本含义像“描述相似”，结构相似像“关系网络相似”。PDF 给出的组合评分形式是 \(S(q,T_d)=\alpha\,sim_{sem}(q,T_d)+(1-\alpha)\,sim_{struct}(q,T_d)\)。
- 决策链：把步骤、因果关系、时间戳写入轨迹，可抽象为 \(T_d=(S_d,E_d^{trace},\tau)\)，其中 \(S_d\) 是步骤集合，\(E_d^{trace}\) 是 `caused` / `next` 这类关系，\(\tau\) 是时间或质量相关信息。
- 检查点：资料抽取后检查实体；先例检索后检查相似理由；最终建议前检查风险；人工批准后把决策轨迹回写。
- 验收标准：建议必须包含来源、至少 2 个相似先例、相似理由、风险边界、人工确认状态、可复查的决策轨迹。

## 来源依据

- `resources/context-graphs-decision-traces-of-ai-agents.pdf` 明确内容：目录和正文围绕 context graph、CRM demo、Jessica 的 reject decision、find precedents、semantic similarity、structural similarity、create-context-graph、neo4j-agent-memory、Q&A 中的 `caused` / `next` 与 decision traces 展开。
- `resources/context-graphs-decision-traces-of-ai-agents.pdf` 明确内容：PDF 给出 \(G=(V,E)\)、混合相似度评分 \(S(q,T_d)\)、agent memory \(M=M_s\cup M_l\cup R\)、决策轨迹 \(T_d=(S_d,E_d^{trace},\tau)\) 等结构表达。
- `resources/context-graphs-decision-traces-of-ai-agents.pdf` 可核图示/结构：Figure 6 / Context Graph Demo 图支持数据源、后端和前端架构，包含 support ticket system、CRM、internal business data，后端包含 Claude agent、OpenAI embeddings、Neo4j Context Graph/GDS。
- `resources/context-graphs-decision-traces-of-ai-agents.pdf` 可核图示/结构：Hybrid Search 图把 text embeddings 用于语义相似，把 graph embeddings / FastRP 用于结构相似，合起来寻找相似场景下的先例决策。
- `resources/context-graphs-decision-traces-of-ai-agents.pdf` 可核图示/结构：Figure 11 / GDS MCP 工具图支持 `find_precedents`、semantic/structural similarity、因果链等工具化检索能力。
- `resources/context-graphs-decision-traces-of-ai-agents.pdf` 可核图示/结构：Figure 19 / 闭环图支持“材料、ontology、上下文图、智能体决策、决策轨迹回写”的闭环。
- 分析推断：面向普通工作者时，最有 PPT 价值的表达应聚焦“让 agent 的判断可追溯、可复用、可审计”，技术名词只作为支撑解释。

## 风险与边界

- 决策轨迹会记录客户、工单、政策和工具调用，必须先设权限、脱敏和审计规则。
- 坏先例也会被复用；团队需要质量分、人工复核和过期机制。
- ontology 和关系类型设计会影响检索质量，关系建错会让相似案例看起来很可信。
- PDF 的 Q&A 表明，如何稳定写入新的 decision traces 仍在探索，落地时应先从模板化记录开始。
- 相似度公式里的权重 \(\alpha\) 会影响结果，业务团队需要定期抽样检查。

## PPT 使用建议

- 适合放在“长程任务方法”页，也可放在“平台运营工程师”案例页。
- 页面主图建议画成闭环：材料与规则 -> 上下文图 -> agent 找先例并给建议 -> 人类确认 -> 决策轨迹回写。
- 讲法：没有留痕的 agent 像只给结论的同事；有决策轨迹的 agent 像会把卷宗、依据卡、相似案例和责任节点一并交出来的协作者。
- 选择理由：比起单讲 RAG，这个 idea 更能解释长程任务里的复用、审计和纠错；比起讲完整图数据库架构，它更贴近普通工作者明天能采用的流程。

## 待确认问题

- PPT 里是否用“平台运营工单”作为主例，还是改成“税务口径审核”以强化合规感？
- 团队希望决策轨迹记录到什么粒度：只记最终建议，还是记录工具调用、草稿、人工驳回和修改原因？
- 高风险决策是否需要设定强制人工批准节点？
