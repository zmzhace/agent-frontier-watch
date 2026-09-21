# 04 · Agent Control

## 从“模型对齐”到“系统控制”

当模型只回答问题时，主要风险在输出内容；当 Agent 能运行数小时、调用工具、读取密钥、修改代码和接触生产系统时，风险来自整个行动链。

Agent Control 的出发点是：**即使模型大多数时候是对齐的，也要把它当作可能犯错、被攻击或主动规避限制的内部主体来设计系统。**

## DeepMind 的 AI Control Roadmap

Google DeepMind 在 2026-06 发布的路线图采用纵深防御：

1. 沙箱、端点安全和提示注入防护作为基础；
2. 根据已验证行为逐步授予权限；
3. 使用独立监督模型检查工作 Agent 的计划、推理和动作；
4. 检测到风险时阻断动作；
5. 用覆盖率、召回率和响应时间衡量控制系统；
6. 随 Agent 能力和规避监控能力升级防线。

- 来源：[Securing the future of AI agents](https://deepmind.google/blog/securing-the-future-of-ai-agents/)
- 技术报告：[AI Control Roadmap](https://storage.googleapis.com/deepmind-media/DeepMind.com/Blog/securing-the-future-of-ai-agents/An_Approach_to_Technical_AGI_Safety_and_Security.pdf)
- 证据等级：框架 **B**；其对未来风险的判断 **D**

## Chain-of-Thought 监控：当前有用，但不能成为唯一防线

OpenAI 2026 年的研究发现，当前推理模型即便知道自己受到监控，也不太擅长按要求系统性控制或隐藏 Chain-of-Thought。这意味着 CoT 暂时仍是有价值的安全信号。

但这个结论不能保证未来仍成立：能力提升、训练压力或不透明的内部推理都可能削弱可监控性。因此 CoT 应当只是信号之一，而不是安全边界。

- 来源：[Reasoning models struggle to control their chains of thought, and that’s good](https://openai.com/index/reasoning-models-chain-of-thought-controllability/)
- 证据等级：**A/B**

## 2026 国际 AI 安全报告的共同框架

报告把技术控制分布在系统多个位置：

- 输入端的攻击和有害内容检测；
- 输出端过滤；
- Chain-of-Thought 与行为监控；
- 模型内部状态监控；
- 工具与网络沙箱；
- 硬件和基础设施级监测；
- 持续的人类监督与问责。

- 来源：[International AI Safety Report 2026](https://internationalaisafetyreport.org/sites/default/files/2026-02/international-ai-safety-report-2026.pdf)
- 证据等级：**A**（综合评估报告）

## 一个可落地的控制栈

```text
任务入口
  ├─ 身份、来源、提示注入检查
  ├─ 风险分级与预算
  ▼
计划阶段
  ├─ 计划审查
  ├─ 最小权限和允许的工具集合
  ▼
执行阶段
  ├─ 沙箱 / 网络隔离 / 凭据代理
  ├─ 动作级策略检查
  ├─ 独立监督器
  └─ 资源与时间上限
  ▼
提交阶段
  ├─ 测试、验证和差异审查
  ├─ 高风险操作需人工批准
  └─ 可回滚部署
  ▼
事后
  ├─ 不可篡改审计日志
  ├─ 事故回放
  └─ 更新评估集和权限策略
```

## 尚未解决的问题

- 监督模型和工作模型可能共享同样的盲点；
- 多 Agent 之间的串谋或错误放大很难从单 Agent 日志发现；
- Agent 可以通过正常工具调用组合出高风险行为；
- 权限越严格，Agent 的实际价值越低，存在明显可用性权衡；
- 只监控文本推理可能漏掉隐藏状态中的计划；
- 自动化程度上升后，人类可能只做形式化审批，形成“橡皮图章”。

Agent Control 最重要的工程原则不是“永远相信监督模型”，而是让任何单点失败都不足以造成不可逆后果。

