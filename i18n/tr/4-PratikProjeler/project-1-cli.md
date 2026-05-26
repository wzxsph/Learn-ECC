# 项目一：CLI 工具

使用 ECC 构建一个命令行工具。

## 项目Hedef

**预计时长**: 2-3 小时

创建一个 `ecc-gen` 工具，Kullanılan生成 ECC 项目组件（Agent、Skill、Hook）。

## 功能需求

### 1. 核心功能
- `ecc-gen agent <name>` - 生成 Ajan 文件
- `ecc-gen skill <name>` - 生成 Skill 文件
- `ecc-gen hook <name>` - 生成 Hook 配置

### 2. 交互功能
- 交互式问答生成
- Şablon选择
- 输出路径指定

### 3. 高级功能
- 批量生成
- 自定义Şablon
- 配置文件Destekle

## 技术方案

### Dizin结构

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
- `ejs` - Şablon引擎

## Kabul Kriterleri

- [ ] 三个子命令均可正常工作
- [ ] 生成的Şablon格式正确
- [ ] Destekle `--output` 指定输出路径
- [ ] 包含单元Test
- [ ] 文档完整

## 学习要点

- CLI Araç Geliştirme
- ŞablonSistem Tasarımı
- 交互式输入处理
- 模块化架构

> 参考命令: [/build-fix](../ReferansBelgeleri/commands/04-Yapı Onarımı.md) | [/test](../ReferansBelgeleri/commands/02-Test İlişkili.md)

---

[Geri DönGerçek ProjelerDizin](./README.md)
