# 工作流速查

## 常用工作流模板

### 1. TDD 开发流程

```
需求 → 写测试(RED) → 实现(GREEN) → 重构(REFACTOR)
```

**使用**：`/tdd`

---

### 2. 代码审查流程

```
提交代码 → 启动审查 → 问题分类 → 修复 → 再次审查 → 合并
```

**使用**：`/code-review`

---

### 3. 错误修复流程

```
发现错误 → 分析原因 → 定位代码 → 修复 → 验证 → 测试
```

**使用**：`/build-fix`

---

### 4. Skill 生成流程

```
分析需求 → 参考已有模式 → 编写模板 → 测试验证
```

**使用**：`/skill-create`

---

## 自定义工作流

工作流定义格式：

```yaml
name: my-workflow
description: 我的工作流
steps:
  - name: step1
    command: do-something
  - name: step2
    command: do-other
    requires: [step1]
```

---

[返回快速参考目录](./README.md)
