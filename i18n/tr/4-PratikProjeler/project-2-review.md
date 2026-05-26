# 项目二：Kod İnceleme

使用 ECC 构建自动化Kod İnceleme流程。

## 项目Hedef

**预计时长**: 2-3 小时

创建一个自动化Kod İnceleme系统，自动检查Kod Kalitesi、Güvenlik问题、规范遵守。

## 功能需求

### 1. 审查维度
- **Kod Kalitesi** - 复杂度、重复、命名
- **Güvenlik审查** - 漏洞、敏感信息、注入
- **规范检查** - 编码规范、文档完整性
- **Test覆盖** - 覆盖率、Test质量

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

### Ajan Tasarım
- **code-reviewer** - 主审查 Agent
- **security-reviewer** - Güvenlik专项
- **tdd-guide** - Test质量评估
> 参考: [/code-review](../ReferansBelgeleri/commands/01-Temel İş Akışı.md) | [code-reviewer Agent](../ReferansBelgeleri/agents/1.%20Kod İnceleme.md)

### 输出格式
```json
{
  "summary": { "critical": "0", "high": "2", "medium": "5" },
  "issues": [
    { "type": "security", "severity": "high", "file": "src/auth.js", "line": 42 }
  ]
}
```

## Kabul Kriterleri

- [ ] Destekle JavaScript/TypeScript 审查
- [ ] 检出率 > 80%
- [ ] 误报率 < 10%
- [ ] 报告格式友好
- [ ] 与 Git 工作流集成

---

[Geri DönGerçek ProjelerDizin](./README.md)
