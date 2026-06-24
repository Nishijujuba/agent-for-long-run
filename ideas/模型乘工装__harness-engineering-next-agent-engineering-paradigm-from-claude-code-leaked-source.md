# 模型乘工装：把 Agent 能力做成可复用任务工装

## 状态

candidate

## 一句话主张

Agent 的稳定产出来自模型能力和任务工装共同作用：普通团队提升长程任务质量的关键抓手，是把资料、工具、权限、检查点、记忆和验收标准做成可复用的 harness。

## 适用角色

跨角色。最适合数据分析师、程序员、平台运营工程师和算法工程师；税务管理员也可用于政策检索、口径整理、材料清单和风险提示等低权限辅助任务。

## 工作场景

以数据分析师制作月度经营分析报告为例。团队每月都要读取指标口径、BI 数据、上期报告、会议纪要和异常工单，再输出图表、异常解释、行动建议和待确认问题。

若每次只把任务丢给一个聊天窗口，质量会高度依赖当次提示词和模型状态。更稳的做法是把这个月报任务做成 harness：固定数据源、指标定义、只读查询工具、报告模板、异常检查清单、人工审批点和历史问题记录。这样 agent 每次进入的都是同一间“有工位、有工具、有规程、有交接本”的工作间。

## Agent 协作方式

- 人类定义目标、边界和责任：报告服务谁，哪些指标必须核对，哪些数据不能进入长期记忆，哪些结论需要负责人确认。
- Harness 定义观察空间 \(O\)：agent 能看到的数据表、指标字典、上期报告、会议纪要、历史异常和业务规则。
- Harness 定义动作空间 \(A\)：agent 能执行只读查询、生成图表草稿、写报告草稿、列待确认问题；外发报告、改生产数据、更新正式口径需要人工批准。
- Agent 执行循环：读取资料，提出分析计划，调用工具，记录证据 ID，生成草稿，按检查清单自检，再把高风险判断交给人类。
- Side query 或子 agent 处理支线问题：例如单独核对某个异常指标、比对两期口径、检查图表数字是否和 SQL 输出一致。
- 复核 agent 或人工 reviewer 做验收：看结论是否有来源、图表是否可复算、异常解释是否过度推断、隐私边界是否被突破。

## 长程任务设计要点

- 核心公式：可把 agent 质量粗略写成 \(Q_{\text{agent}} \approx M_{\text{model}} \times H_{\text{harness}}\)。\(M\) 是模型能力，\(H\) 是任务工装质量；任一项太低都会拉低结果。
- Prompt 与 Context：把稳定规则、角色目标、输出格式、数据口径和风险边界放入固定上下文，减少每月重新解释。
- Observation space 与 Action space：明确 agent 能看什么、能做什么。能看包括数据、文档、日志和历史报告；能做包括查询、汇总、制图、草拟和自检。
- 记忆：用 Markdown 或结构化文件保存历史口径、已确认判断、常见异常、报告模板和证据 ID。类比为团队的交接本。
- Prompt caching：把稳定规则和工具 schema 固定下来，让高频任务减少重复成本和上下文结构漂移风险。
- Side query：把支线验证从主循环里拆出去。主 agent 只接收结论、证据和未解决问题，避免主上下文被细节拖满。
- 权限与安全：默认只读；写入、外发、删除、部署、修改生产口径等动作需要审批。安全 hook 用于拦截高风险命令或敏感信息外流。
- Evaluation 与 ablation：为报告设置验收清单，例如关键数字可复算、指标口径一致、异常解释有证据、待确认项明确。改动 harness 后要比较质量变化。
- 反馈闭环：把使用反馈、错误样例和人工修正沉淀为 bug backlog，再决定更新模型提示、工具、权限、记忆或验收标准。可抽象为：

\[
\text{Usage} \rightarrow D_1 \rightarrow \text{Bug Triage} \rightarrow \{\text{Model Update}, \text{Harness Update}\} \rightarrow \text{Better Agent}
\]

## 来源依据

- `resources/harness-engineering-next-agent-engineering-paradigm-from-claude-code-leaked-source.pdf` - PDF 明确内容：目录显示材料从 Claude Code/OpenCode 对比进入 Harness Engineering，随后展开 Prompt、Context、Harness、Observation space、Action space、Prompt caching、side query、Markdown、Agent 安全、Sub-agent、Evaluation/Ablation、GUI/Context/AI-native Organization 和 “Model × Harness = Agent”。
- `resources/harness-engineering-next-agent-engineering-paradigm-from-claude-code-leaked-source.pdf` - PDF 明确内容：正文抽取到状态转移表达 \(S_{t+1}=F(S_t,o_t,a_t)\)，并把 \(o_t\) 解释为 observation、\(a_t\) 解释为 action，说明 harness 关心 agent 能观察什么和能执行什么。
- `resources/harness-engineering-next-agent-engineering-paradigm-from-claude-code-leaked-source.pdf` - PDF 明确内容：第 6 章标题为 “Model × Harness = Agent”，正文抽取到 \(Q \approx M \times H\)，并出现 bug backlog、first-party data、Harness update、Model update 等反馈闭环元素。
- `resources/harness-engineering-next-agent-engineering-paradigm-from-claude-code-leaked-source.pdf` - 可核图示/结构：嵌入截图“Agent 的核心公式：Agent = Model × Harness”列出 Claude Code 中的 harness 全貌，包括 environment、prompt cache、CLAUDE.md、MCP、tools、hook、shell、permission mode 等。
- `resources/harness-engineering-next-agent-engineering-paradigm-from-claude-code-leaked-source.pdf` - 可核图示/结构：嵌入截图“范式演进：Prompt ⊂ Context ⊂ Harness”把 agent 发展从提示词、上下文工程推进到 harness 工程，并用 LangChain、OpenAI Agents、Claude Code 等作为对照案例。
- `resources/harness-engineering-next-agent-engineering-paradigm-from-claude-code-leaked-source.pdf` - 可核图示/结构：嵌入截图包含 Prompt Cache、五层上下文压缩、Side Query、Markdown 记忆架构，说明长程任务需要稳定上下文、支线查询和可恢复记忆。
- `resources/harness-engineering-next-agent-engineering-paradigm-from-claude-code-leaked-source.pdf` - 可核图示/结构：安全相关截图包含 fail-closed、LLM 权限分类、PreToolUse/stop hooks、Shell 安全命令检查、工具锁和 capability-based subagent，说明 harness 也承担权限与风险控制。
- `resources/harness-engineering-next-agent-engineering-paradigm-from-claude-code-leaked-source.pdf` - 可核图示/结构：evaluation/ablation 截图展示把研究方法做成产品工程，包括 baseline、feature flag、GrowthBook、fake tools 和反蒸馏等机制。
- `resources/harness-engineering-next-agent-engineering-paradigm-from-claude-code-leaked-source.pdf` - 可核图示/结构：后段截图“应用层公司的护城河需要在技术之外”强调应用层优势来自 first-party data、用户反馈、bug backlog、harness 更新和组织上下文。
- `resources/harness-engineering-next-agent-engineering-paradigm-from-claude-code-leaked-source.pdf` - 分析推断：数据分析师月报场景是面向本项目听众的应用迁移，PDF 原材料主要围绕 Claude Code、OpenCode 和 agent 工程。
- `resources/harness-engineering-next-agent-engineering-paradigm-from-claude-code-leaked-source.pdf` - 分析推断：“任务工装”是对 harness 的中文工作化表达，强调普通团队可操作的资料、工具、权限、记忆、检查点和验收标准。

## 风险与边界

- Harness 质量差会把错误流程固化。若指标口径、权限规则或报告模板有错，agent 会稳定地产出同类错误。
- First-party data 有隐私和合规风险。用户反馈、客户数据、生产日志、税务材料和业务秘密不能无边界进入长期记忆。
- Prompt cache 和 Markdown 记忆会复用旧信息。涉及政策、价格、接口、组织权限和生产数据时，需要版本、日期和失效规则。
- 权限过宽会放大误操作风险；权限过窄会让 agent 只能生成空泛草稿。需要按任务风险分级配置只读、写入、外发、删除、部署等权限。
- Evaluation 可能被表面指标误导。格式正确、语气专业、图表漂亮，仍然可能掩盖口径错误或证据不足。
- 税务、合规、生产系统和对外发布场景必须保留人类最终判断。Agent 可整理证据、提出建议和生成草稿，责任归属仍在授权负责人。

## PPT 使用建议

适合放在第 9 页“长程任务方法”，也可放在第 3 页“人和 agent 分工框架”之后，作为“为什么长程任务要设计工作流”的总括页。

建议一页只放一个公式和一张图：

\[
Q_{\text{agent}} \approx M_{\text{model}} \times H_{\text{harness}}
\]

图示左侧是“模型能力 \(M\)”，右侧是“任务工装 \(H\)”；任务工装下方放六个模块：上下文、工具、权限、记忆、验收、反馈。页面主句可以写成：先把任务工装搭好，再让 agent 跑长程任务。

讲解类比：请一位临时同事做月报，不能只说“帮忙分析一下”。团队要给他数据入口、口径说明、报告模板、能按哪些按钮、什么时候停下问负责人、最后按什么清单验收。Agent 也是如此。

Evaluation/Ablation、first-party data 和 bug backlog 适合放入 speaker notes，正文保留公式、月报类比和六模块图。

## 待确认问题

- PPT 中是否采用“任务工装”作为 Harness 的中文讲法，还是沿用已有 idea 的“工作台与护栏”？
- 主案例是否使用数据分析师月报，还是换成平台运营排障、程序员缺陷修复、税务政策口径整理？
- 是否把 \(Q_{\text{agent}} \approx M_{\text{model}} \times H_{\text{harness}}\) 放在正文页，还是只放入 speaker notes？
- 是否单独展开 first-party data 和 bug backlog 反馈闭环，还是作为本页角落的一条补充说明？
