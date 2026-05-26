# 测试运行器

本文档介绍 ECC 项目的测试结构和运行方式。

## 测试入口

**路径**: `tests/run-all.js`

集中式测试运行器,自动发现并运行所有测试文件。

### 使用方法

```bash
# 运行所有测试
node tests/run-all.js

# 运行单个测试文件
node tests/lib/utils.test.js
node tests/hooks/hooks.test.js
```

### 测试文件发现

测试运行器通过 glob 模式 `tests/**/*.test.js` 自动发现测试文件。

规则:
- 文件必须位于 `tests/` 目录下
- 文件名必须以 `.test.js` 结尾
- 目录结构会被保留用于显示

### 输出格式

```
╔══════════════════════════════════════════════════════════╗
║           Everything Claude Code - Test Suite             ║
╚══════════════════════════════════════════════════════════╝

━━━ Running lib/utils.test.js ━━━
(测试输出...)
━━━ Running hooks/hooks.test.js ━━━
(测试输出...)

╔══════════════════════════════════════════════════════════╗
║                     Final Results                        ║
╠══════════════════════════════════════════════════════════╣
║  Total Tests:    42                                      ║
║  Passed:         42  ✓                                   ║
║  Failed:         0                                        ║
╚══════════════════════════════════════════════════════════╝
```

### 测试统计

测试运行器会解析每个测试文件的输出来汇总统计:
- `Passed: <数量>`
- `Failed: <数量>`

---

## 测试结构

### 目录布局

```
tests/
├── run-all.js              # 主测试运行器
├── lib/                    # 库工具测试
│   ├── utils.test.js
│   ├── package-manager.test.js
│   └── ...
├── hooks/                  # Hook 集成测试
│   └── hooks.test.js
├── integration/            # 集成测试
├── scripts/                # 脚本测试
├── commands/               # 命令测试
├── docs/                   # 文档测试
├── opencode-config.test.js
├── plugin-manifest.test.js
├── test_*.py               # Python 测试
└── conftest.py             # pytest 配置
```

### 测试类型

1. **单元测试** (`*.test.js`)
   - 测试独立函数和工具
   - 使用 Node.js 原生 assert 或 Jest 风格

2. **集成测试** (`tests/integration/`)
   - 测试多个组件协作
   - 测试钩子完整流程

3. **Python 测试** (`test_*.py`)
   - pytest 格式的 Python 测试
   - 使用 `conftest.py` 配置

---

## 测试配置

### Node.js 测试

测试文件使用标准的 Node.js assert 模块:

```javascript
const assert = require('assert');

test('example test', () => {
  assert.strictEqual(actual, expected, 'Optional message');
});
```

### Python 测试 (pytest)

位置: `tests/conftest.py`

pytest 配置文件,提供共享的 fixtures 和配置。

```bash
# 运行 Python 测试
python -m pytest tests/

# 运行特定测试
python -m pytest tests/test_builder.py
```

---

## 测试最佳实践

### AAA 模式

```
test('测试描述', () => {
  // Arrange - 准备测试数据
  const input = 'test data';

  // Act - 执行被测操作
  const result = myFunction(input);

  // Assert - 验证结果
  assert.strictEqual(result, expected);
});
```

### 测试命名

使用描述性名称,清晰说明测试的行为:

```javascript
test('returns empty array when no markets match query', () => {});
test('throws error when API key is missing', () => {});
test('falls back to substring search when Redis is unavailable', () => {});
```

### 测试隔离

每个测试应该:
- 独立运行,不依赖其他测试
- 清理自己的测试数据
- 使用 mock 隔离外部依赖

---

## CI 集成

在 `package.json` 中配置测试脚本:

```json
{
  "scripts": {
    "test": "validate-commands.js && tests/run-all.js"
  }
}
```

测试运行器设计为可在 CI 环境中工作:
- 非零退出码表示失败
- JSON 输出用于自动化解析
- 清晰的错误消息用于调试
