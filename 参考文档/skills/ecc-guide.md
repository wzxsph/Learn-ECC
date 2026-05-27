---
name: ecc-guide
description: 通过读取实时仓库内容来引导用户了解 ECC 当前的智能体、技能、命令、钩子、规则、安装配置文件和项目入门知识。
origin: community
---

# ECC 指南

当用户需要帮助了解、导航、安装或选择 Everything Claude Code 的各个部分时，使用此技能。

## 何时使用

在以下情况下使用此技能：

- 用户询问 ECC 包含哪些内容
- 用户需要帮助寻找某个技能、命令、智能体、钩子、规则或安装配置文件
- 用户是仓库新手，需要引导式入门
- 用户询问"如何使用 ECC 做 X？"
- 用户询问哪些 ECC 组件适合某个项目
- 用户需要轻量级解释，了解命令、技能、智能体、钩子和规则之间的关系
- 用户对安装路径、重复安装、重置/卸载或选择性安装感到困惑

## 核心原则

从当前文件回答，而非依赖记忆。ECC 变化很快，固化编目的数量、功能列表和安装说明会很快过时。

当 ECC 仓库可用时，在给出具体答案前先检查相关文件：

```bash
node scripts/ci/catalog.js --json
find skills -maxdepth 2 -name SKILL.md | sort
find commands -maxdepth 1 -name '*.md' | sort
find agents -maxdepth 1 -name '*.md' | sort
node scripts/install-plan.js --list-profiles
node scripts/install-plan.js --list-components --json
```

根据用户的问题，使用最小化的读取集合。

## 仓库地图

- `README.md`：安装路径、卸载/重置指南、公开定位、常见问题
- `AGENTS.md`：贡献者指南和项目结构
- `agent.yaml`：导出的 gitagent 接口和命令列表
- `commands/`：维护的命令兼容垫片
- `skills/*/SKILL.md`：可重用工作流和领域手册
- `agents/*.md`：委托的子智能体角色提示
- `rules/`：语言和工具规则
- `hooks/README.md`、`hooks/hooks.json`、`scripts/hooks/`：钩子行为和安全门
- `manifests/install-*.json`：选择性安装模块、组件、配置文件和支持目标
- `docs/`：工具指南、架构笔记、翻译文档、发布文档

## 回复风格

先给出答案，再给出下一步操作。大多数用户不需要完整的目录转储。

良好的首个回复结构：

1. 使用什么
2. 为什么适合
3. 要检查的确切文件或命令
4. 一个下一步命令或问题

避免：

- 默认列出每个技能或命令
- 重复大型自述文件部分
- 当存在技能优先路径时推荐已弃用的命令垫片
- 在不检查文件系统的情况下声称存在某个组件
- 当托管安装程序支持目标时用手动复制命令替换安装指南

## 常见任务

### 新用户入门

提供一个简短菜单：

- 安装或重置 ECC
- 为项目选择技能
- 理解命令与技能的区别
- 检查钩子和安全行为
- 运行工具审计
- 找到特定工作流

指向 `README.md` 了解安装/重置，指向 `/project-init` 了解项目特定入门。

### 功能发现

对于"X 应该用什么？"：

1. 搜索 `skills/`、`commands/` 和 `agents/`
2. 优先使用技能作为主要工作流界面
3. 只有当命令是维护的兼容垫片或用户明确需要斜杠命令行为时才使用命令
4. 在委托有用时提及智能体

有用的搜索：

```bash
rg -n "<query>" skills commands agents docs
find skills -maxdepth 2 -name SKILL.md | sort
```

### 安装指导

使用托管安装路径：

```bash
node scripts/install-plan.js --list-profiles
node scripts/install-plan.js --profile minimal --target claude --json
node scripts/install-apply.js --profile minimal --target claude --dry-run
```

对于特定技能安装：

```bash
node scripts/install-plan.js --skills <skill-id> --target claude --json
node scripts/install-apply.js --skills <skill-id> --target claude --dry-run
```

警告用户不要堆叠插件安装和完整手动/配置文件安装，除非他们故意想要重复界面。

### 项目入门

当用户想要为目标仓库配置 ECC 时，使用 `/project-init`。预期顺序是：

1. 从项目文件检测技术栈
2. 解析干运行安装计划
3. 检查现有的 `CLAUDE.md` 和设置文件
4. 申请前询问
5. 保持生成的指导最少且特定于仓库

### 故障排除

首先询问目标工具和安装路径，然后检查：

- 插件安装元数据
- `.claude/`、`.cursor/`、`.codex/`、`.gemini/`、`.opencode/`、`.codebuddy/`、`.joycode/` 或 `.qwen/`
- `hooks/hooks.json`
- 安装状态文件
- 相关命令/技能文件

对于仓库健康状况，建议：

```bash
npm run harness:audit -- --format text
npm run observability:ready
npm test
```

## 输出模板

### 简短推荐

```text
使用 <技能或命令>。它适合是因为 <原因>。

规范文件：<路径>
验证命令：<命令>
下一步：<一个具体操作>
```

### 搜索结果

```text
最佳匹配：
- <路径>：<为什么重要>
- <路径>：<为什么重要>

推荐：<首先使用哪个以及为什么>
```

### 安装计划摘要

```text
检测到：<技术栈证据>
目标：<工具>
计划：<配置文件/模块/技能>
干运行：<命令>
将更改：<路径>
需要批准后再应用：<是/否>
```

## 相关界面

- `/project-init`：针对目标仓库的栈感知入门计划
- `/harness-audit`：确定性就绪评分卡
- `/skill-health`：技能质量审查
- `/skill-create`：从本地 git 历史生成新技能
- `/security-scan`：检查 Claude/OpenCode 配置安全