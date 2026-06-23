# 先做任务契约：让 Agent 带着资料追问，人类负责取舍

## 状态

confirmed

## 一句话主张

长程任务开工前，agent 最有价值的角色不是直接执行，而是带着资料把模糊需求拷问成可验收契约；价值取舍、合规判断、产品 taste 必须留在人类在环里。

## 适用角色

跨角色，尤其适合税务管理员、平台运营工程师、产品/工程协作场景。

## 工作场景

用户说“帮我做一个功能”“帮我整理政策口径”“帮我生成一份数据报告”。这些话听起来明确，实际隐藏了很多待拍板问题：适用地区、生效日期、指标口径、风险边界、上线范围、是否需要兼容历史数据、哪些行为算完成。

## 讲给 PPT agent 的内容说明

这一页要强调：agent 不应把模糊 brief 当成执行指令。更好的做法是 Grill with docs：agent 先读资料和代码，再一次只问一个关键问题，并给出推荐答案。推荐答案的价值在于降低人类认知负担；人类可以接受、修改或否定它。

问题要有证据来源和业务含义，不能只是“你想怎么做”。一条好问题应该包含：当前材料里看到的事实、需要人类拍板的分叉、推荐选择、选择后的影响。

## 实际例子

课程笔记中的 Cadence gamification 案例很适合讲这一页。Agent 问：积分经济里哪些行为应该获得积分，分值是多少？Matt 让它先保持简单，只保留两个积分来源，并明确排除 video watch events，因为观看视频噪声大且容易被刷。

接着 agent 又问：已有 lesson progress 记录是否要 retroactive backfill？这不是纯技术问题。若回填，老用户会立刻获得积分，涉及公平感、迁移脚本、发布说明和测试范围；若不回填，系统从上线后开始计算，逻辑更简单但用户体验不同。这类决策必须由人类和领域专家拍板。

## 应掌握的概念和方法论

- Human-in-the-loop：需要人类判断、取舍、审美、风险偏好或组织约束的阶段。
- AFK task：目标、边界、验收方式已经清楚后，可交给 agent 离键执行的任务。
- Recommendation-backed question：带推荐答案的问题，而不是把空白选择扔回给人类。
- Negative decisions：被明确拒绝的方案也要记录，例如“视频观看暂不计分，因为噪声大且容易被刷”。
- Out of scope：把诱人的扩展关在范围外，防止 agent 主动扩张任务。

## 常见失败方式

第一种失败是“让 agent 替人拍板”。比如让它自行决定积分规则、税务适用口径、生产系统操作策略。它可能给出合理语气，却没有承担业务后果的资格。

第二种失败是“问题很多但没有推进”。Grill Me 问 40 个、80 个问题并不代表成功。成功标志是关键假设被显式化，决策依赖被解除，后续能压缩成 PRD、issue 和验收标准。

第三种失败是“只记录采用方案，不记录放弃方案”。后续 agent 看到旧材料时，会把已经否定过的选项重新当成灵感。

## 来源依据

- `resources/agent-frontline-practice-implementation-experience-and-know-how.pdf`: 支撑一线实践中“先对齐、再执行”的经验。
- `resources/how-claude-code-works-in-large-codebases.pdf`: 支撑编码任务中先读代码和项目规则，再澄清边界的做法。
- `resources/real-engineers-ai-agent-coding-workflow.pdf`: Cadence gamification、recommendation-backed questions、human-in-the-loop 与 AFK task 分界。
- `AGENTS.md`: 要求讨论时一次只解决一个关键问题，agent 先给推荐答案，再说明取舍。

## PPT 使用建议

适合做“人机分工”页。画面可以是一条从模糊 brief 到 PRD 的转换链：模糊需求、追问树、已确认决策、被拒绝方案、验收标准。不要把它写成“人类监督 AI”的空话，要用积分、税务口径或数据指标定义这种真实决策举例。