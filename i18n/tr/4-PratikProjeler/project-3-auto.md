# 项目三：Otomatik İş Akışı

构建端到端Otomatik İş Akışı系统。

## 项目Hedef

**预计时长**: 2-3 小时

Tasarım并Gerçekleştir一个Otomatik İş Akışı引擎，处理常见开发Görev。

## 功能需求

### 1. 工作流定义
- YAML 格式定义
- Adımlar串联和并行
- 条件分支
- 循环处理

### 2. 内置工作流
- **代码检查** - lint + type-check + test
- **构建发布** - build + test + deploy
- **问题修复** - analyze + fix + verify

### 3. 监控告警
- 执行状态追踪
- 失败告警通知
- 耗时统计

## 技术方案

### 工作流定义Örnek

```yaml
name: code-check
steps:
  - name: lint
    command: npm run lint
  - name: type-check
    command: npm run type-check
  - name: test
    command: npm test
    requires: [lint, type-check]
```

### Hook 集成
- PreToolUse - 参数Doğrulama
- PostToolUse - 输出Doğrulama
> 参考: [Hook Türleri](../ReferansBelgeleri/hooks/HookTür.md) | [Otomasyon ve Betik](../ReferansBelgeleri/skills/Otomasyon ve Betik.md)

## Kabul Kriterleri

- [ ] Destekle YAML 定义
- [ ] Adımlar依赖解析正确
- [ ] Paralel Yürütme优化
- [ ] 失败重试机制
- [ ] 执行日志完整

---

[Geri DönGerçek ProjelerDizin](./README.md)
