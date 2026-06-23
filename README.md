# agent-for-long-run

这个仓库用于组织一项 agent 协作任务：基于技术视频总结 PDF、讨论沉淀的 ideas，生成一份约 10 页、约 8 分钟的 PPT。

PPT 主题：税务管理员、程序员、数据分析师、算法工程师、平台运营工程师等知识工作者，如何使用 agent 加速工作、如何与 agent 协作做好工作、如何设计和使用 agent 完成长程任务。

## 快速入口

- `AGENTS.md`: agent 执行本项目时应先阅读的指导文件。
- `resources/`: 放原材料，例如技术视频总结 PDF。
- `ideas/`: 放讨论后达成共识的 ideas，一个 idea 对应一个 markdown 文件。

## 推荐流程

1. 把相关 PDF 或其他资料放入 `resources/`。
2. 让 agent 盘点原材料，并生成候选 ideas。
3. 与 GPT/agent 逐个讨论候选 ideas，确认概念、场景、风险和 PPT 价值。
4. 把确认后的 ideas 写入 `ideas/`。
5. 基于 confirmed ideas 生成 PPT 骨架、逐页内容和演讲稿。

详细规则见 `AGENTS.md`。
