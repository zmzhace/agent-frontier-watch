# 2026 RSI 路线图：哪些系统真的在“改进如何改进”

> 核验日期：2026-09-21。本页只收录具有论文、官方技术披露或公开代码的项目。

## 核心判断

2026 年最重要的变化，不是出现了一个能独立训练出更强继任者的完整 RSI，而是研究对象从“改进一次答案”逐渐上移到了五个更持久的层面：

```text
经验与知识 → 记忆/技能 → Agent Harness → 改进策略 → 模型权重
   KSI       PAST-Bench    Meta^n        Dream-RSI     ScienceBuddy
                          Hyperagents                  AI R&D agents
```

其中，**Hyperagents 和 Dream-RSI 最接近“改进改进机制”这一递归要求**；ScienceBuddy 开始把 Harness 演化和模型训练接在一起；OpenAI、Anthropic 的内部系统则显示自动化 AI 研发正在生产环境扩张。但没有公开项目同时证明了自主选题、训练、独立验证、安全部署和跨代加速。

## 快速比较

| 项目 | 持久改进对象 | 被固定的关键部分 | 是否改进“如何改进” | 证据状态 | 判断 |
|---|---|---|---|---|---|
| PAST-Bench / Hermes+ | 跨会话记忆、技能、状态更新 | 基础模型、任务序列、框架 | 弱 | 论文＋代码 | RSI 基础能力测试，不是完整 RSI |
| KSI | 可审计共享知识库 | 通用 Agent 与整理协议 | 间接 | 论文＋代码 | 可迁移、低风险的外部化自我改进 |
| Meta^n | 多层预处理代码和工具库 | 元操作 Ω | 是，但核心算子固定 | 预印本＋代码 | 通过递归输入形成可变深度 |
| Hyperagents / DGM-H | Task Agent、Meta Agent 及改进程序 | 评估器、基础模型、外层实验框架 | **是** | Meta 论文页＋代码＋日志 | 当前最直接的元级自修改证据之一 |
| Dream-RSI | 探索策略 | 基础 Agent、评估器、已观测历史 | **是** | 预印本＋项目页 | 低成本优化“怎样搜索”，不是改模型权重 |
| ScienceBuddy | Harness 与模型权重 | 人类需求、反馈、训练基础设施 | **双层闭环** | 预印本＋代码＋案例 | 方向重要，广泛实证仍不足 |
| OpenAI 自动研究实习生 | AI 研究执行流程 | 研究方向、训练和部署决策 | 部分 | 内部测量 | 受监督自动化 AI R&D，接近 L3 但未闭环 |
| Anthropic AI R&D | 工程、实验和研究下一步建议 | 高层方向、组织与最终判断 | 部分 | 内部测量 | 真实生产加速强，因果测量仍有限 |
| Sakana RSI Lab | Agent 代码、AI Scientist、未来模型研发 | 现阶段仍由人设定战略与评价 | 目标是 | 官方路线＋既有论文 | 战略整合信号，不等于完整 RSI 已实现 |

## 1. PAST-Bench：先验证“经验真的留下来了吗”

很多 Agent 声称会记忆，但保存文件不等于未来表现因此提高。PAST-Bench 用带持久化和不带持久化的匹配对照，测试 7 个基础模型、4 个 Agent 框架，覆盖 26 个场景、204 个 episode，并检查提升是否真的沿着保存、检索、更新路径发生。

论文发现，跨会话提升存在但很不均匀；相同的最终增益可能对应完全不同的机制证据。它的重要性在于为 RSI 加入了一个反事实问题：**如果关掉持久经验，后续能力是否真的下降？**

- [论文](https://arxiv.org/abs/2608.04003)
- [代码](https://github.com/Gen-Verse/PAST-Bench)
- RSI 等级：**L1**；证据：**C**

## 2. KSI：改进对象可以不是 Agent，而是知识

Knowledge-Centric Self-Improvement（KSI）让一次性 Agent 将有证据支撑的经验写入共享知识库，再通过任务内讨论、跨任务讨论和蒸馏形成未来 Agent 可复用的知识。论文在抽象推理、编码和终端任务上报告了相对 Agent 自修改基线更高的解题率和更低成本，并测试了跨任务与跨模型迁移。

它不是强递归 RSI，因为整理协议本身仍固定；但它提供了一条工程上很实用的路线：改进可以被版本控制、审计、删除和迁移，不必锁在某一模型权重或某次 Agent 运行里。

- [论文](https://arxiv.org/abs/2607.19592)
- [代码](https://github.com/recursive-knowledge/KSI)
- RSI 等级：**L1–L2**；证据：**C**

## 3. Meta^n：不修改元操作，而是递归增加观察深度

Meta^n 固定一个元操作 Ω，让它反复读取下层执行轨迹与生成这些轨迹的代码，再写出下一层的预处理逻辑和可调用工具库。层数由收敛而不是预设深度决定，演化档案负责搜索不同层链。

作者报告，两个 backbone 在 8 个 benchmark family 上超过先前自改进 Agent，并在 ARC-AGI-2 上成为对照中唯一取得非零成绩的方法。消融结果将大部分收益归因于层间传递的条件信息。

需要注意：Ω、外层档案和评估仍由人固定。因此它展示的是**递归计算结构带来的过程改进**，不是系统无限制重写全部改进机制。

- [论文](https://arxiv.org/abs/2608.24735)
- [代码](https://github.com/minnesotanlp/meta-n)
- RSI 等级：**L2**；证据：**C**

## 4. Hyperagents：把 Meta Agent 也放进可编辑程序

Hyperagents 将解决任务的 Task Agent 和修改两者的 Meta Agent 放在同一个可编辑程序中。其 DGM-H 实现允许修改产生未来 Agent 的过程，例如加入持久记忆、性能跟踪或新的生成逻辑，而不只是修改任务提示。

Meta 报告它在代码、论文评审、机器人奖励设计和奥数解答评分中持续提高，并优于没有开放式探索或没有自我改进的对照；部分元级改进还能跨领域和跨运行积累。这是目前公开工作里最接近“系统提高自身改进能力”的证据之一。

边界同样清楚：基础模型、评分函数、算力预算和外层选择环境仍固定；任务又必须可计算且可评价。代码仓库还明确警告会执行不受信任的模型生成代码，需要 Docker 沙箱和人工监督。

- [Meta 研究页](https://ai.meta.com/research/publications/hyperagents/)
- [论文](https://arxiv.org/abs/2603.19461)
- [代码与实验日志](https://github.com/facebookresearch/Hyperagents)
- RSI 等级：**L2，局部触及 L3**；证据：**C（预印本＋可复现材料，尚待独立大规模复现）**

## 5. Dream-RSI：利用过去的搜索树“梦见”更好的搜索策略

Dream-RSI 的闭环分三步：当前策略在线搜索并记录 discovery tree；历史树被转换成 replay simulator；系统在不重新执行昂贵实验的情况下测试大量候选探索策略，再把胜出策略部署到下一轮。

论文在算法工程、数学优化和 GPU kernel 工程上报告了相当或更好的发现质量，并在部分任务上显著减少搜索代数；例如 VGG16 和 LayerNorm 达到相近最终表现时分别少用约 2.43× 和 1.79× generations。

它真正递归改进的是**探索计算如何分配**。基础 coding agent 和 evaluator 没有同步变强，replay world 也只包含已经探索过的分支，所以不能把它写成“模型已经自主训练出更强模型”。

- [论文](https://arxiv.org/abs/2609.14858)
- [项目页](https://www.dream-rsi.com/)
- RSI 等级：**L2，局部触及 L3**；证据：**C**

## 6. ScienceBuddy：Harness 演化和模型训练开始相互喂数据

ScienceBuddy 提出 recursive-in-recursive improvement：

- 内循环固定模型，根据研究者反馈和执行证据改进 Agent Harness；
- 外循环在改进后的 Harness 下收集训练经验，对模型执行强化学习；
- 新模型又为下一轮 Harness 改进提供更强起点。

与只改提示或只训练权重相比，它第一次把两个改进面明确组成互相作用的双层循环。不过当前论文主要提供 4 类科学任务的案例和两轮动态，尚不足以证明长期单调提升、跨域泛化或自加速。

- [论文](https://arxiv.org/abs/2609.17523)
- [代码](https://github.com/Gen-Verse/ScienceBuddy)
- RSI 等级：**目标为 L3，当前证据更接近 L2**；证据：**C**

## 7. 大厂内部：AI 研发自动化正在成为真实生产变量

OpenAI 披露，截至 2026 年 8 月中旬，研究组织每个人类工作日对应约 3.1 个 Agent 工作日，并称已达到“自动化研究实习生”里程碑。但人类仍承担优先级、研究方向、结果判断和部署决策。

Anthropic 的 130 人内部调查中，受访者对 Mythos Preview 的中位自报生产率提升约为 4×，官方明确认为真实提升可能更低。其开放式 Claude Code 任务成功率从约 26% 上升到约 91%，但成功由另一个 Claude judge 判定，因此需要警惕同源评估偏差。

- [OpenAI：Research acceleration](https://openai.com/index/research-acceleration-view-inside-openai/)
- [Anthropic：When AI builds itself](https://www.anthropic.com/institute/recursive-self-improvement)
- RSI 等级：**L2，接近受监督 L3**；证据：**B**

## 8. Sakana AI RSI Lab：从单个实验变成组织战略

Sakana AI 新设 RSI Lab，将 Darwin Gödel Machine、AI Scientist 和 Agent-Native Foundation Models 放进同一战略循环：Agent-native 模型驱动 AI Scientist，AI Scientist 再帮助构建更好的模型。此前 DGM 已展示通过维护并评估自改写 Agent 的谱系，提高软件工程表现。

这值得关注，因为组织开始把 RSI 当作独立研发方向配置资源；但实验室成立和路线图是未来承诺，不是新一轮闭环已经完成的证据。

- [Sakana AI RSI Lab](https://sakana.ai/rsi-lab/)
- RSI 等级：**现有工作 L2，战略目标 L3–L4**；证据：**B/C**

## 如何识别“RSI 洗词”

一个系统至少应回答以下问题，才能把“self-improvement”升级为较强的 RSI 主张：

1. 什么东西跨轮次永久改变了：回答、记忆、知识、代码、Harness、权重，还是改进器？
2. 新版本是否在未见任务和严格留出集上更好，而不只是利用测试集？
3. 是否有关闭持久化或关闭递归层的反事实对照？
4. 评价器是否独立，能否抵抗奖励投机和同源模型偏差？
5. 改进是否迁移到不同模型、任务和环境？
6. 下一代是否更擅长产生再下一代改进，而不仅是任务分数提高？
7. 算力、人工干预和失败实验成本是否随轮次下降？
8. 系统是否有沙箱、权限边界、日志、暂停和回滚？

只做“生成—批评—重写”通常是 test-time refinement；只保存聊天记录是 persistence；只用自动奖励训练一次模型是自动化 post-training。它们可以成为 RSI 的部件，但不应单独被称为完整 RSI。

## 下一步最值得跟踪

1. Hyperagents 的跨实验室复现，以及元级改进是否在更多轮次持续累积。
2. Dream-RSI 在历史覆盖不足、分布突变和错误 evaluator 下是否仍能改进。
3. ScienceBuddy 的多轮曲线：Harness 和权重共进化会持续增益还是互相过拟合。
4. PAST-Bench 的机制证据能否扩展到真实长期个人 Agent。
5. OpenAI、Anthropic 是否公开“AI 发现并进入下一代训练”的具体改进数量，而不只报告 Agent 工时和代码量。
6. 是否出现能自主修改训练器、数据生成器与 evaluator，并在独立留出环境上通过验证的系统。

## 最终判断

RSI 正在从概念变成一组可测量的工程闭环，但现在实现的是**局部递归**：记忆会积累、代码会演化、搜索策略会更新、Harness 和权重开始相互作用。距离完整 RSI 仍差三个关键能力：自主选择高价值目标、建立不被投机的独立评价，以及安全地把改进部署为下一代系统。
