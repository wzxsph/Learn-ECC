# 02 Ajans İşbirliği

> 理解 6 类 Ajan 的Sorumluluk，组合使用Bitir复杂开发Görev

## 模块Hedef

Bitir本模块后，你将Yapabilir：

- 识别 Claude Code 的 6 类 Ajan 及各自Sorumluluk
- 根据GörevTürUygun Ajanı Seç
- 组合多个 Ajan Bitir复杂Görev
- 理解 Ajans İşbirliği模式和En İyi Uygulamalar

## Ajan 分类体系

Claude Code 的 Ajan Bölünmüş 6 大类：

| Ajan Tür | 核心Sorumluluk | Uygulama Senaryosu |
|------------|----------|----------|
| **审查类** | Kod Kalitesi、模式规范、Güvenlik Açıkları | Kod İnceleme、Teknik Borç Temizleme |
| **构建类** | 编译构建、错误修复、依赖Yönet | 构建失败、编译错误 |
| **Planlama** | Görev Ayrıştırma、架构Tasarım、技术方案 | 新功能、架构变更 |
| **Test** | Test编写、覆盖率分析、Test策略 | TDD、Test补充 |
| **Güvenlik** | Güvenlik审计、Açık Tarama、Uyumluluk Kontrolü | Güvenlik敏感代码、Ödeme Modülü |
| **Mimari** | Sistem Tasarımı、Kalıp Seçimi、Teknoloji Seçimi | 大型重构、架构演进 |

## 6 类 Ajan 详解

### 1. 审查类 Agent

**Sorumluluk**: Kod Kalitesi审查、模式规范检查

可用 Agent：
- `code-reviewer` - 通用Kod İnceleme
- `python-reviewer` - Python 专项审查
- `go-reviewer` - Go Dil İnceleme
- `rust-reviewer` - Rust Güvenlik审查
- `security-reviewer` - Güvenlik Açıkları审查

**Kullanım Senaryosu**:
- 提交代码前进行质量检查
- 发现潜在的Kod Kokusu
- 审查第三方代码

### 2. 构建类 Agent

**Sorumluluk**: 编译构建、错误修复、依赖Yönet

可用 Agent：
- `build-error-resolver` - 构建错误解析
- `go-build` - Go 构建
- `rust-build` - Rust 构建
- `kotlin-build` - Kotlin 构建
- `flutter-build` - Flutter 构建

**Kullanım Senaryosu**:
- 构建失败时的错误修复
- Bağımlılık Çakışması Çözümü
- Çapraz Platform Yapı Sorunları

### 3. Planlama Agent

**Sorumluluk**: Görev Ayrıştırma、架构Tasarım、实施计划

可用 Agent：
- `planner` - 实施计划生成
- `architect` - 架构Tasarım
- `tdd-guide` - TDD 工作流指导

**Kullanım Senaryosu**:
- Yeni Özellik Geliştirme Planlaması
- Teknik Şema Değerlendirmesi
- 复杂Görev Ayrıştırma

### 4. Test Agent

**Sorumluluk**: Test编写、覆盖率提升、Test策略

可用 Agent：
- `tdd-guide` - Test Güdümlü Geliştirme
- `e2e-runner` - Uçtan Uca Test
- `test-coverage` - 覆盖率分析

**Kullanım Senaryosu**:
- 编写新功能Test
- 提升Test覆盖率
- E2E TestSenaryoTasarım

### 5. Güvenlik Agent

**Sorumluluk**: Güvenlik审计、Açık Tarama、Uyumluluk Kontrolü

可用 Agent：
- `security-reviewer` - Güvenlik Açıkları审查
- `security-scan` - 自动化Güvenlik扫描

**Kullanım Senaryosu**:
- 支付和认证Kod İnceleme
- 外部数据处理检查
- 合规性Doğrulama

### 6. Mimari Agent

**Sorumluluk**: Sistem Tasarımı、Kalıp Seçimi、Teknoloji Seçimi

可用 Agent：
- `architect` - 架构Tasarım
- `refactor-cleaner` - 重构指导
- `api-design-reviewer` - API Tasarım审查

**Kullanım Senaryosu**:
- Mikro Hizmet Ayrıştırma
- 数据库Tasarım
- API Mimari Evrimi

## Ajans İşbirliği模式

### 模式 1: 串行协作

```
Görev → Planner → 实施 → Reviewer → Test → Bitir
```

适Kullanılan：Doğrusal Akış，Adımlar之间有依赖

### 模式 2: 并行协作

```
            → Reviewer A ─→
Görev → 分解 ─→ Reviewer B ─→ 汇总 → Bitir
            → Reviewer C ─→
```

适Kullanılan：Çok Perspektifli İnceleme、Bağımsız Modül İnceleme

### 模式 3: 级联协作

```
重大Görev → Architect → Planner → 实施 → 各级 Reviewer → 合并
```

适Kullanılan：Karmaşık Proje，Büyük Özellik Geliştirme

## Ajan 组合Örnek

### Senaryo: 开发新功能

```
1. 使用 /plan 规划功能
2. 使用 tdd-guide 进行 TDD 开发
3. 使用 code-reviewer Kodu İncele
4. 使用 security-reviewer 检查Güvenlik
5. 使用 verify Doğrulama功能
```

### Senaryo: 修复Güvenlik Açıkları

```
1. 使用 security-reviewer 扫描漏洞
2. 使用 planner Onarım Planı Oluştur
3. 使用 build-fix 实施修复
4. 使用 security-reviewer 重新Doğrulama
5. 使用 verify 确认修复
```

## Ajan 选择指南

| GörevTür | 推荐 Ajan | Gerekçe |
|----------|------------|------|
| 新功能开发 | planner + tdd-guide | Gerekli规划和 TDD |
| Kod İnceleme | code-reviewer + security-reviewer | 质量和Güvenlik双重检查 |
| 构建失败 | build-error-resolver | 专注错误修复 |
| 架构重构 | architect + refactor-cleaner | 架构层面指导 |
| Güvenlik敏感 | security-reviewer (优先) | Zorunlu先过Güvenlik关 |
| Test编写 | tdd-guide + test-coverage | Test策略和覆盖率 |

## Destekleyici Materyaller

Tam Ajan Belgesi Konumu：`../../ReferansBelgeleri/agents/1. Kod İnceleme.md`

## Sonraki Adım

- 学习[Alıştırma题目](./exercises/Alıştırma.md)
- 阅读 [Agent 完整文档](../../ReferansBelgeleri/agents/1. Kod İnceleme.md)