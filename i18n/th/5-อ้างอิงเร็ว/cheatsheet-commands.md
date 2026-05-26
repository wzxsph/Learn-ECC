# 命令速查

## 斜杠命令

| 命令 | 功能 | 使用场景 |
|------|------|----------|
| `/tdd` | 测试驱动开发工作流 | 新功能开发 |
| `/plan` | 实现计划生成 | 复杂任务规划 |
| `/code-review` | [代码审查](../เอกสารอ้างอิง/commands/01-核心工作流.md) | 代码质量检查 |
| `/build-fix` | [构建修复](../เอกสารอ้างอิง/commands/04-构建修复.md) | 构建失败时 |
| `/learn` | 从会话提取模式 | 知识沉淀 |
| `/skill-create` | 从 Git 生成 Skill | 经验复用 |
| `/test` | [测试相关命令](../เอกสารอ้างอิง/commands/02-测试相关.md) | 测试相关 |

## 脚本命令

| 脚本 | 功能 |
|------|------|
| `node scripts/hooks/run-with-flags.js` | 带标志位运行钩子 |
| `node tests/run-all.js` | 运行全量测试 |
| `node scripts/lib/utils.js` | 工具函数库 |

## Agent 命令

| Agent | 功能 |
|-------|------|
| `/planner` | 任务规划 |
| `/code-reviewer` | 代码审查 |
| `/tdd-guide` | TDD 指导 |
| `/security-reviewer` | 安全审查 |
| `/build-error-resolver` | 构建修复 |

## 全局参数

| 参数 | 说明 |
|------|------|
| `--model` | 指定模型 |
| `--no-input` | 非交互模式 |
| `--output` | 输出格式 |

## Hook 环境变量

| 变量 | 说明 |
|------|------|
| `ECC_HOOK_PROFILE` | 钩子配置 |
| `ECC_DISABLED_HOOKS` | 禁用钩子列表 |

---

[返回快速参考目录](./README.md)
