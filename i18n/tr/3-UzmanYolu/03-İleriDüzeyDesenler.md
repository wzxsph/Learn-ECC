# 03-İleri Düzey Kalıplar

掌握 ECC 的高级Tasarım模式和En İyi Uygulamalar。

## 模式分类

> 参考: [En İyi Uygulamalar](../ReferansBelgeleri/skills/En İyi Uygulamalar.md)

### 1. 工作流模式
- **Borulu Kalıp** - 数据流经多个处理阶段
- **广播模式** - 一个Görev分发到多个执行者
- **回压模式** - 控制Görev生产消费速率

### 2. Ajan 模式
- **层级模式** - Ana Ajan 协调Alt Ajans
- **市场模式** - Agents Vasıtasıyla竞价获取Görev
- **蜂巢模式** - 大量同类 Agents 协作

### 3. Hata Yönetimi模式
- **熔断器模式** - 失败后快速失败
- **重试模式** - 指数退避重试
- **死信队列** - 无法处理的Görev暂存

## Performans Optimizasyonu

### 上下文Yönet
- 及时清理无用上下文
- 使用压缩减少 token 消耗
- 分离冷热数据

### 并发控制
- 限制并发 Ajan 数量
- 使用信号量控制资源
- Gerçekleştir优雅降级

## Güvenlik性

### 权限控制
- 最小权限原则
- 操作审计日志
- 敏感操作二次确认

### 输入Doğrulama
- 所有用户输入Doğrulama
- 参数化查询防注入
- 输出内容脱敏

## Sonraki Adım

- [Geri Dön上一章](./02-Otonom Ajanlar.md) - Otonom Ajanlar
- [Gerçek Projeler](../4-Gerçek Projeler/README.md) - 应用所学
- [Geri DönUzmanlık YoluDizin](../README.md)
