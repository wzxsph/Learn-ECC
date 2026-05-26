# 02-Otonom Ajanlar

Nasıl构建Yapabilir自主决策和执行Görev的代理系统。

## 什么是Otonom Ajanlar？

Otonom Ajanlar（Autonomous Agent）是一种Yapabilir：
- 自主规划GörevAdımlar
- 根据环境反馈调整策略
- 持续执行直到达成Hedef

的智能系统。

> 参考: [Otomasyon ve Betik](../ReferansBelgeleri/skills/Otomasyon ve Betik.md) | [Kod İnceleme Agent](../ReferansBelgeleri/agents/1.%20Kod İnceleme.md)

## Öğrenme Hedefleri

- 理解Otonom Ajanlar架构
- Tasarım代理决策流程
- Gerçekleştir代理执行循环
- 处理异常和恢复

## 核心组件

### 1. 规划器（Planner）
将复杂Görev Ayrıştırma为可执行Adımlar。

### 2. 执行器（Executor）
调用工具执行具体操作。

### 3. 评估器（Evaluator）
判断Görev是否Bitir，评估执行结果。

### 4. 记忆（Memory）
存储对话历史和上下文。

## 代理模式

### 反应式代理
根据当前状态直接响应环境变化。

### Hedef导向代理
围绕明确Hedef规划和执行。

### 学习型代理
Vasıtasıyla经验改进决策策略。

## Sonraki Adım

- [İleri Düzey Kalıplar](./03-İleri Düzey Kalıplar.md) - Tasarım模式与En İyi Uygulamalar
- [Geri DönUzmanlık YoluDizin](../README.md)

## 配套资源

- [MCP Geliştirme](./01-MCPGeliştirme.md) - 掌握模型上下文协议
