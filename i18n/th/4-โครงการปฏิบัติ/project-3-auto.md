# 项目三：自动化工作流

构建端到端自动化工作流系统。

## 项目目标

**预计时长**: 2-3 小时

设计并实现一个自动化工作流引擎，处理常见开发任务。

## 功能需求

### 1. 工作流定义
- YAML 格式定义
- 步骤串联和并行
- 条件分支
- 循环处理

### 2. 内置工作流
- **代码检查** - lint + type-check + test
- **构建发布** - build + test + deploy
- **问题修复** - analyze + fix + verify

### 3. 监控告警
- 执行状态追踪
- 失败告警通知
- 耗时统计

## 技术方案

### 工作流定义示例

```yaml
name: code-check
steps:
  - name: lint
    command: npm run lint
  - name: type-check
    command: npm run type-check
  - name: test
    command: npm test
    requires: [lint, type-check]
```

### Hook 集成
- PreToolUse - 参数验证
- PostToolUse - 输出验证
> 参考: [Hook 类型](../เอกสารอ้างอิง/hooks/Hook类型.md) | [自动化与脚本](../เอกสารอ้างอิง/skills/自动化与脚本.md)

## 验收标准

- [ ] 支持 YAML 定义
- [ ] 步骤依赖解析正确
- [ ] 并行执行优化
- [ ] 失败重试机制
- [ ] 执行日志完整

---

[返回实战项目目录](./README.md)
