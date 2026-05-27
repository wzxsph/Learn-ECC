---
name: security-scan
description: Scan your Claude Code configuration (.claude/ directory) for security vulnerabilities, misconfigurations, and injection risks using AgentShield. Checks CLAUDE.md, settings.json, MCP servers, hooks, and agent definitions.
origin: ECC
---

# 安全扫描技能

使用 [AgentShield](https://github.com/affaan-m/agentshield) 审计你的 Claude Code 配置中的安全问题。

## 激活时机

- 设置新的 Claude Code 项目时
- 修改 `.claude/settings.json`、`CLAUDE.md` 或 MCP 配置之后
- 提交配置更改之前
- 加入已有 Claude Code 配置的新代码库时
- 定期安全检查

## 扫描范围

| 文件 | 检查项 |
|------|--------|
| `CLAUDE.md` | 硬编码密钥、自动执行指令、提示注入模式 |
| `settings.json` | 过度宽松的允许列表、缺失的拒绝列表、危险绕过标志 |
| `mcp.json` | 有风险的 MCP 服务器、硬编码环境密钥、npx 供应链风险 |
| `hooks/` | 通过插值实现的命令注入、数据泄露、静默错误抑制 |
| `agents/*.md` | 无限制的工具访问、提示注入面、缺失的模型规格 |

## 前置条件

需要安装 AgentShield。如未安装，请检查并安装：

```bash
# 检查是否已安装
npx ecc-agentshield --version

# 全局安装（推荐）
npm install -g ecc-agentshield

# 或直接通过 npx 运行（无需安装）
npx ecc-agentshield scan .
```

## 使用方法

### 基本扫描

针对当前项目的 `.claude/` 目录运行扫描：

```bash
# 扫描当前项目
npx ecc-agentshield scan

# 扫描指定路径
npx ecc-agentshield scan --path /path/to/.claude

# 设置最低严重级别过滤
npx ecc-agentshield scan --min-severity medium
```

### 输出格式

```bash
# 终端输出（默认）— 带评级的彩色报告
npx ecc-agentshield scan

# JSON — 用于 CI/CD 集成
npx ecc-agentshield scan --format json

# Markdown — 用于文档
npx ecc-agentshield scan --format markdown

# HTML — 自包含深色主题报告
npx ecc-agentshield scan --format html > security-report.html
```

### 自动修复

自动应用安全修复（仅修复标记为可自动修复的项目）：

```bash
npx ecc-agentshield scan --fix
```

此举将：
- 用环境变量引用替换硬编码的密钥
- 将通配符权限收紧为作用域替代方案
- 不会修改仅手动建议的项目

### Opus 4.6 深度分析

运行对抗性三代理管道进行更深入的分析：

```bash
# 需要 ANTHROPIC_API_KEY
export ANTHROPIC_API_KEY=your-key
npx ecc-agentshield scan --opus --stream
```

这将运行：
1. **攻击者（红队）** — 寻找攻击向量
2. **防御者（蓝队）** — 推荐加固方案
3. **审计员（最终裁决）** — 综合双方观点

### 初始化安全配置

从头开始构建新的安全 `.claude/` 配置：

```bash
npx ecc-agentshield init
```

创建内容：
- `settings.json`（包含作用域权限和拒绝列表）
- `CLAUDE.md`（包含安全最佳实践）
- `mcp.json` 占位符

### GitHub Action

添加到你的 CI 管道：

```yaml
- uses: affaan-m/agentshield@v1
  with:
    path: '.'
    min-severity: 'medium'
    fail-on-findings: true
```

## 严重级别

| 等级 | 评分 | 含义 |
|------|------|------|
| A | 90-100 | 安全配置 |
| B | 75-89 | 轻微问题 |
| C | 60-74 | 需要注意 |
| D | 40-59 | 重大风险 |
| F | 0-39 | 严重漏洞 |

## 结果解读

### 严重发现（立即修复）
- 配置文件中硬编码的 API 密钥或令牌
- 允许列表中的 `Bash(*)`（无限制 shell 访问）
- 通过 `${file}` 插值在钩子中实现的命令注入
- 运行 shell 的 MCP 服务器

### 高风险发现（生产前修复）
- CLAUDE.md 中的自动执行指令（提示注入向量）
- 权限中缺少拒绝列表
- 具有不必要 Bash 访问权限的代理

### 中等风险发现（建议修复）
- 钩子中的静默错误抑制（`2>/dev/null`、`|| true`）
- 缺少 PreToolUse 安全钩子
- MCP 服务器配置中的 `npx -y` 自动安装

### 信息级发现（注意事项）
- MCP 服务器缺少描述
- 正确标记为良好实践的禁止性指令

## 相关链接

- **GitHub**: [github.com/affaan-m/agentshield](https://github.com/affaan-m/agentshield)
- **npm**: [npmjs.com/package/ecc-agentshield](https://www.npmjs.com/package/ecc-agentshield)