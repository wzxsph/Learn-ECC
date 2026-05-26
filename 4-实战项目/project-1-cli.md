# 项目一：CLI 工具

使用 ECC 构建一个命令行工具。

## 项目目标

**预计时长**: 2-3 小时

创建一个 `ecc-gen` 工具，用于生成 ECC 项目组件（Agent、Skill、Hook）。

## 功能需求

### 1. 核心功能
- `ecc-gen agent <name>` - 生成 Agent 文件
- `ecc-gen skill <name>` - 生成 Skill 文件
- `ecc-gen hook <name>` - 生成 Hook 配置

### 2. 交互功能
- 交互式问答生成
- 模板选择
- 输出路径指定

### 3. 高级功能
- 批量生成
- 自定义模板
- 配置文件支持

## 技术方案

### 目录结构

```
ecc-gen/
├── src/
│   ├── commands/
│   │   ├── agent.js
│   │   ├── skill.js
│   │   └── hook.js
│   ├── generators/
│   │   └── templates/
│   └── index.js
├── package.json
└── README.md
```

### 使用库
- `commander` - CLI 参数解析
- `inquirer` - 交互式问答
- `ejs` - 模板引擎

## 验收标准

- [ ] 三个子命令均可正常工作
- [ ] 生成的模板格式正确
- [ ] 支持 `--output` 指定输出路径
- [ ] 包含单元测试
- [ ] 文档完整

## 学习要点

- CLI 工具开发
- 模板系统设计
- 交互式输入处理
- 模块化架构

> 参考命令: [/build-fix](../参考文档/commands/04-构建修复.md) | [/test](../参考文档/commands/02-测试相关.md)

---

[返回实战项目目录](./README.md)
