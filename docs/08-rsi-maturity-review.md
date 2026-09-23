# RSI 成熟度审查：哪些进展已经越过演示阶段？

> 核验日期：2026-09-23。这里的“成熟”指有长期运行、留出验证、真实部署、公开代码或可审计产物中的至少两项，不等同于完整 RSI。

## 结论先行

截至目前，**没有公开证据表明完整、通用、无人监督的 RSI 已实现**。OpenAI 也明确表示，能独立推动一代代更强系统的完全自主 RSI 尚未发生。

但三个局部方向已经相对成熟：

1. **可自动验证的算法搜索已经进入生产**：AlphaEvolve 是最强实例。
2. **AI 研发执行正在实验室规模化**：OpenAI 和 Anthropic 已在真实研发流程中大量使用 Agent，但研究方向和部署仍由人控制。
3. **端到端自动训练闭环开始跨越玩具规模**：A-Evolve-Training 展示了多周、四轮、30B 模型无人值守 post-training，是目前公开材料中最接近“模型开发闭环”的案例。

更激进的 Harness 自修改、权重与 Harness 联合更新、评价器共进化已经能跑，但大多仍是预印本和有限基准，尚未达到生产成熟。

## 成熟度标准

| 维度 | 成熟信号 | 不足以证明成熟的信号 |
|---|---|---|
| 循环长度 | 多轮、多天或多周运行 | 一次生成—批评—重写 |
| 评价 | 隐藏集、外部指标、重复随机种子 | 使用搜索时可见的同一分数 |
| 现实性 | 生产部署或真实训练任务 | 小型合成环境演示 |
| 可审计 | 代码、日志、产物和失败案例 | 只有最终排行榜截图 |
| 迁移 | 跨模型、任务或环境成立 | 单模型、单 benchmark |
| 递归性 | 新版本提高后续改进能力 | 只提高当前任务分数 |
| 安全性 | 沙箱、回滚、冻结边界和人工闸门 | 允许模型无约束执行自身代码 |

## 第一梯队：成熟的 RSI 组件

### 1. AlphaEvolve：生产成熟，但不是完整 RSI

AlphaEvolve 用 LLM 生成程序、自动 evaluator 验证、演化档案保留优秀方案。它已经越过论文演示：Google 披露其数据中心调度启发式已生产运行超过一年，平均持续回收全球约 0.7% 的计算资源；发现的矩阵乘法拆分让 Gemini 相关 kernel 加速 23%，使训练时间降低约 1%；另有方案进入 TPU 设计。

它还是目前最可靠的“自我改进组件”，因为其结果可由编译、运行时间、数学验证或硬件验证自动检查。它甚至优化了训练 AlphaEvolve 所依赖模型的基础设施，形成了间接反馈。

但演化算法、评价器、目标和基础模型仍由人固定，因此严格来说是**成熟的可验证搜索与自动研发系统**，不是能改写自身完整改进机制的 RSI。

- [Google DeepMind 官方发布](https://deepmind.google/blog/alphaevolve-a-gemini-powered-coding-agent-for-designing-advanced-algorithms/)
- 成熟度：**生产级组件**；RSI 闭环：**局部 L2**

### 2. OpenAI / Anthropic：研发自动化成熟，闭环仍由人掌舵

OpenAI 披露，截至 2026 年 8 月中旬，其研究组织每个人类工作日使用约 3.1 个 Agent 工作日，并达到“自动化研究实习生”里程碑。Anthropic 披露开放式研究任务的内部成功率从约 26% 升至 91%，同时 AI 已深度参与公司代码和实验工作。

这类证据的价值在于规模和真实环境，不在于递归深度。两家公司都仍由人类决定研究方向、优先级、评价框架和是否训练、暂停或部署；数据又主要是内部测量，部分 Anthropic 成功率由 Claude judge 判定。

- [OpenAI：Research acceleration](https://openai.com/index/research-acceleration-view-inside-openai/)
- [Anthropic：When AI builds itself](https://www.anthropic.com/institute/recursive-self-improvement)
- [OpenAI 对现状的明确边界](https://openai.com/index/ai-policy-window/)
- 成熟度：**真实生产流程**；RSI 闭环：**受监督 L2，逼近 L3**

## 第二梯队：最强公开闭环实证

### 3. A-Evolve-Training：30B 模型的多周自主 post-training

这是目前最值得重视的公开案例之一。系统在无人介入的情况下，对 30B Nemotron 进行四轮、持续数周的 post-training：自主提出方向、启动真实 GPU 训练、读取评测并调整下一轮研究策略。最终隐藏分数为 0.86，接近公开挑战中最佳人工方案的 0.87，当时约列 4,000 份提交中的第 8 名。

更关键的是，系统发现内部 dev 指标已不再跟踪外部目标，于是改变搜索策略，不再简单最大化失真的代理指标。论文还报告在 120B 和 550B 上闭合了训练流程，但没有可比较的人类基线，因此只能证明基础设施可扩展，不能证明效果同样强。

限制是单一基础模型、单一挑战、预先审计的不可变基础设施和冻结 meta-prompt；当前仍是团队自报，缺少独立复现。它是**较成熟的窄域自动训练闭环**，而非通用 RSI。

- [论文](https://arxiv.org/abs/2606.20657)
- [公开组织与产物](https://github.com/A-EVO-Lab)
- 成熟度：**强研究原型**；RSI 闭环：**窄域 L3**

### 4. Recursive 的自动 AI 研究系统：结果强，但系统本身不完全开放

Recursive 报告系统可连续提出想法、实现、执行实验、检查方差和奖励投机，再据此安排后续研究。在三个可验证任务上，它报告：

- NanoChat 固定预算训练从此前 0.9372 BPB 改善到 0.9109；
- NanoGPT Speedrun 从 79.7 秒缩短到 77.5 秒；
- 235 个 GPU kernel 的平均 SOL 分数从 0.699 提高到 0.754。

其优势是使用多随机种子、检查 reward hacks，并公开部分运行产物；不足是完整研究系统没有开放，数字主要来自公司自己的运行。因此它比概念演示成熟，但可复现性低于完全开源研究。

- [First Steps Toward Automated AI Research](https://www.recursive.com/articles/first-steps-toward-automated-ai-research)
- 成熟度：**强研究系统**；RSI 闭环：**L2–L3**

## 第三梯队：可运行、可研究，但还不算成熟

### 5. Darwin Gödel Machine / Hyperagents

DGM 已成为 ICLR 2026 论文并开放代码，能够保留自修改 Agent 的多个谱系，避免只沿单一路线爬山。Hyperagents 进一步允许 Task Agent 和 Meta Agent 一起被修改，覆盖代码、论文评审、奖励设计和数学评分。

这是递归性最直接的路线之一，但实验仍主要依赖固定 benchmark、固定基础模型和外部选择框架，而且运行模型生成代码有明确安全风险。当前适合视为**开放研究底座**，而不是可靠生产 Agent。

- [DGM 论文](https://arxiv.org/abs/2505.22954)
- [Hyperagents](https://ai.meta.com/research/publications/hyperagents/)
- [Hyperagents 代码](https://github.com/facebookresearch/Hyperagents)
- 成熟度：**开源研究框架**；递归性：**强，可靠性仍不足**

### 6. Self-Harness：Harness 自修改开始有跨模型留出验证

Self-Harness 从执行轨迹挖掘弱点，提出最小 Harness 修改，并经过回归测试才接受。在 Terminal-Bench 2.0 上，三个不同模型的 held-out pass rate 分别从 40.5% 提至 61.9%、23.8% 提至 38.1%、42.9% 提至 57.1%；论文还测试 SWE-bench Verified 和 AppWorld。

它的成熟信号是跨三种模型、使用 held-out 集和回归验证；不足是尚无长期生产数据，基础模型、验证框架和目标都固定。

- [论文](https://arxiv.org/abs/2606.09498)
- 成熟度：**较扎实原型**；RSI 闭环：**L2**

### 7. SIA：Harness 与权重联合更新

SIA 让 Feedback-Agent 同时更新任务 Agent 的 Harness 和模型权重，并在中国法律分类、GPU kernel 优化和单细胞 RNA 去噪三个领域优于只改 Harness 的方案。代码已经公开。

这是结构上重要的一步，因为权重和运行时框架不再被人为拆开；但其百分比提升跨越完全不同指标，不能直接横向比较，而且尚无长期多代曲线或独立复现。

- [论文](https://arxiv.org/abs/2605.27276)
- [代码](https://github.com/hexo-ai/sia)
- 成熟度：**早期开放原型**；RSI 闭环：**L2，结构上接近 L3**

### 8. Red Queen Gödel Machine：让评价器也演化

RQGM 解决固定 evaluator 最终饱和或被投机的问题：每个 epoch 内冻结评价标准，只在边界处用固定 ground-truth anchor 检验并替换 evaluator。论文报告，它在代码、论文写作/评审和奥数证明评分上优于先前自改进 Agent；例如共进化 grader 的 ground-truth accuracy 提高 9%。

这比只让 solver 变强更接近真正 RSI，但“评价器评价评价器”的稳定性仍依赖冻结锚点，官方完整实现与独立复现也不足。

- [论文](https://arxiv.org/abs/2606.26294)
- 成熟度：**前沿实验**；递归性：**很强，成熟度较低**

## 最新基准带来的冷静结论

ByteDance Seed 与 TokenWave 的 Self-Developing Agents 系列专门测试完整循环最容易失败的三处：选择改进目标、从经验学习、让 Harness 改进持续存在。结果很有警示意义：

- Aspire 的 30 个配置—目标组合里，28 个产出了 checkpoint，但只有 1 个通过保留阈值；
- HarnessDev 的 64 次版本切换里，可见反馈与隐藏集方向一致的只有 34 次；
- 9 个最终版本里只有 2 个恰好是隐藏集最优；
- S³Gym 中模型的自我判断几乎不能预测下一轮是否提高，相关系数约为 −0.010 和 −0.018；
- 没有一种记忆表示在所有环境中稳定获胜。

这意味着“循环跑起来”远远不等于“能力可靠复利”。当前最成熟的系统都有一个共同点：**窄目标、强验证器、不可变安全边界、隐藏集验证和随时回滚**。

- [Self-Developing Agents](https://self-developing-agents.github.io/)

## 综合排名

| 类别 | 当前代表 | 生产成熟 | 闭环完整 | 递归深度 | 公开可复现 |
|---|---|---:|---:|---:|---:|
| 可验证算法搜索 | AlphaEvolve | 高 | 中 | 低 | 低–中 |
| 大厂 AI 研发自动化 | OpenAI / Anthropic | 高 | 中 | 低–中 | 低 |
| 自主大模型 post-training | A-Evolve-Training | 低–中 | **高** | 中 | 中 |
| 自动 AI 研究 | Recursive | 中 | 高 | 中 | 低–中 |
| Agent 代码自修改 | DGM / Hyperagents | 低 | 中–高 | **高** | 高 |
| Harness 自修改 | Self-Harness | 低 | 中 | 中 | 中 |
| Harness＋权重联合更新 | SIA | 低 | 高 | 中 | 高 |
| Agent＋Evaluator 共进化 | RQGM | 低 | 高 | **高** | 低–中 |

## 最终判断

如果“成熟”指可以创造实际价值，**AlphaEvolve 和大厂内部 AI 研发 Agent 已经成熟**；如果指公开的闭环自我改进，**A-Evolve-Training 是目前最强的规模化证据之一**；如果指真正递归地修改改进机制，**Hyperagents 和 RQGM 最接近，但远未生产成熟**。

因此最准确的表述是：

> RSI 的零件正在成熟，窄域闭环已经成立；完整、通用、可靠复利的 RSI 尚未出现。
