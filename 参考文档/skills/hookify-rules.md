---
name: hookify-rules
description: 当用户要求创建hookify规则、编写钩子规则、配置hookify、添加hookify规则，或需要有关hookify规则语法和模式的指导时，应使用此技能。
---

# 编写Hookify规则

## 概述

Hookify规则是带有YAML前置数据的markdown文件，定义了要监视的模式以及这些模式匹配时显示的消息。规则存储在`.claude/hookify.{规则名称}.local.md`文件中。

## 规则文件格式

### 基本结构

```markdown
---
name: rule-identifier
enabled: true
event: bash|file|stop|prompt|all
pattern: regex-pattern-here
---

当此规则触发时向Claude显示的消息。
可以包含markdown格式、警告、建议等。
```

### 前置数据字段

| 字段 | 必填 | 值 | 描述 |
|-------|----------|--------|---------|
| name | 是 | kebab-case字符串 | 唯一标识符（动词优先：warn-*、block-*、require-*） |
| enabled | 是 | true/false | 切换开关，无需删除 |
| event | 是 | bash/file/stop/prompt/all | 触发此规则的钩子事件 |
| action | 否 | warn/block | warn（默认）显示消息；block阻止操作 |
| pattern | 是* | 正则字符串 | 要匹配的模式（*或对复杂规则使用conditions） |

### 高级格式（多条件）

```markdown
---
name: warn-env-api-keys
enabled: true
event: file
conditions:
  - field: file_path
    operator: regex_match
    pattern: \.env$
  - field: new_text
    operator: contains
    pattern: API_KEY
---

你正在向.env文件添加API密钥。确保此文件在.gitignore中！
```

**按事件类型的条件字段：**
- bash: `command`
- file: `file_path`、`new_text`、`old_text`、`content`
- prompt: `user_prompt`

**操作符：** `regex_match`、`contains`、`equals`、`not_contains`、`starts_with`、`ends_with`

所有条件必须匹配规则才能触发。

## 事件类型指南

### bash事件
匹配Bash命令模式：
- 危险命令：`rm\s+-rf`、`dd\s+if=`、`mkfs`
- 权限提升：`sudo\s+`、`su\s+`
- 权限问题：`chmod\s+777`

### file事件
匹配Edit/Write/MultiEdit操作：
- 调试代码：`console\.log\(`、`debugger`
- 安全风险：`eval\(`、`innerHTML\s*=`
- 敏感文件：`\.env$`、`credentials`、`\.pem$`

### stop事件
完成检查和提醒。模式`.*`始终匹配。

### prompt事件
匹配用户提示内容以强制执行工作流。

## 模式编写技巧

### 正则基础
- 转义特殊字符：`.` 转为 `\.`、`(` 转为 `\(`
- `\s` 空白、`\d` 数字、`\w` 单词字符
- `+` 一个或多个、`*` 零个或多个、`?` 可选
- `|` 或操作符

### 常见陷阱
- **太宽泛**：`log` 会匹配 "login"、"dialog" — 使用 `console\.log\(`
- **太具体**：`rm -rf /tmp` — 使用 `rm\s+-rf`
- **YAML转义**：使用未加引号的模式；带引号的字符串需要 `\\s`

### 测试
```bash
python3 -c "import re; print(re.search(r'your_pattern', 'test text'))"
```

## 文件组织

- **位置**：项目根目录的`.claude/`目录
- **命名**：`.claude/hookify.{描述性名称}.local.md`
- **Gitignore**：将`.claude/*.local.md`添加到`.gitignore`

## 命令

- `/hookify [描述]` - 创建新规则（无参数时自动分析会话）
- `/hookify-list` - 以表格格式查看所有规则
- `/hookify-configure` - 交互式开关规则开/关
- `/hookify-help` - 完整文档

## 快速参考

最小可行规则：
```markdown
---
name: my-rule
enabled: true
event: bash
pattern: dangerous_command
---
此处显示警告消息
```