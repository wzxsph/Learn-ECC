# Otimização-de-Desempenho

## 模型选择策略

**Haiku 4.5**（Sonnet 90% 能力，1/3 成本）：
- 频繁调用的轻量级 agent
- 结对编程和代码生成
- 多 agent 系统中的 worker agent

**Sonnet 4.6**（最佳编码模型）：
- 主要开发工作
- 编排多 agent 工作流
- 复杂编码任务

**Opus 4.5**（最深度推理）：
- 复杂架构决策
- 最大推理需求
- 研究和分析任务

## 上下文窗口管理

避免使用上下文窗口的最后 20%：
- 大规模重构
- 跨多文件的功能实现
- 调试复杂交互

低上下文敏感度任务：
- 单文件编辑
- 独立工具创建
- 文档更新
- 简单 bug 修复

## 扩展思考 + 计划模式

扩展思考默认启用，为内部推理保留最多 31,999 tokens。

控制扩展思考：
- **切换**: Option+T (macOS) / Alt+T (Windows/Linux)
- **配置**: 在 `~/.claude/settings.json` 中设置 `alwaysThinkingEnabled`
- **预算上限**: `export MAX_THINKING_TOKENS=10000`
- **详细模式**: Ctrl+O 查看思考输出

需要深度推理的复杂任务：
1. 确保扩展思考已启用（默认）
2. 启用**计划模式**以获得结构化方法
3. 使用多轮批评进行彻底分析
4. 使用分角色 sub-agents 获得不同视角

## 构建故障排除

如果构建失败：
1. 使用 **build-error-resolver** agent
2. 分析错误消息
3. 增量修复
4. 每次修复后验证
