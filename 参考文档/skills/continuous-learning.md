---
name: continuous-learning
description: "[已弃用 - 请使用continuous-learning-v2] 旧的v1停止钩子技能提取器。v2是严格超集，具有基于本能的、项目范围的、钩子可靠的学习。不要调用v1；将持续学习、会话学习和模式提取请求路由到continuous-learning-v2。"
origin: ECC
---

# 持续学习技能 - 已弃用

> **2026-04-28弃用。** 请改用`continuous-learning-v2`。v2是严格超集：停止钩子观察变为PreToolUse/PostToolUse观察，完整技能变为带置信度评分的原子本能，全局唯一存储变为项目范围加全局提升。
>
> 本文件保留用于归档参考和与现有安装的向后兼容。

---

## 原始v1文档（归档）

自动评估Claude Code会话以提取可重用的模式，这些模式可保存为学到的技能。

## 激活时机

- 设置从Claude Code会话自动提取模式
- 配置会话评估的停止钩子
- 查看或策展`~/.claude/skills/learned/`中学到的技能
- 调整提取阈值或模式类别
- 比较v1（本文）和v2（基于本能）方法

## 状态

此v1技能仍受支持，但`continuous-learning-v2`是新安装的首选路径。在明确需要更简单的停止钩子提取流程或需要与旧版已学技能工作流兼容时保留v1。

## 工作原理

此技能在每个会话结束时作为**停止钩子**运行：

1. **会话评估**：检查会话是否有足够的消息（默认：10+）
2. **模式检测**：从会话中识别可提取的模式
3. **技能提取**：将有用的模式保存到`~/.claude/skills/learned/`

## 配置

编辑`config.json`进行自定义：

```json
{
  "min_session_length": 10,
  "extraction_threshold": "medium",
  "auto_approve": false,
  "learned_skills_path": "~/.claude/skills/learned/",
  "patterns_to_detect": [
    "error_resolution",
    "user_corrections",
    "workarounds",
    "debugging_techniques",
    "project_specific"
  ],
  "ignore_patterns": [
    "simple_typos",
    "one_time_fixes",
    "external_api_issues"
  ]
}
```

## 模式类型

| 模式 | 描述 |
|---------|-------------|
| `error_resolution` | 特定错误的解决方法 |
| `user_corrections` | 用户纠正的模式 |
| `workarounds` | 框架/库怪癖的解决方案 |
| `debugging_techniques` | 有效的调试方法 |
| `project_specific` | 项目特定约定 |

## 钩子设置

添加到您的`~/.claude/settings.json`：

```json
{
  "hooks": {
    "Stop": [{
      "matcher": "*",
      "hooks": [{
        "type": "command",
        "command": "~/.claude/skills/continuous-learning/evaluate-session.sh"
      }]
    }]
  }
}
```

## 为什么使用停止钩子？

- **轻量级**：在会话结束时运行一次
- **非阻塞**：不会为每条消息增加延迟
- **完整上下文**：可访问完整会话记录

## 相关

- [长文指南](https://x.com/affaanmustafa/status/2014040193557471352) - 关于持续学习的章节
- `/learn`命令 - 会话中手动模式提取

---

## 比较说明（研究：2025年1月）

### vs Homunculus

Homunculus v2采用更复杂的方法：

| 特性 | 我们的方法 | Homunculus v2 |
|---------|--------------|---------------|
| 观察 | 停止钩子（会话结束） | PreToolUse/PostToolUse钩子（100%可靠） |
| 分析 | 主上下文 | 后台智能体（Haiku） |
| 粒度 | 完整技能 | 原子"本能" |
| 置信度 | 无 | 0.3-0.9加权 |
| 演进 | 直接到技能 | 本能 → 聚类 → 技能/命令/智能体 |
| 共享 | 无 | 导出/导入本能 |

**来自homunculus的关键洞察：**
> "v1依赖技能进行观察。技能是概率性的——它们触发率约为50-80%。v2使用钩子进行观察（100%可靠），并将本能作为学习行为的原子单位。"

### 潜在的v2增强

1. **基于本能的学习** - 带置信度评分的更小、原子级行为
2. **后台观察者** - 并行分析的Haiku智能体
3. **置信度衰减** - 本能若被反驳则降低置信度
4. **领域标签** - code-style、testing、git、debugging等
5. **演进路径** - 将相关本能聚类为技能/命令

参见：`docs/continuous-learning-v2-spec.md`完整规格。