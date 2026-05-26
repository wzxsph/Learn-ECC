# 02-自主代理

学习如何构建能够自主决策和执行任务的代理系统。

## 什么是自主代理？

自主代理（Autonomous Agent）是一种能够：
- 自主规划任务步骤
- 根据环境反馈调整策略
- 持续执行直到达成目标

的智能系统。

> 参考: [Automação-e-Scripts](../DocumentosDeReferência/skills/Automação-e-Scripts.md) | [代码审查类 Agent](../DocumentosDeReferência/agents/1.%20代码审查类.md)

## 学习目标

- 理解自主代理架构
- 设计代理决策流程
- 实现代理执行循环
- 处理异常和恢复

## 核心组件

### 1. 规划器（Planner）
将复杂任务分解为可执行步骤。

### 2. 执行器（Executor）
调用工具执行具体操作。

### 3. 评估器（Evaluator）
判断任务是否完成，评估执行结果。

### 4. 记忆（Memory）
存储对话历史和上下文。

## 代理模式

### 反应式代理
根据当前状态直接响应环境变化。

### 目标导向代理
围绕明确目标规划和执行。

### 学习型代理
通过经验改进决策策略。

## 下一步

- [高级模式](./03-PadrõesAvançados.md) - 设计模式与Melhores-Práticas
- [返回专家之路目录](../README.md)

## 配套资源

- [MCP 开发](./01-DesenvolvimentoMCP.md) - 掌握模型上下文协议
