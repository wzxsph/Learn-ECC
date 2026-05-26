# 项目四：Agent 系统

构建多 Ajans İşbirliği平台。

## 项目Hedef

**预计时长**: 2-3 小时

创建一个多 Ajans İşbirliği系统，多个专业 Ajan 协同Bitir复杂Görev。

## 功能需求

### 1. Ajan Rol
- **architect** - Sistem Tasarımı
- **developer** - 代码Gerçekleştir
- **code-reviewer** - Kod İnceleme
- **tester** - Test工程师
> 参考: [Kod İnceleme Agent](../ReferansBelgeleri/agents/1.%20Kod İnceleme.md) | [Mimari Agent](../ReferansBelgeleri/agents/6.%20Mimari.md)

### 2. 协作机制
- Görev分发
- 结果汇总
- 冲突解决
- 状态同步

### 3. Yönet功能
- Ajan 生命周期Yönet
- Görev队列
- 性能监控
- 资源调度

## 技术方案

### 消息协议

```json
{
  "type": "task",
  "from": "architect",
  "to": "developer",
  "payload": { "task": "implement-auth", "context": {} }
}
```

### Görev状态机
- `pending` - 等待分配
- `running` - 执行中
- `completed` - 已Bitir
- `failed` - 失败
- `blocked` - 阻塞

## Kabul Kriterleri

- [ ] Destekle 4+ 种 Ajan Rol
- [ ] Görev自动分发
- [ ] 结果正确汇总
- [ ] 失败恢复机制
- [ ] 实时状态监控

---

[Geri DönGerçek ProjelerDizin](./README.md)
