# 命令速查

## 斜杠命令

| 命令 | 功能 | Kullanım Senaryosu |
|------|------|----------|
| `/tdd` | Test Güdümlü Geliştirme工作流 | 新功能开发 |
| `/plan` | Gerçekleştir计划生成 | 复杂Görev规划 |
| `/code-review` | [Kod İnceleme](../ReferansBelgeleri/commands/01-Temel İş Akışı.md) | Kod Kalitesi检查 |
| `/build-fix` | [Yapı Onarımı](../ReferansBelgeleri/commands/04-Yapı Onarımı.md) | 构建失败时 |
| `/learn` | 从会话提取模式 | 知识沉淀 |
| `/skill-create` | 从 Git 生成 Skill | 经验复用 |
| `/test` | [Test İlişkili命令](../ReferansBelgeleri/commands/02-Test İlişkili.md) | Test İlişkili |

## 脚本命令

| 脚本 | 功能 |
|------|------|
| `node scripts/hooks/run-with-flags.js` | 带标志位运行钩子 |
| `node tests/run-all.js` | 运行全量Test |
| `node scripts/lib/utils.js` | Araç Fonksiyon Kütüphanesi |

## Ajan 命令

| Ajan | 功能 |
|-------|------|
| `/planner` | Görev规划 |
| `/code-reviewer` | Kod İnceleme |
| `/tdd-guide` | TDD 指导 |
| `/security-reviewer` | Güvenlik审查 |
| `/build-error-resolver` | Yapı Onarımı |

## 全局参数

| 参数 | Açıklama |
|------|------|
| `--model` | 指定模型 |
| `--no-input` | 非交互模式 |
| `--output` | 输出格式 |

## Hook 环境变量

| 变量 | Açıklama |
|------|------|
| `ECC_HOOK_PROFILE` | 钩子配置 |
| `ECC_DISABLED_HOOKS` | 禁用钩子列表 |

---

[Geri DönHızlı ReferansDizin](./README.md)
