# 01 · RSI 与自动化 AI 研发

## RSI 到底指什么

“模型修改一段自己的提示词”与“模型自主构建更强的下一代模型”之间差别很大。为了避免把所有循环都称为 RSI，可以把进展分为五级：

| 等级 | 系统能改进什么 | 2026 年状态 |
|---|---|---|
| L0 | 单次回答，自我反思后重写 | 已普及 |
| L1 | 提示、记忆、工具与 Agent 工作流 | 已在实验和生产中使用 |
| L2 | 自动生成数据、奖励、实验和代码 | 快速发展 |
| L3 | 自动选择研究方向并完成训练闭环 | 尚未可靠实现 |
| L4 | 自主设计、训练、验证并部署更强继任系统 | 未实现 |

当前最实质的进展处于 **L2，正在接近受监督的 L3**。

最新项目的逐项证据与边界见：[2026 RSI 路线图](07-rsi-landscape-2026.md)。

## 2026 年的新信号

### OpenAI：自动化研究“实习生”

OpenAI 在 2026-09-06 披露，其系统已经能够在人类监督下完成定义明确、原本需要熟练研究者数天的研究任务。其内部统计还显示，截至 8 月，研究组织每个人类工作日对应约 3.1 个 Agent 工作日。

这说明实验执行、代码实现和部分分析正在大规模自动化。但 OpenAI 同时明确指出，人类仍负责确定优先级、判断哪些结果值得推进，以及决定训练、暂停或部署。

- 来源：[Research acceleration: The view inside OpenAI](https://openai.com/index/research-acceleration-view-inside-openai/)
- 证据等级：**B**（一手机构披露，内部测量方法有说明，但数据不可完全外部复现）

### Anthropic：AI 已经开始“构建 AI”

Anthropic 的最新披露显示，Claude 已经能够完成越来越开放的工程和研究任务。一个代表性案例是：多 Agent 围绕弱模型监督强模型的问题，自主提出假设、运行实验、共享结果并迭代；人类仍然选择研究问题并建立评分标准。

Anthropic 还报告，Claude 生成了其代码库中很大比例的合并代码。不过“代码行数”并不等于研究产出或真实生产率，Anthropic 自己也明确提示了这一局限。

- 来源：[When AI builds itself](https://www.anthropic.com/institute/recursive-self-improvement)
- 来源：[Measurements for understanding the pace of AI development](https://www.anthropic.com/institute/measuring-pace-of-ai-development)
- 证据等级：**B**

### 学术界：开始定义“什么才算 RSI”

2026 年的研究开始明确区分：

- **有界自我改进**：目标和评分规则固定，系统优化提示、记忆、代码或策略；
- **研究循环自动化**：系统产生并运行实验，但人类仍把握方向和部署；
- **开放式 RSI**：系统还会改进“如何改进”的机制本身，并自主构建后继系统。

目前有证据支持前两种，没有公开证据支持第三种已经实现。

- 综述：[Recursive Self-Improvement in AI: From Bounded Self-Refinement to Autonomous Research Loops](https://arxiv.org/abs/2607.07663)
- 最新观点论文：[The Last AI Built by Humans: Toward Genuine Recursive Self-Improvement](https://arxiv.org/abs/2609.11873)
- 证据等级：**C**

## 为什么还不能称为完整 RSI

完整闭环至少需要同时解决：

1. **目标生成**：系统知道什么研究问题最值得做；
2. **实验设计**：提出能区分不同假设的实验；
3. **可靠执行**：代码、数据和基础设施没有隐蔽错误；
4. **结果判断**：不会奖励投机、过拟合或虚假改进；
5. **跨代保留**：有效经验真正进入下一代模型或系统；
6. **安全部署**：改进不会绕过权限、验证与治理。

今天进展最快的是第 3 项，最难的仍是第 1、4 和 6 项。

## 观察指标

比“模型写了多少代码”更有意义的 RSI 指标包括：

- Agent 独立提出并验证的新研究结论占比；
- 从高层目标到可信结果之间需要的人类干预次数；
- 结果在不同模型、任务和规模上的迁移能力；
- 对失败实验的识别率，而不只是成功展示；
- 经 Agent 发现并最终进入正式训练或产品的改进数量；
- 监督、审查和事故率是否随 Agent 工作量同步改善。
