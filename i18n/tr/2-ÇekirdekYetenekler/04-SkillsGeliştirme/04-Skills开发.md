# 04 - Skills Geliştirme

Nasıl创建可复用的 Skills 工作流。

## Genel Bakış

ECC Sağla **234+ [Skills](../../ReferansBelgeleri/skills/En İyi Uygulamalar.md)**，按领域分类，涵盖En İyi Uygulamalar、Programlama Dilleri、Çerçeveler、Test、Güvenlik、Frontend ve Tasarım、后端与 API、部署与 DevOps 等 16 个领域。

---

## 1. Skill 格式

### 1.1 基本结构

Skills 使用 Markdown 格式，包含清晰的章节结构：

```markdown
# Skill Ad

## When to Use
## How It Works
## Examples
```

### 1.2 YAML Frontmatter

```yaml
---
name: skill-name
description: Skill KullanımTanım
category: 分类
---
```

### 1.3 章节结构

| 章节 | Açıklama |
|------|------|
| **When to Use** | 何时使用此 Skill |
| **How It Works** | 工作原理和Adımlar |
| **Examples** | 使用Örnek |

---

## 2. Skills 索引

### 2.1 按领域分类

| 领域 | Açıklama |
|------|------|
| [En İyi Uygulamalar](../../ReferansBelgeleri/skills/En İyi Uygulamalar.md) | 编码标准、Hata Yönetimi、自主循环 |
| [Programlama Dilleri](../../ReferansBelgeleri/skills/Programlama Dilleri.md) | Python/Go/Rust/Kotlin/C++ 等 |
| [Çerçeveler](../../ReferansBelgeleri/skills/Çerçeveler.md) | Django/Laravel/NestJS/Spring Boot 等 |
| [Test](../../ReferansBelgeleri/skills/Test.md) | TDD/单元Test/集成Test/E2E |
| [Güvenlik](../../ReferansBelgeleri/skills/Güvenlik.md) | Güvenlik审查、Açık Tarama |
| [Frontend ve Tasarım](../../ReferansBelgeleri/skills/Frontend ve Tasarım.md) | 前端开发、Tasarım系统 |
| [Backend ve API](../../ReferansBelgeleri/skills/Backend ve API.md) | 后端服务、API Tasarım、数据库 |
| [Dağıtım ve DevOps](../../ReferansBelgeleri/skills/Dağıtım ve DevOps.md) | Docker/K8s/部署策略 |
| [İzleme ve Gözlemlenebilirlik](../../ReferansBelgeleri/skills/İzleme ve Gözlemlenebilirlik.md) | 可观测性、网络诊断 |
| [Otomasyon ve Betik](../../ReferansBelgeleri/skills/Otomasyon ve Betik.md) | 自主循环、持续学习、代理工程 |
| [Arama ve Veri Alma](../../ReferansBelgeleri/skills/Arama ve Veri Alma.md) | Exa 搜索、数据抓取、MCP |
| [GitHub ve İşbirliği](../../ReferansBelgeleri/skills/GitHub ve İşbirliği.md) | GitHub 工作流、Kod İnceleme |
| [AI ve Makine Öğrenimi](../../ReferansBelgeleri/skills/AI ve Makine Öğrenimi.md) | 神经网络、PyTorch、MLOps |
| [Cloud Native ve Altyapı](../../ReferansBelgeleri/skills/Cloud Native ve Altyapı.md) | Kubernetes、Docker、Terraform |
| [Özel Alan Becerileri](../../ReferansBelgeleri/skills/Özel Alan Becerileri.md) | 区块链、游戏开发、音视频、IoT |
| [Geliştirme Araç Zinciri](../../ReferansBelgeleri/skills/Geliştirme Araç Zinciri.md) | TestÇerçeveler、CI/CD、Kod Kalitesi |
| [İleri Teknolojiler](../../ReferansBelgeleri/skills/İleri Teknolojiler.md) | AI Agent、量子计算、边缘计算 |

### 2.2 热门 Skills

| Skill | Kullanım |
|-------|------|
| TDD 工作流 | Test Güdümlü Geliştirme标准流程 |
| Kod İnceleme | Kod Kalitesi检查清单 |
| Güvenlik审查 | OWASP Top 10 检查 |
| API Tasarım | RESTful API Tasarım原则 |
| Docker 部署 | 容器化部署En İyi Uygulamalar |
| CI/CD 流水线 | 持续集成和部署 |

---

## 3. 创建自定义 Skill

### 3.1 创建Adımlar

1. **确定Kullanım**：明确 Skill 要解决的问题
2. **编写结构**：按照 When to Use / How It Works / Examples 结构
3. **添加 YAML frontmatter**：Sağla元数据
4. **保存到 skills/ Dizin**：使用小写 + 连字符命名

### 3.2 Örnek Skill

```markdown
---
name: my-custom-skill
description: 执行特定Görev的工作流
category: 自定义
---

# My Custom Skill

## When to Use

当Gerekli进行特定Görev X 时使用此 Skill。

## How It Works

1. Adımlar一：准备数据
2. Adımlar二：执行核心操作
3. Adımlar三：Doğrulama结果
4. Adımlar四：清理资源

## Examples

### Örnek 1：基本Kullanım

```
执行 My Custom Skill 来Bitir X
```

### Örnek 2：高级Kullanım

```
使用自定义参数执行 My Custom Skill
```
```

### 3.3 命名规范

| Tür | 规范 | Örnek |
|------|------|------|
| 文件名 | 小写 + 连字符 | `code-review.md`, `tdd-workflow.md` |
| Dizin | 按领域组织 | `skills/Programlama Dilleri/` |
| 位置 | 项目 skills/ 或全局 ~/.claude/skills/ | |

---

## 4. 工作流Tasarım

### 4.1 Adımlar定义

清晰定义每个Adımlar：

```markdown
## How It Works

1. **需求分析**：理解用户需求
2. **方案Tasarım**：制定实施方案
3. **代码Gerçekleştir**：编写代码
4. **TestDoğrulama**：确保质量
5. **Belge Güncelleme**：记录变更
```

### 4.2 条件分支

处理不同Senaryo：

```markdown
## How It Works

1. 分析输入
2. 根据条件分支：
   - 如果是 A，执行Adımlar X
   - 如果是 B，执行Adımlar Y
   - 否则，执行Adımlar Z
3. 汇总结果
```

### 4.3 Hata Yönetimi

标准化的Hata Yönetimi：

```markdown
## How It Works

1. 执行操作
2. 检查结果
   - 成功：继续Sonraki Adım
   - 失败：记录错误并Sağla修复建议
3. Geri Dön最终结果
```

---

## 5. 参数化Şablon

### 5.1 参数定义

```markdown
## How It Works

1. 接收输入参数：
   - `name`: Ad（必填）
   - `type`: Tür（可选，默认值：`standard`）
2. 处理参数
3. Geri Dön结果
```

### 5.2 变量替换

```markdown
## Examples

### 基本Kullanım

使用默认参数：
```
执行 Skill，Ad为 {{name}}
```
```

### 自定义参数

指定自定义Tür：
```
执行 Skill，Ad为 {{name}}，Tür为 {{type}}
```
```

---

## 6. 发布和分享

### 6.1 本地存储

| 位置 | Açıklama |
|------|------|
| 项目 `skills/` | 项目特定的 Skills |
| `~/.claude/skills/` | 全局可用的 Skills |

### 6.2 导入导出

```bash
# 导出 Skill 到文件
/skill-export my-skill

# 导入 Skill 从文件
/skill-import path/to/skill.md
```

### 6.3 Skill 市场

ECC Destekle从市场安装共享的 Skills。

---

## 7. Skill 创建工作流

使用 `/skill-create` 命令分析 git 历史并生成 Skill 文件：

```bash
# 分析 git 历史并生成 Skill
/skill-create

# 指定分析范围
/skill-create --days 30

# 指定输出路径
/skill-create --output ./my-skills/
```

---

## 8. En İyi Uygulamalar

### 8.1 Tasarım原则

| 原则 | Açıklama |
|------|------|
| **单一Sorumluluk** | 每个 Skill 只做一件事 |
| **可组合** | Skill 之间Olabilir组合使用 |
| **可复用** | Tasarım通用而非特定 |
| **可Test** | SağlaDoğrulamaAdımlar |

### 8.2 文档质量

| 要求 | Açıklama |
|------|------|
| 清晰的 When to Use | 让用户知道何时使用 |
| 详细的 How It Works | Adımlar清晰可执行 |
| 实用的 Examples | Sağla真实用例 |
| Hata YönetimiAçıklama | 告诉用户如何处理错误 |

### 8.3 维护建议

- 定期更新以反映最新实践
- 根据用户反馈改进
- 保持简洁，避免过度复杂
- 添加版本号便于追踪

---

## Alıştırma

Bitir [Alıştırma](./exercises/Alıştırma.md) 中的Görev。

---

[Geri DönTemel YeteneklerDizin](../README.md)