# 思考预算：让 agent 在目标、预算和检查点内分配工作

## 状态

candidate

## 一句话主张

长程 agent 任务的关键做法，是把目标、资料、工具权限、时间/token/成本预算和验收标准写清，让 agent 在 thinking、tool calling、text 三类动作之间自适应分配计算，并用 eval 与检查点校准质量。

## 适用角色

跨角色，尤其适合程序员、数据分析师、算法工程师、平台运营工程师；税务管理员也可借用该方法做政策检索、口径整理和风险提示，但最终业务判断必须由人承担。

## 工作场景

平台运营工程师处理一个反复出现的服务异常工单：agent 读取告警、日志、部署记录和历史工单，提出根因假设，执行只读检查，整理证据链、处置建议和复盘初稿。人类在涉及生产操作、责任归因和对外结论时做最终判断。

## Agent 协作方式

人类先设定边界：目标是“找出最可能根因并给出处置建议”，资料范围是日志、监控、变更记录和历史工单，工具范围先限制为只读，时间上限为 45 分钟，中间每 15 分钟汇报一次，输出必须包含证据、假设、验证状态和未验证风险。

Agent 在边界内动态调度三类动作：thinking 用于形成假设和检查矛盾，tool calling 用于读取日志、搜索历史工单和查询监控，text 用于阶段汇报、追问和最终交付。可用一个成本分账直觉表示：

\[
n_{\text{total}} \approx n_{\text{think}} + n_{\text{tool}} + n_{\text{text}}
\]

其中 \(n_{\text{think}}\) 是推理草稿消耗，\(n_{\text{tool}}\) 是工具调用和工具结果处理消耗，\(n_{\text{text}}\) 是面向人的输出消耗。长程任务还要满足预算约束：

\[
\max Q \quad \text{s.t.}\quad n_{\text{total}}\le B_n,\ L\le B_L,\ C\le B_C
\]

这里 \(Q\) 表示任务质量，\(L\) 表示延迟或运行时间，\(C\) 表示综合成本，\(B_n,B_L,B_C\) 分别表示 token、时间和费用预算。

## 长程任务设计要点

- 目标：明确最终交付物，例如“根因候选排序、证据链、处置建议、复盘初稿”。
- 资料：列清可读取材料，区分日志、监控、变更记录、历史工单和人工经验。
- 计划：先建立假设，再读证据，再验证，再输出结论；每一步都保留可回溯记录。
- 检查点：需求澄清、证据清单、根因假设、验证结果、最终建议都应有人工确认或抽检。
- 记忆：把关键假设、已排除路径、证据链接和待验证项写入文件或任务记录，避免长任务中丢上下文。
- 工具：默认只读；涉及生产变更、权限扩大、对外发送和删除操作时必须停下等人确认。
- 预算：effort 表示投入倾向，budget 表示硬边界；复杂、高风险任务可在小样本 eval 中测试 extra high 或 max 是否划算，批量低风险任务可从 low/medium 起测。
- 验收标准：结论有来源，失败类型有记录，关键风险有标注，成本和延迟在预算内，未验证内容清楚标为待确认。

## 来源依据

- `resources/thinking-lever-claude-adaptive-reasoning-cost-latency-quality-tradeoffs.pdf` 明确内容：PDF 将测试时计算拆成 thinking、tool calling、text 三类消耗，并给出 \(n_{\text{total}} \approx n_{\text{think}} + n_{\text{tool}} + n_{\text{text}}\) 的教学近似。
- `resources/thinking-lever-claude-adaptive-reasoning-cost-latency-quality-tradeoffs.pdf` 明确内容：PDF 区分 effort 与 budget，说明 effort 是投入倾向，budget 是 token、时间、费用等资源边界。
- `resources/thinking-lever-claude-adaptive-reasoning-cost-latency-quality-tradeoffs.pdf` 明确内容：PDF 说明 adaptive thinking 允许模型在 think、tool、text 之间动态选择动作顺序，适合工具反馈驱动任务。
- `resources/thinking-lever-claude-adaptive-reasoning-cost-latency-quality-tradeoffs.pdf` 明确内容：PDF 强调用 eval 在真实任务上比较质量、token、延迟和成本，并提示高 effort 存在边际收益递减。
- `resources/thinking-lever-claude-adaptive-reasoning-cost-latency-quality-tradeoffs.pdf` 明确内容：PDF 最后把全篇压缩为四层控制面：thinking 是能力，effort 是倾向，budget 是边界，eval 是校准。
- 注：当前 PDF 中文正文存在字体映射和抽取缺字，以上核查主要依据可读英文术语、公式、图表和可辨认页面结构。
- 分析推断：该 PDF 最适合转化为“长程任务资源调度”这一 PPT idea，因为它能把抽象的 token、effort 和 eval 翻译成普通工作者可执行的任务委托规则。
- 分析推断：平台运营工单、代码迁移、数据报告和税务口径整理都可套用这一框架；其中平台运营工单最能体现工具调用、证据验证、预算限制和人工检查点。

## 风险与边界

- PDF 是课程笔记/视频总结材料，未等同于 Anthropic 官方接口文档；其中公式主要是教学抽象。
- “更多 effort 通常提升质量”需要任务条件支持；简单抽取、格式转换和低风险分类可能只会增加延迟和成本。
- 预算太紧会让 agent 在关键推理或验证前停止；预算太松会让 agent 在低价值分支上消耗资源。
- eval 必须使用真实困难样本，简单样本容易掩盖长程任务里的失败模式。
- 涉及税务、合规、隐私、生产系统操作和对外承诺时，agent 只能辅助整理、检查和建议，人类仍承担最终判断与责任。

## PPT 使用建议

适合放在第 9 页“长程任务方法”。页面主论点可写成：“长程任务要管理 agent 的思考预算”。配图建议使用四步闭环：

用户约束（时间 / token / 成本 / 质量目标） -> 自适应分配（thinking / tool calling / text） -> 受约束执行 -> eval 与反馈校准。

页面下方用一句通俗类比收束：agent 像接到任务的同事，不能只说“认真做”，还要给目标、资料、权限、截止时间、检查点和验收标准。

若进入最终 PPT，优先保留四步闭环图；公式适合进入 speaker notes，避免正文同时出现 \(n_{\text{total}}\) 与预算约束造成解释负担。

## 待确认问题

- 该 idea 在最终 PPT 中是否作为“长程任务方法”总框架使用？
- 角色案例是否优先采用平台运营工单，还是改成程序员代码迁移、数据分析月报或税务政策口径整理？
- PPT 是否保留 \(n_{\text{total}}\) 与预算约束公式，还是改成完全口语化的“草稿纸、工具费、汇报篇幅”类比？
