# 可验证切片闭环

## 状态

candidate

## 一句话主张

长程 agent 任务要先由人类完成目标取舍，再把目标压缩成文档、拆成可验证的垂直切片，交给 agent 循环执行，并用测试、QA 和 review 把新问题回流成下一批任务。

## 适用角色

跨角色。程序员是 PDF 中的源场景；税务管理员、数据分析师、算法工程师、平台运营工程师也可复用这套“澄清 - 文档 - 任务图 - 执行 - 验证 - 回流”的长程任务方法。

## 工作场景

一个团队收到模糊任务，例如“给课程平台加 gamification”“整理一项新税务政策的执行清单”“分析运营指标异常并给出修复建议”。人类先和 agent 澄清关键取舍，再形成目标文档，随后拆成可抓取、可排序、可验收的小任务。agent 负责执行清楚的小任务；人类在需求判断、质量评估和最终责任节点介入。

## Agent 协作方式

1. 人类在环完成需求对齐：agent 像一名不知疲倦的追问者，一次提出一个关键问题，并给出推荐答案。人类、领域专家和执行者负责拍板，例如哪些行为值得计分、哪些指标可被刷、哪些范围暂缓。
2. 子代理隔离大规模探索：agent 可读取代码库、资料、政策文件或日志，再把摘要交回主任务。摘要适合降低上下文压力，高风险结论仍需回到原始证据确认。
3. PRD 或目标文档压缩共识：把高价值对话沉淀为问题陈述、方案、用户故事、完成定义、范围外事项、实现决策和测试决策。它像施工单，说明要去哪里、哪里别碰、怎样算完成。
4. Kanban/DAG 拆成可执行路线：每个 ticket 写明任务、依赖、执行类型和验收点。依赖关系可理解为 \(A \to B\)：A 完成后 B 才解锁。无环任务图能找出可并行任务。
5. 垂直切片提供早反馈：一张好 ticket 要同时包含触发事件、状态变化和可见反馈。例如完成 lesson 触发 points，系统保存积分，dashboard 显示结果。税务场景中可对应“识别政策条款 - 生成材料清单 - 用样例企业检查输出”。
6. AFK loop 只处理边界清楚的任务：agent 读取本地 issue、最近提交或最近工作记录，再按 prompt 约束选择已解锁任务，实施、验证、提交或总结。
7. QA/review 回流 backlog：测试、typecheck、构建、浏览器检查属于 verification；人工 QA、code review、taste、业务判断属于 evaluation。可用粗略关系表达：\(Q_{AI} \le f(V,E)\)。AI 产出质量受验证质量 \(V\) 和评估质量 \(E\) 共同限制。

## 长程任务设计要点

- 目标：先得到 shared design concept，即人类、领域专家、执行者和 agent 对问题、约束、取舍、完成状态形成同一张脑内地图。
- 资料：把 PDF、代码、政策、数据表、日志、访谈记录作为原材料；agent 摘要只能作为导航，高风险判断要能回到来源。
- 计划：使用 \(R \to D \to P \to I \to V \to E \to P'\) 表达流程。R 是需求澄清，D 是目标文档，P 是计划或 Kanban，I 是实现，V 是机械验证，E 是质量评估，\(P'\) 是 QA 产生的新任务回流。
- 检查点：需求对齐检查、PRD 检查、切片质量检查、AFK 执行检查、verification 检查、evaluation 检查。
- 记忆：把稳定结论写入 PRD、issue、测试、清单或代码结构，减少后续 agent 对长聊天历史的依赖。
- 工具：Grill Me 类追问、PRD 模板、Kanban、本地 issue、DAG、测试命令、浏览器 smoke check、review 清单。
- 验收标准：每张任务卡都有触发事件、状态变化、可见反馈、依赖关系、执行类型和完成定义；AFK 任务有明确输入、权限边界、验证命令和可审查输出。

## 来源依据

- `resources/real-engineers-ai-agent-coding-workflow.pdf`

PDF 明确内容：

- 第 2 章说明从模糊 brief 到 shared design concept 的过程，Grill Me 通过逐问逐答逼出隐藏决策，例如 points economy 和 retroactive backfill。
- 第 2.6 节区分 human-in-the-loop 与 AFK task：规划对齐需要人类参与，明确后的实现 ticket 才适合离键执行。
- 第 3 章把 PRD 定义为 destination document，用来把高价值对话压缩成 problem statement、solution、user stories、definition of done、out of scope、implementation decisions、testing decisions。
- 第 4 章把 PRD 拆成 Kanban、blocking relationships、vertical slices 和 DAG，并强调第一张好 ticket 要贯穿触发、状态和可见结果。
- 第 5 章说明 AFK Agent Loop 的输入条件：本地 issue、最近 commits、明确 prompt；缺少边界会退化成随机改代码。
- 第 6 章区分 verification 与 evaluation，并用测试通过后仍由人工 QA 发现 SQLite error 的例子说明真实路径检查的重要性。
- 第 8.6 节总结主线为 \(R \to D \to P \to I \to V \to E \to P'\)，其中 QA/review 会产生新 issue 回流 Kanban。

分析推断：

- PDF 源场景是 AI coding，但“人类负责取舍、agent 负责执行清楚任务、QA 回流新任务”的结构可泛化到税务、数据分析、算法实验和平台运营。
- 税务政策清单、数据分析报告和平台运维排障都是可替换案例，PDF 未直接讨论这些业务场景。
- 对普通知识工作者来说，最适合进入 PPT 的重点是长程任务的交接物：目标文档、任务卡、依赖图、验证清单和人工评估点。

## 风险与边界

- 跳过需求对齐会把错误目标写进漂亮计划，后续 agent 只会更快地产生偏差。
- AFK loop 只适合边界清楚、可验证、可审查的小任务；涉及业务责任、权限、安全、合规、产品 taste 的节点应留在人类在环。
- Verification 只能证明被检查项通过；evaluation 负责判断结果是否值得接受。测试全绿仍可能漏掉真实用户路径、数据迁移、政策解释或异常流程。
- 子代理摘要有遗漏风险。涉及税务、合规、生产系统和客户影响时，必须回到来源文件、日志、测试或权威材料。
- 并行 agent 需要依赖图和隔离任务。多开 agent 窗口无法自动形成高质量并行，缺少依赖关系会制造合并冲突和责任混乱。

## PPT 使用建议

首选放在第 9 页“长程任务方法”；第 5 页“程序员”只作为备选案例，避免 10 页、8 分钟结构里重复展开。页面可画一条闭环：

`需求澄清 R -> 目标文档 D -> Kanban/DAG P -> agent 执行 I -> 机械验证 V -> 人工评估 E -> 新任务 P'`

讲法建议用“白天班/夜班”类比：白天班由人类整理目标、边界和验收；夜班由 agent 处理已拆好的小任务；第二天人类 review 和 QA，把新问题放回任务板。这样普通听众能理解 agent 加速工作的前提是可交接、可验证、可回流。

## 待确认问题

- 这条 idea 最终放在第 9 页“长程任务方法”，还是放在第 5 页“程序员”角色页？
- PPT 示例是否沿用 PDF 的 gamification coding 案例，或替换成税务政策清单、数据分析报告、平台运维排障等跨角色案例？
- 听众对 PRD、Kanban、DAG 这些词的熟悉度如何？若不熟，PPT 可改用“目标单、任务板、依赖图”的中文讲法。
