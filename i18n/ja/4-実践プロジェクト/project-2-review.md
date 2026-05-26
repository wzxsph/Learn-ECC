# Project 2: Code Review

ECC を使用して自動コードレビューシステムを構築します。

## Project Goal

**Estimated time**: 2-3 hours

Create an automated code review system that automatically checks code quality, security issues, and compliance with standards.

## Feature Requirements

### 1. Review Dimensions
- **Code Quality** - Complexity, duplication, naming
- **Security Review** - Vulnerabilities, sensitive information, injection
- **Compliance Check** - Coding standards, documentation completeness
- **Test Coverage** - Coverage rate, test quality

### 2. Review Process
- Commit triggers review
- Parallel multi-dimensional checks
- Summary report generation
- Issue severity tagging

### 3. Integration Capabilities
- Git Hook integration
- CI/CD integration
- GitHub PR comments

## Technical Solution

### Agent Design
- **code-reviewer** - Main review Agent
- **security-reviewer** - Security specialist
- **tdd-guide** - Test quality assessment
> Reference: [/code-review](../参考ドキュメント/commands/01-核心工作流.md) | [code-reviewer Agent](../参考ドキュメント/agents/1.%20代码审查类.md)

### Output Format
```json
{
  "summary": { "critical": "0", "high": "2", "medium": "5" },
  "issues": [
    { "type": "security", "severity": "high", "file": "src/auth.js", "line": 42 }
  ]
}
```

## Acceptance Criteria

- [ ] Support JavaScript/TypeScript review
- [ ] Detection rate > 80%
- [ ] False positive rate < 10%
- [ ] Friendly report format
- [ ] Git workflow integration

---

[Return to practical projects directory](./README.md)
