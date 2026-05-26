# Project 2: Code Review

Build an automated code review pipeline using ECC.

## Project Goal

**Estimated Duration**: 2-3 hours

Create an automated code review system that automatically checks code quality, security issues, and standards compliance.

## Functional Requirements

### 1. Review Dimensions
- **Code Quality** - Complexity, duplication, naming
- **Security Review** - Vulnerabilities, sensitive information, injection
- **Standards Check** - Coding standards, documentation completeness
- **Test Coverage** - Coverage rate, test quality

### 2. Review Flow
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
- **tdd-guide** - Test quality evaluation
> Reference: [/code-review](../Reference-Docs/commands/01-Core-Workflow.md) | [code-reviewer Agent](../Reference-Docs/agents/01-Code-Review.md)

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

- [ ] Supports JavaScript/TypeScript review
- [ ] Detection rate > 80%
- [ ] False positive rate < 10%
- [ ] Report format is user-friendly
- [ ] Integrated with Git workflow

---

[Return to Practical Projects README](./README.md)