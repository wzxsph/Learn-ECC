# 项目二：代码审查

使用 ECC 构建自动化代码审查流程。

## 项目目标

**预计时长**: 2-3 小时

创建一个自动化代码审查系统，自动检查代码质量、安全问题、规范遵守。

## 功能需求

### 1. 审查维度
- **代码质量** - 复杂度、重复、命名
- **安全审查** - 漏洞、敏感信息、注入
- **规范检查** - 编码规范、文档完整性
- **测试覆盖** - 覆盖率、测试质量

### 2. 审查流程
- 提交触发审查
- 并行多维度检查
- 汇总报告生成
- 问题分级标记

### 3. 集成能力
- Git Hook 集成
- CI/CD 集成
- GitHub PR 评论

## 技术方案

### Agent 设计
- **code-reviewer** - 主审查 Agent
- **security-reviewer** - 安全专项
- **tdd-guide** - 测试质量评估
> 参考: [/code-review](../参考文档/commands/01-核心工作流.md) | [code-reviewer Agent](../参考文档/agents/1.%20代码审查类.md)

### 输出格式
```json
{
  "summary": { "critical": "0", "high": "2", "medium": "5" },
  "issues": [
    { "type": "security", "severity": "high", "file": "src/auth.js", "line": 42 }
  ]
}
```

## 验收标准

- [ ] 支持 JavaScript/TypeScript 审查
- [ ] 检出率 > 80%
- [ ] 误报率 < 10%
- [ ] 报告格式友好
- [ ] 与 Git 工作流集成

---

[返回实战项目目录](./README.md)
