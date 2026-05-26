# 会话管理命令

## 概述

会话管理命令用于保存、恢复和管理 Claude Code 会话状态。这些命令对于在长会话中保持上下文、在会话之间交接工作、以及追踪进度至关重要。

## 命令列表

### /save-session

**用途**: 保存会话状态 - 将当前会话的完整上下文写入日期文件

**描述**: 将当前会话的所有上下文保存到 `~/.claude/session-data/` 目录下的日期文件中，包括：构建了什么、什么工作了、什么失败了、剩余工作、文件状态、决策记录和待解决 blockers。

**会话文件命名规范**:
- 格式: `YYYY-MM-DD-<short-id>-session.tmp`
- 有效字符: 字母 `a-z`/`A-Z`、数字 `0-9`、连字符 `-`、下划线 `_`
- 最小长度: 1 字符（但不建议过短）
- 推荐: 8+ 字符的小写字母/数字/连字符组合避免同日冲突

**使用时机**:
- 工作结束时（会话结束前）
- 接近上下文限制前（先保存，再开新会话）
- 复杂问题解决后（保存成功模式）
- 需要交接给未来会话时

**工作流**:
1. **收集上下文** - 读取所有修改过的文件，审查讨论内容
2. **创建目录** - `mkdir -p ~/.claude/session-data`
3. **写入文件** - 创建带时间戳的会话文件
4. **展示给用户** - 显示内容并请求确认

**会话文件格式**:

```markdown
# Session: YYYY-MM-DD

**Started:** [approximate time if known]
**Last Updated:** [current time]
**Project:** [project name or path]
**Topic:** [one-line summary of what this session was about]

---

## What We Are Building
[1-3 paragraphs describing the feature, bug fix, or task]

## What WORKED (with evidence)
- **[thing that works]** — confirmed by: [specific evidence]

## What Did NOT Work (and why)
- **[approach tried]** — failed because: [exact reason / error message]

## What Has NOT Been Tried Yet
- [approach / idea]

## Current State of Files
| File              | Status         | Notes                      |
| ----------------- | -------------- | -------------------------- |
| `path/to/file.ts` | PASS: Complete | [what it does]             |

## Decisions Made
- **[decision]** — reason: [why this was chosen over alternatives]

## Blockers & Open Questions
- [blocker / open question]

## Exact Next Step
[The single most important thing to do when resuming]
```

**关键原则**:
- "What Did NOT Work" 部分是最重要的 - 未来会话会盲目重试失败的方法
- 每个会话独立文件 - 绝不追加到之前会话
- 文件供下次会话通过 `/resume-session` 读取

**最佳实践**:
- 中途保存时（不是结束时），清楚标记进行中项目
- 包含具体错误信息和失败原因（"threw X error because Y"）
- 下一具体步骤要足够精确，无需思考就知道从哪开始

---

### /resume-session

**用途**: 恢复保存的会话

**描述**: 从 `~/.claude/session-data/` 加载并恢复之前保存的会话上下文。

**使用场景**:
- 开始新会话时
- 需要继续之前的工作
- 需要回顾之前的问题解决过程

---

### /sessions

**用途**: 浏览+搜索+管理会话历史

**描述**: 浏览、搜索和管理所有已保存的会话历史。

**功能**:
- 列出所有会话
- 按日期搜索
- 管理会话别名
- 清理旧会话

---

### /checkpoint

**用途**: 创建/验证工作流检查点

**描述**: 在工作流关键节点创建检查点，确保进度可追溯。

**使用场景**:
- 重要步骤完成时
- 需要验证关键里程碑
- 多阶段任务中

---

### /aside

**用途**: 回答侧问而不丢失上下文

**描述**: 切换到侧向对话模式，处理次要问题后无缝返回主任务。

**使用场景**:
- 需要问一个快速问题
- 需要查阅文档
- 需要执行辅助任务

---

## 相关命令

- `/save-session` - 保存当前会话
- `/resume-session` - 恢复会话
- `/sessions` - 管理会话历史
