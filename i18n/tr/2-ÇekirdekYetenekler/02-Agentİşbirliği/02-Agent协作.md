# 02 - Ajans İşbirliği

NasılTasarım、Gerçekleştir和Yönet多 Ajans İşbirliği系统。

## Genel Bakış

ECC Sağla **66 个专业[Agent](../../ReferansBelgeleri/agents/1.%20Kod İnceleme.md)**，Bölünmüş 6 大Kategori。这些 Ajan Vasıtasıyla标准化的工作流协同工作，Bitir从Kod İnceleme到Sistem Tasarımı的各类复杂Görev。

---

## 1. Ajan Mimarisi

### 1.1 Tek Ajan vs Çoklu Ajan

| 架构 | Uygulama Senaryosu | Özellikler |
|------|----------|------|
| **Tek Ajan** | 简单Görev、独立操作 | Doğrudan Çağırma、快速响应 |
| **Çoklu Ajan** | 复杂Görev、Gerekli协作 | Görev Ayrıştırma、Sonuç Birleştirme |

### 1.2 Kullanılabilir Ajan Listesi

| Ajan | Kullanım | Kullanım Zamanı |
|-------|------|----------|
| **planner** | Uygulama Planlaması | 复杂功能、重构 |
| **architect** | Sistem Tasarımı | 架构决策 |
| **tdd-guide** | Test Güdümlü Geliştirme | 新功能、bug 修复 |
| **code-reviewer** | Kod İnceleme | 编写代码后 |
| **security-reviewer** | Güvenlik Analizi | 提交前 |
| **build-error-resolver** | Yapı Hatalarını Onarma | 构建失败时 |
| **e2e-runner** | Uçtan Uca Test | 关键用户流程 |
| **refactor-cleaner** | Ölü Kod Temizleme | 代码维护 |
| **doc-updater** | Belge Güncelleme | 更新文档时 |
| **rust-reviewer** | Rust Kod İnceleme | Rust 项目 |
| **harmonyos-app-resolver** | HarmonyOS Uygulama Geliştirme | HarmonyOS/ArkTS 项目 |

### 1.3 Ajan 文件格式

```yaml
---
name: agent-name
description: Ajan Kullanım
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

---

## 2. Ajan Kategorileri详解

### 2.1 Kod İnceleme（16个）

负责Kod Kalitesi、Güvenlik和可维护性审查。

| Ajan | Uzmanlık |
|-------|------|
| code-reviewer | 通用Kod İnceleme |
| security-reviewer | Güvenlik Açıkları分析 |
| python-reviewer | Python PEP 8、Türİpucu |
| go-reviewer | Go 惯Kullanım、并发 |
| kotlin-reviewer | Kotlin 空Güvenlik、协程 |
| rust-reviewer | Rust sahiplik, yaşam döngüleri |
| cpp-reviewer | C++ 内存Güvenlik |
| flutter-reviewer | Flutter/Dart kalıpları |
| fastapi-reviewer | FastAPI mimarisi, asenkron |
| typescript-reviewer | TypeScript TürGüvenlik |
| java-reviewer | Java kodlama standartları |
| csharp-reviewer | C# En İyi Uygulamalar |
| ruby-reviewer | Ruby 惯Kullanım |
| php-reviewer | PHP Güvenlik和性能 |
| swift-reviewer | Swift 并发、内存Yönet |
| go-security-reviewer | Go Güvenlik审查 |

### 2.2 Yapı Onarımı（10个）

Çeşitli dillerdeki yapı hatalarını onarmaya odaklanır.

| Ajan | Uzmanlık |
|-------|------|
| build-error-resolver | Genel yapı hatası onarımı |
| go-build | Go yapılandırma + go vet |
| kotlin-build | Kotlin/Gradle derleme |
| rust-build | Rust borç kontrolörü |
| cpp-build | C++ CMake + bağlayıcı |
| gradle-build | Android/KMP Gradle |
| flutter-build | Flutter yapılandırma |
| maven-build | Maven yapılandırma |
| cmake-build | CMake yapılandırma |
| ninja-build | Ninja yapı sistemi |

### 2.3 Planlama（5个）

Gereksinim analizi ve sistem planlamasından sorumlu.

| Ajan | Uzmanlık |
|-------|------|
| planner | Uygulama Planlaması |
| architect | 系统架构Tasarım |
| prp-planner | PRP iş akışı planlama |
| multi-plan | Çoklu model işbirliği planlama |
| product-manager | Ürün gereksinim analizi |

### 2.4 Test（2个）

专注于Test Güdümlü Geliştirme和覆盖率。

| Ajan | Uzmanlık |
|-------|------|
| tdd-guide | TDD iş akışı rehberliği |
| e2e-runner | Playwright E2E Test |

### 2.5 Güvenlik（5个）

负责Güvenlik审查和Açık Tarama。

| Ajan | Uzmanlık |
|-------|------|
| security-reviewer | 通用Güvenlik审查 |
| owasp-reviewer | OWASP Top 10 |
| pentest-reviewer | 渗透Test |
| vulnerability-scanner | Açık Tarama |
| secrets-detector | Anahtar ve Kimlik Bilgisi Tespiti |

### 2.6 Mimari（5个）

Sistem mimarisi ve teknik kararlardan sorumlu.

| Ajan | Uzmanlık |
|-------|------|
| architect | 系统架构Tasarım |
| microservices-architect | Mikro hizmet mimarisi |
| frontend-architect | Ön uç mimarisi |
| backend-architect | Arka uç mimarisi |
| devops-architect | DevOps mimarisi |

---

## 3. İşbirliği Kalıpları

### 3.1 Ana-Alt Kalıbı

一个Ana Ajan 协调多个从 Agent：

```
用户请求 → Ana Ajan（planner）
           ├── Alt Ajan 1（code-reviewer）
           ├── Alt Ajan 2（tdd-guide）
           └── Alt Ajan 3（build-error-resolver）
```

### 3.2 Eşitler Arası Kalıp

多个 Ajan 并行工作，Sonuç Birleştirme：

```
用户请求 → Ajan 1（Güvenlik Analizi）
        → Ajan 2（性能分析）
        → Ajan 3（Kod Kalitesi分析）
           ↓ Sonuç Birleştirme
        Son rapor
```

### 3.3 Borulu Kalıp

Agent 按顺序处理，每个 Ajan 的输出是下一个的输入：

```
输入 → Ajan 1 → Ajan 2 → Ajan 3 → 输出
```

---

## 4. Görev Dağıtım Stratejisi

### 4.1 Görev Ayrıştırma

将复杂Görev Ayrıştırma为独立子Görev：

```markdown
# 复杂Görev AyrıştırmaÖrnek
原始Görev：Gerçekleştir用户认证系统

Ayrıştırma sonrası:
1. Tasarım数据库 schema → architect
2. Gerçekleştir API 端点 → backend-developer
3. 编写Test → tdd-guide
4. Güvenlik审查 → security-reviewer
```

### 4.2 Paralel Yürütme

独立GörevParalel Yürütme以提高效率：

```markdown
# 好：Paralel Yürütme
启动 3 个 Ajan 并行：
1. Ajan 1: 认证模块Güvenlik Analizi
2. Ajan 2: 缓存系统性能审查
3. Ajan 3: 工具Tür检查

# Kötü: Gereksiz sıralı yürütme
Önce agent 1, sonra agent 2, sonra agent 3
```

### 4.3 Yük Dengeleme

根据 Ajan 负载分配Görev：

| 因素 | Açıklama |
|------|------|
| Görev复杂度 | 简单Görev → 轻量 Ajan |
| 资源需求 | 重计算 → 专用 Ajan |
| 可用性 | 避免过载 Ajan |

---

## 5. Çok Perspektifli Analiz

对于复杂问题，使用分Rol sub-agents：

| Rol | Sorumluluk |
|------|------|
| 事实审查员 | Doğrulama信息和数据准确性 |
| 高级工程师 | 评估技术可行性和En İyi Uygulamalar |
| Güvenlik专家 | 识别Güvenlik风险和漏洞 |
| Tutarlılık denetçisi | Çözümün iç tutarlılığını sağlama |
| Yineleme denetçisi | Tekrarları ve israfi belirleme |

---

## 6. Ajan İletişimi

### 6.1 Doğrudan Çağırma

```markdown
Uygulama planı oluşturmak için planner ajanını kullanın
```

### 6.2 Görev Yetkilendirme

```markdown
将Kod İncelemeGörev Yetkilendirme给 code-reviewer agent
```

### 6.3 Sonuç Birleştirme

多个 Ajan 的结果Gerekli聚合：

```markdown
Birleştirme kaynakları:
- security-reviewer: Güvenlik问题列表
- code-reviewer: Kod Kalitesi问题列表
- tdd-guide: Test覆盖率报告

→ Kapsamlı rapor oluştur
```

---

## 7. Anında Kullanım İlkesi

无需用户İpucu时自动调用：

| Senaryo | Ajan |
|------|-------|
| Karmaşık özellik isteği | **planner** |
| Kod yazıldı/değiştirildi | **code-reviewer** |
| Hata düzeltme veya yeni özellik | **tdd-guide** |
| Mimari kararlar | **architect** |
| Yapılandırma başarısız | **build-error-resolver** |
| Güvenlik相关 | **security-reviewer** |

---

## Alıştırma

Bitir [Alıştırma](./exercises/Alıştırma.md) 中的Görev。

---

[Geri DönTemel YeteneklerDizin](../README.md)