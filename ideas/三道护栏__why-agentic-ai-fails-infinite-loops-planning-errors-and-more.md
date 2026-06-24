# 长程 agent 要先装三道护栏

## 状态

candidate

## 一句话主张

很多 agent 失败源于系统约束缺口：长程任务要先设计停止门、计划验证门和权限审批门，再让 agent 循环执行。

## 适用角色

跨角色。PPT 中建议以平台运营工程师为主例，税务管理员、程序员、数据分析师、算法工程师可作为旁例。

## 工作场景

平台运营工程师处理一次线上告警：让 agent 使用监控查询、日志检索、工单系统、变更系统和只读诊断命令，形成排查计划，输出原因假设和修复建议。凡是涉及重启服务、修改配置、发送通知、关闭告警、删除记录的动作，都必须进入人工审批。

这个场景适合普通工作者理解，因为它同时包含资料检索、计划拆解、工具调用、循环重试、业务风险和人工判断。

该平台运营告警场景是面向 PPT 听众的应用改写。PDF 原例主要包括旅行 API、季度报告、删记录和自动邮件等 agent 失败场景。

## Agent 协作方式

Agent 负责：

- 观察：收集告警、日志、近期变更、历史工单。
- 计划：把排查拆成可执行步骤，并标明每一步需要的工具和数据。
- 执行：优先执行只读、可回滚、低风险动作。
- 评估：比较每轮结果是否带来新信息，例如用 \( \Delta_t = score(r_t) - score(r_{t-1}) \) 表示本轮进展。
- 停止或求助：达到最大轮数、进展为零、权限不足、信息矛盾时，停止并向人类报告。

人类负责：

- 定义目标、范围、风险等级和验收标准。
- 审核 agent 的计划是否真的可执行。
- 批准写入、发送、删除、发布、支付、部署等高风险动作。
- 对最终业务判断和生产系统后果负责。

## 长程任务设计要点

可以把可靠 agent 简化为：

\[
R_{\text{agent}} = f(B_{\text{stop}}, V_{\text{plan}}, P_{\text{perm}}, M_{\text{track}})
\]

其中：

- \(B_{\text{stop}}\)：停止边界。包含最大重试次数、最大步骤数、最大运行时间、无进展停止规则。
- \(V_{\text{plan}}\)：计划验证。计划中的每个动作都要匹配已有工具能力、工具 schema 和业务约束。
- \(P_{\text{perm}}\)：权限控制。读取、写入、删除、发布属于不同风险层级，越接近不可逆动作，越需要审批。
- \(M_{\text{track}}\)：监控与记录。保留工具调用日志、计划验证日志、审批日志、错误类型和成本记录。
- 验收标准：每轮输出证据来源、已执行动作、未执行高风险动作、根因假设和下一步建议；超过最大轮数或 \(\Delta_t \le 0\) 时停止；任何写入、删除、发送、发布、部署动作都需要审批记录。

直观类比：agent 像一个会跑腿的实习同事。给他一张任务单还不够，还要写清楚什么时候停、哪些门不能进、哪些动作要请示、每一步要留下记录。少了这些护栏，agent 会把同一个错误重复很多次，成本和风险会一起放大。

## 来源依据

- `resources/why-agentic-ai-fails-infinite-loops-planning-errors-and-more.pdf` - PDF 明确内容：agentic system 被描述为带工具的 LLM，运行模式包含 observe、plan、act/tool use、evaluate、retry/stop 的循环。
- `resources/why-agentic-ai-fails-infinite-loops-planning-errors-and-more.pdf` - PDF 明确内容：Infinite Loop 部分指出失败表现是 search、evaluate、plan、retry 反复发生且没有 meaningful progress；对应护栏包括 proper termination condition、action tracking、progress tracking、max retries、max steps、max runtime usage。
- `resources/why-agentic-ai-fails-infinite-loops-planning-errors-and-more.pdf` - PDF 明确内容：Hallucinated Planning 部分强调 plausible versus possible，风险来自 tool capabilities 未定义、计划与执行未分离、缺少 validation gate、未检查 constraints；对应方法包括 tool schema、verifier agent、human in the loop、ask for clarification。
- `resources/why-agentic-ai-fails-infinite-loops-planning-errors-and-more.pdf` - PDF 明确内容：Unsafe Tool Use 部分指出动作在形式上可执行，也可能带来 risky、destructive、unintended 后果；对应方法包括 approval workflow、overprivileged tools 控制、read/write/delete access tiers、principle of least agency。
- `resources/why-agentic-ai-fails-infinite-loops-planning-errors-and-more.pdf` - PDF 明确内容：总结页把三类失败模式对应到 termination condition/action tracking/progress tracking、tool schema/constraint validation/verifier agent/human in the loop、least agency/approval workflow/read-write-delete access tiers。
- `resources/why-agentic-ai-fails-infinite-loops-planning-errors-and-more.pdf` - 分析推断：面向 8 分钟 PPT 时，最适合压缩成一页的表达是“三道护栏”，即停止门、计划验证门、权限审批门。这样比逐条讲 bug 更容易让普通工作者明天直接套用。
- `resources/why-agentic-ai-fails-infinite-loops-planning-errors-and-more.pdf` - 分析推断：平台运营工程师主例和“三道护栏”表达均为演示场景压缩，并非 PDF 逐字结论。

## 风险与边界

- 这个 idea 适合讲方法，无法证明某个具体业务系统一定安全。不同组织的权限、审计、合规要求差异很大。
- “最小代理权”需要解释清楚：它接近最小权限原则，重点是让 agent 只拥有完成当前步骤所需的行动能力。读取、写入、删除、发布的风险级别差异很大。
- 平台运营、税务、财务、法务、人事等场景涉及隐私、合规和责任归属。Agent 可以整理材料和提出建议，最终判断应由人类或制度授权的负责人承担。
- 计划验证门不能只看文字是否顺畅，还要检查工具是否真的存在、输入输出 schema 是否匹配、业务约束是否满足。
- 监控日志本身也可能包含敏感信息，需要权限隔离、脱敏和留存周期控制。

## PPT 使用建议

适合放在第 9 页“长程任务方法”，也可放在风险边界页。

建议画成一张三门流程图：

任务目标 -> 停止门 -> 计划验证门 -> 权限审批门 -> 执行与记录 -> 人类验收

每道门配一句话：

- 停止门：什么时候继续，什么时候停下求助。
- 计划验证门：计划看起来合理，还要检查是否可执行。
- 权限审批门：读、写、删、发、部署分层处理。

页面只讲一个主论点：长程 agent 的可靠性来自工程化约束。公式 \(R_{\text{agent}} = f(B_{\text{stop}}, V_{\text{plan}}, P_{\text{perm}}, M_{\text{track}})\) 可放在页脚，作为记忆钩子，避免讲成算法课。

## 待确认问题

- PPT 主例是否采用平台运营工程师，还是换成更贴近日常办公的“税务政策检索与材料清单整理”？
- “principle of least agency”在中文里采用“最小代理权”“最小行动权”还是直接解释为“只给 agent 当前步骤必需的权限”？
- 公式是否进入 PPT 正文，还是只放进 speaker notes？
