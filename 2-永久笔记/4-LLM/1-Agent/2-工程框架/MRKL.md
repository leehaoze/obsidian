---
alias: [Modular Reasoning Knowledge and Language]
date created: 星期一, 四月 8日 2024, 11:00:27 上午
date modified: 星期一, 四月 8日 2024, 11:21:47 上午
tags: 
---

# MRKL

## MRKL是什么

MRKL Systems (Modular Reasoning, Knowledge and Language, pronounced "miracle")，直译过来是**模块化推理、知识和语言**。

最初的解释来自与这里：

> A modular, neuro-symbolic architecture that combines large language models, external knowledge sources and discrete reasoning[1]

一种模块化的，结合LLM，外部知识，分步推理的结构。

## 组成

MRKL由一组Module以及一个Router组成，Router可以将自然语言输入路由到最适合响应该输入的module（被称为专家）。

module可以是一个LLM Agent，也可以是一个RAG搜索，亦可以是一个简单的tool。

## 优势

- 可扩展性强
- 可解释性强
- 可以访问最新信息和私有知识

## 一些细节

### 路由分类时的参数提取

router本身是一个模型，需要训练router为每个module提取参数的能力。

[1]:https://arxiv.org/abs/2205.00445
