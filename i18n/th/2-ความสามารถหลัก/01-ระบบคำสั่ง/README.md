# 01 命令系统精通

> 掌握 Claude Code 的 75+ 命令，灵活组合实现高效开发

## 模块目标

完成本模块后，你将能够：

- 识别并使用所有命令分类下的命令
- 根据场景选择最合适的命令
- 组合多个命令创建自定义工作流
- 使用命令文档快速查找命令用法

## 命令概览

Claude Code 提供了 75+ 命令，涵盖以下分类：

| 分类 | 命令数 | 示例命令 |
|------|--------|----------|
| 开发流程 | 15+ | `/plan`, `/tdd`, `/build-fix`, `/verify` |
| 代码审查 | 10+ | `/code-review`, `/security-review`, `/python-review` |
| 构建测试 | 15+ | `/go-build`, `/rust-build`, `/flutter-build` |
| 项目管理 | 10+ | `/project-init`, `/projects`, `/santa-loop` |
| 技能管理 | 10+ | `/skill-create`, `/skill-stocktake`, `/skill-scout` |
| 学习研究 | 10+ | `/learn`, `/learn-eval`, `/deep-research` |
| 辅助工具 | 15+ | `/config`, `/jira`, `/pr`, `/e2e` |

## 命令文档结构

每个命令都有标准的文档结构：

```
命令名称
├── 命令描述
├── 使用场景
├── 参数说明
├── 使用示例
└── 注意事项
```

## 常用命令速查

### 开发流程
- `/plan` - 创建实施计划
- `/tdd` - 测试驱动开发
- `/build-fix` - 修复构建错误
- `/verify` - 验证代码变更
- `/review` - 代码审查

### 构建相关
- `/go-build`, `/rust-build`, `/kotlin-build` - 各种语言构建
- `/cpp-build`, `/flutter-build` - 编译型语言构建
- `/docker-build` - Docker 镜像构建

### 项目管理
- `/project-init` - 初始化新项目
- `/projects` - 列出所有项目
- `/santa-loop` - 自动化任务循环

### 技能开发
- `/skill-create` - 从 git 历史创建 Skill
- `/skill-scout` - 搜索可用 Skills
- `/learn` - 从会话提取模式

## 如何使用命令文档

配套的完整命令文档位于：`../../เอกสารอ้างอิง/commands/01-Workflowหลัก.md`

文档包含：
- 每个命令的详细说明
- 参数和选项说明
- 使用示例和输出
- 最佳实践建议

## 组合使用命令

命令可以组合使用，实现复杂工作流：

```
/plan → /tdd → /build-fix → /code-review → /verify
```

每个命令的输出可以传递给下一个命令，形成自动化流水线。

## 下一步

- 学习[练习题目](./exercises/练习.md)
- 阅读[命令完整文档](../../เอกสารอ้างอิง/commands/01-Workflowหลัก.md)