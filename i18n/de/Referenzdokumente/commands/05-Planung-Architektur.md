# 规划与架构命令

## 概述

规划与架构命令用于产品规划、功能设计和系统架构决策。这些命令确保在写代码之前有清晰的计划，减少返工和提高代码质量。

## 命令列表

### /plan

**用途**: 通用实施规划 - 复述需求、评估风险、创建分步骤实施计划

**描述**: 在触碰任何代码之前，等待用户确认后创建全面的实施计划。接受自由形式需求或 PRD markdown 文件。

**输入模式**:

| 输入 | 模式 | 行为 |
|---|---|---|
| `path/to/name.prd.md` | PRD artifact 模式 | 读取 PRD，选择下一个待交付里程碑，生成 `.claude/plans/{name}.plan.md` |
| 其他 markdown 路径 | 引用模式 | 读取文件作为上下文，产生内联计划 |
| 自由形式文本 | 对话模式 | 产生内联计划 |
| 空输入 | 澄清模式 | 询问要规划什么 |

**关键参数**:
- `$ARGUMENTS`: `[feature description | path/to/*.prd.md]` - 功能描述或 PRD 文件路径

**工作流**:
1. **复述需求** - 澄清需要构建什么
2. **识别风险** - 列出潜在问题和阻碍
3. **创建步骤计划** - 分解为阶段性实施
4. **等待确认** - 必须收到用户批准后才能继续

**输出格式** (PRD artifact 模式):
```markdown
# Plan: {Feature Name}

**Source PRD**: {path}
**Selected Milestone**: {milestone or phase name}
**Complexity**: {Small | Medium | Large}

## Summary
{2-3 sentences}

## Patterns to Mirror
| Category | Source | Pattern |
|---|---|---|
| Naming | `path:line` | {short description} |
| Errors | `path:line` | {short description} |
| Tests | `path:line` | {short description} |

## Files to Change
| File | Action | Why |
|---|---|---|
| `path` | CREATE / UPDATE / DELETE | {reason} |

## Tasks
### Task 1: {name}
- **Action**: {what to do}
- **Mirror**: {pattern to follow}
- **Validate**: {command that proves correctness}

## Validation
```bash
{project-specific validation commands}
```

## Risks
| Risk | Likelihood | Mitigation |
|---|---|---|
```

**最佳实践**:
- 使用前先研究代码库中的现有模式
- 在 PRD artifact 模式中创建 `.claude/plans/` 目录（如不存在）
- 如果 PRD 包含 `Delivery Milestones` 表格，只更新所选行状态

**常见陷阱**:
- 空输入时不询问澄清就尝试规划
- 不等待用户确认就开始写代码
- 跳过 Pattern Grounding 步骤

**与其他命令集成**:
- 规划后使用 `tdd-workflow` skill 进行测试驱动开发
- 使用 `/build-fix` 修复构建错误
- 使用 `/code-review` 审查完成实现
- 使用 `/pr` 或 `/prp-pr` 打开 Pull Request

---

### /plan-prd

**用途**: 交互式 PRD 生成器

**描述**: 生成以问题为中心的产品需求文档，包含问题定义、用户画像、成功指标和 MVP 范围。

**使用场景**: 
- 确定要构建的功能
- 需要明确产品需求
- 从零开始新功能规划

**工作流**:
1. FRAME - 了解用户和问题
2. GROUND - 收集证据
3. DECIDE - 确定范围和假设
4. GENERATE - 生成 PRD 文件

---

### /prp-plan

**用途**: 全面的功能规划

**描述**: 完整的项目规划，涵盖代码库分析、风险评估和分步骤实施计划。

**使用场景**:
- 复杂功能的详细规划
- 需要代码库上下文分析
- 多阶段实施项目

---

### /prp-prd

**用途**: PRP 工作流 PRD 生成器

**描述**: 使用 PRP（Plan-Record-Produce）工作流生成产品需求文档。

---

### /prp-implement

**用途**: 执行 PRP 计划+验证循环

**描述**: 按照 PRP 工作流执行计划，包含持续的验证和检查。

---

### /prp-pr

**用途**: 从 PRP 工作流创建 PR

**描述**: 从 PRP 工作流的结果创建 GitHub Pull Request。

---

### /prp-commit

**用途**: PRP 验证提交

**描述**: 在 PRP 工作流中执行验证并提交代码。

---

### /multi-plan

**用途**: 多模型协作规划

**描述**: 结合 Codex 和 Gemini 的双模型分析，生成综合实施计划。

**使用场景**:
- 需要多角度分析
- 复杂跨领域项目
- 需要平衡前端和后端考量

---

## 相关命令

- `/plan` - 通用实施规划
- `/code-review` - 代码审查
- `/build-fix` - 构建修复
