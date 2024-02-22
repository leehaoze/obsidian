---
date created: 星期日, 二月 18日 2024, 3:31:33 下午
date modified: 星期日, 二月 18日 2024, 5:15:48 下午
tags: 
---

# LLM Powered Autonomous Agents

## LLM Agent System Overview

LLM[[Agent]]系统以LLM为核心，包含三个关键组件：
- **Planning**
	- **Subgoal and decomposition**: Agent将原始任务，分解为更小、可管理的子目标，以高效的处理复杂任务
	- **Reflection and refinement**: Agent对过去的行动进行反思，从错误中吸取教训以提高最终结果的质量
- **Memory**
	- **Short-term memory**: 所有[[In-Context Learning]]都被视为Agent的短期记忆
	- **Long-term memory**：为Agent提供保留和回忆（无限）信息的能力，通常利用向量数据库 + 检索实现。
- **Tool use**
	- Agent通过调用外部的工具（API）来获取模型预训练数据中没有的信息或执行一些操作，比如一些时效性信息、执行代码等。

