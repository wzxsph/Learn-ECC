# 工作流速查

## 常用工作流Şablon

### 1. TDD 开发流程

```
需求 → 写Test(RED) → Gerçekleştir(GREEN) → 重构(REFACTOR)
```

**使用**：`/tdd`

---

### 2. Kod İnceleme流程

```
提交代码 → 启动审查 → 问题分类 → 修复 → 再次审查 → 合并
```

**使用**：`/code-review`

---

### 3. 错误修复流程

```
发现错误 → 分析原因 → 定位代码 → 修复 → Doğrulama → Test
```

**使用**：`/build-fix`

---

### 4. Skill 生成流程

```
分析需求 → 参考已有模式 → 编写Şablon → TestDoğrulama
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

[Geri DönHızlı ReferansDizin](./README.md)
