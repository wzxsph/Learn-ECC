# 测试类 Agent

测试类 Agent 专门负责测试驱动开发、端到端测试、测试覆盖率分析和质量保证。

## Agent 列表

| Agent 名称 | 用途 | 使用模型 | 核心工具 |
|------------|------|----------|----------|
| tdd-guide | TDD 测试驱动开发专家 | sonnet | Read, Write, Edit, Bash, Grep |
| e2e-runner | 端到端测试专家 | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| mle-reviewer | ML 工程代码审查（训练/推理/监控） | sonnet | Read, Grep, Glob, Bash |
| pr-test-analyzer | PR 测试覆盖分析 | sonnet | Read, Grep, Glob, Bash |

---

## tdd-guide

### 名称和用途
测试驱动开发专家，强制执行写测试优先的方法论。主动用于编写新功能、修复 bug 或重构代码。确保 80%+ 的测试覆盖率。

### 能力说明
- 强制执行测试优先的方法论
- 指导 Red-Green-Refactor 周期
- 确保 80%+ 测试覆盖率
- 编写全面的测试套件（单元、集成、E2E）
- 在实施前捕获边缘用例

### 适用场景
- 编写新功能时
- 修复 bug 时
- 重构代码时
- 需要 TDD 工作流程时

### 使用的工具列表
- Read: 读取现有代码
- Write: 编写测试
- Edit: 修改测试和实现
- Bash: 运行测试命令
- Grep: 搜索测试模式

### 与其他 Agent 的协作方式
- code-reviewer 在 TDD 周期后审查代码
- build-error-resolver 修复测试构建错误
- e2e-runner 进行关键用户流的 E2E 测试

### TDD 工作流

#### 1. 先写测试 (RED)
编写描述预期行为的失败测试。

#### 2. 运行测试 - 验证它 FAIL
```bash
npm test
```

#### 3. 写最小实现 (GREEN)
只写足够的代码让测试通过。

#### 4. 运行测试 - 验证它 PASS

#### 5. 重构 (IMPROVE)
移除重复，改进名称，优化 - 测试必须保持绿色。

#### 6. 验证覆盖率
```bash
npm run test:coverage
# 要求: 80%+ 分支、函数、行、语句覆盖率
```

### 必须测试的边缘用例

1. **Null/Undefined** 输入
2. **空** 数组/字符串
3. **无效类型** 传递
4. **边界值** (最小/最大)
5. **错误路径** (网络失败、数据库错误)
6. **竞态条件** (并发操作)
7. **大数据** (10k+ 项的性能)
8. **特殊字符** (Unicode、表情符号、SQL 字符)

### 反模式 - 必须避免

- 测试实现细节（内部状态）而非行为
- 测试相互依赖（共享状态）
- 断言太少（通过但不验证任何东西）
- 外部依赖未 mock (Supabase、Redis、OpenAI 等)

### 质量检查清单

- [ ] 所有公共函数都有单元测试
- [ ] 所有 API 端点都有集成测试
- [ ] 关键用户流有 E2E 测试
- [ ] 边缘用例已覆盖 (null、empty、invalid)
- [ ] 错误路径已测试（不只是 happy path）
- [ ] 外部依赖已 mock
- [ ] 测试相互独立（无共享状态）
- [ ] 断言具体且有意义
- [ ] 覆盖率 80%+

### v1.8 Eval-Driven TDD 附录

将 eval-driven 开发集成到 TDD 流程：

1. 在实施前定义能力 + 回归 evals
2. 运行基线并捕获失败特征
3. 实现最小通过变更
4. 重新运行测试和 evals；报告 pass@1 和 pass@3
5. 发布关键路径应在合并前达到 pass^3 稳定性

---

## e2e-runner

### 名称和用途
端到端测试专家，使用 Vercel Agent Browser（首选）和 Playwright 后备。主动用于生成、维护和运行 E2E 测试。

### 能力说明
- 测试旅程创建 - 为用户流编写测试
- 测试维护 - 保持测试与 UI 变化同步
- 不稳定测试管理 - 识别和隔离不稳定测试
- 测试报告 - 生成 HTML 报告和 JUnit XML
- CI/CD 集成 - 确保测试在管道中可靠运行
- 截图/视频/trace 管理

### 适用场景
- 关键用户旅程需要验证时
- 需要端到端验证时
- CI/CD 管道中运行 E2E 测试时
- 发现集成问题时

### 使用的工具列表
- Read: 读取页面内容
- Write: 编写测试
- Edit: 修改测试
- Bash: 运行 Playwright/Agent Browser 命令
- Grep: 搜索测试
- Glob: 查找测试文件

### 与其他 Agent 的协作方式
- tdd-guide 编写单元/集成测试
- code-reviewer 审查测试质量
- mle-reviewer 审查 ML 相关测试

### 主要工具: Agent Browser

**优先于原始 Playwright 使用 Agent Browser** - 语义选择器、AI 优化、自动等待、基于 Playwright。

```bash
# 设置
npm install -g agent-browser && agent-browser install

# 核心工作流
agent-browser open https://example.com
agent-browser snapshot -i          # 获取带 refs 的元素 [ref=e1]
agent-browser click @e1            # 按 ref 点击
agent-browser fill @e2 "text"     # 按 ref 填充输入
agent-browser wait visible @e5     # 等待元素可见
agent-browser screenshot result.png
```

### 后备: Playwright

当 Agent Browser 不可用时，直接使用 Playwright。

```bash
npx playwright test                        # 运行所有 E2E 测试
npx playwright test tests/auth.spec.ts     # 运行特定文件
npx playwright test --headed               # 可见浏览器
npx playwright test --debug                # 使用检查器调试
npx playwright test --trace on             # 带 trace 运行
npx playwright show-report                 # 查看 HTML 报告
```

### 工作流

#### 1. 计划
- 识别关键用户旅程（认证、核心功能、支付、CRUD）
- 定义场景: happy path、边缘用例、错误用例
- 按风险排序: HIGH（金融、认证）、MEDIUM（搜索、导航）、LOW（UI 优化）

#### 2. 创建
- 使用页面对象模型 (POM) 模式
- 优先使用 `data-testid` 选择器
- 在关键步骤添加断言
- 在关键时刻捕获截图
- 使用适当的等待（从不 `waitForTimeout`）

#### 3. 执行
- 本地运行 3-5 次检查不稳定性
- 使用 `test.fixme()` 或 `test.skip()` 隔离不稳定测试
- 上传 artifact 到 CI

### 关键原则

- **使用语义选择器**: `[data-testid="..."]` > CSS 选择器 > XPath
- **等待条件而非时间**: `waitForResponse()` > `waitForTimeout()`
- **自动等待内置**: `page.locator().click()` 自动等待；原始 `page.click()` 不等待
- **隔离测试**: 每个测试应该独立；无共享状态
- **快速失败**: 在每个关键步骤使用 `expect()` 断言
- **重试时 trace**: 配置 `trace: 'on-first-retry'` 用于调试失败

### 不稳定测试处理

```typescript
// 隔离
test('flaky: market search', async ({ page }) => {
  test.fixme(true, 'Flaky - Issue #123')
})

// 识别不稳定性
// npx playwright test --repeat-each=10
```

常见原因: 竞态条件（使用自动等待选择器）、网络计时（等待响应）、动画计时（等待 `networkidle`）。

### 成功指标

- 所有关键旅程通过 (100%)
- 总体通过率 > 95%
- 不稳定率 < 5%
- 测试持续时间 < 10 分钟
- Artifact 上传并可访问

---

## mle-reviewer

### 名称和用途
生产机器学习工程审查专家，专注于数据契约、特征管道、训练可复现性、离线/在线评估、模型服务、监控和回滚。

### 能力说明
- 问题定义和决策质量审查
- 指标、阈值和错误分析审查
- 数据契约和泄漏检查
- 训练可复现性验证
- 评估和推广流程审查
- 服务和部署安全审查
- 监控和事件响应规划

### 适用场景
- ML、MLOps、模型训练代码变更时
- 推理、特征存储、评估代码变更时
- 模型推广决策时

### 使用的工具列表
- Read: 读取 ML 代码和配置
- Grep: 搜索模式
- Glob: 查找文件
- Bash: 运行 pytest, ruff, mypy

### 与其他 Agent 的协作方式
- python-reviewer 处理 Python 风格、类型、错误处理
- pytorch-build-resolver 处理 tensor/CUDA/训练失败
- database-reviewer 处理特征表、标签存储
- security-reviewer 处理 secrets、PII、pickle Segurança
- performance-optimizer 处理延迟、内存、GPU 利用率
- build-error-resolver 处理 CI、依赖、原生扩展失败
- pr-test-analyzer 分析测试覆盖
- silent-failure-hunter 发现管道静默失败
- e2e-runner 进行产品流 E2E 测试
- a11y-architect 审查预测可访问性
- doc-updater 更新文档

### 关键审查领域

#### 问题定义和决策质量
- 变更是否从用户或系统决策开始，而非模型架构偏好
- 利益相关者和失败成本是否明确
- 指标选择是否遵循错误预算

#### 指标、阈值和错误分析
- 基线和当前生产行为是否比较
- 阈值和配置是否作为产品决策
- 假阳性和假阴性是否直接检查

#### 数据契约和泄漏
- 实体粒度、主键、时间戳是否明确
- 分割是否遵守时间、用户/实体分组
- 特征连接是否时间点正确
- 敏感属性是否排除或正当处理

#### 训练可复现性
- 训练是否可从代码、配置、数据集版本、种子运行
- 超参数、预处理、依赖版本是否记录
- 随机性和 GPU 非确定性是否有意处理

#### 评估和推广
- 指标是否与基线和当前生产模型比较
- 推广门是否在选择前声明
- 切片指标是否覆盖重要队列

#### 服务和部署
- 训练和服务转换是否共享或等价测试
- 输入模式是否拒绝过时、缺失、无效特征
- 推理路径是否有超时、资源限制、回退逻辑

#### 监控和事件响应
- 监控是否覆盖服务健康、特征漂移、预测漂移
- 日志是否包含足够标识符
- 回滚是否命名前一 artifact、配置、数据依赖

### 常见阻塞器

- 在时间相关或用户相关数据上随机 train/test 分割
- 特征生成使用预测时不可用的字段
- 离线指标改进而关键切片回归
- 训练预处理被手动复制到服务代码
- 模型版本不在预测日志中

### 批准标准

- **APPROVE**: 无关键/高 MLE 风险，相关测试或评估门通过
- **APPROVE WITH WARNINGS**: 只有中等问题，有明确的后续行动
- **BLOCK**: 任何可能的泄漏、不可复现的推广、不安全的服务行为、缺少生产部署回滚、敏感数据暴露、或关键评估差距

---

## pr-test-analyzer

### 名称和用途
审查 Pull Request 测试覆盖质量和完整性，强调行为覆盖和真实 bug 预防。

### 能力说明
- 识别变更代码映射
- 验证行为覆盖
- 评估测试质量
- 发现覆盖差距

### 适用场景
- PR 审查时
- 评估测试覆盖率时
- 验证测试充分性时

### 使用的工具列表
- Read: 读取变更代码和测试
- Grep: 搜索测试
- Glob: 查找测试文件
- Bash: 运行测试命令

### 与其他 Agent 的协作方式
- tdd-guide 改进测试
- e2e-runner 进行端到端覆盖
- code-reviewer 进行代码质量审查

### 分析流程

#### 1. 识别变更代码
- 映射变更的函数、类和模块
- 定位对应测试
- 识别新的未测试代码路径

#### 2. 行为覆盖
- 检查每个功能是否有测试
- 验证边缘用例和错误路径
- 确保重要集成被覆盖

#### 3. 测试质量
- 优先选择有意义的断言而非无抛出检查
- 标记不稳定的模式
- 检查隔离性和测试名称清晰度

#### 4. 覆盖差距
按影响评级：
- critical（关键）
- important（重要）
- nice-to-have（锦上添花）

### 输出格式

1. 覆盖总结
2. 关键差距
3. 改进建议
4. 积极观察
[返回 Agent 索引](../README.md)
