# Sistema de Documentação ECC

ECC (Everything Claude Code) é um plugin do Claude Code que fornece 75 comandos, 60 Agents, 16 Skills, 7 documentos de regras e um sistema completo de configuração Hook/MCP.

---

## 1. Visão Geral da Documentação ECC

O ECC fornece suporte de fluxo de trabalho de desenvolvimento de nível de produção para o Claude Code, cobrindo múltiplos frameworks de AI Agent (Claude Code, Codex, OpenCode, Cursor, Gemini).

### Pilha de Tecnologias

| Camada | Tecnologia | Versão |
|------|------------|---------|
| Linguagem Primária | JavaScript/Node.js | >=18 |
| Linguagem Secundária | Python | >=3.11 |
| Gerenciador de Pacotes | Yarn | 4.9.2 |
| Executor de Testes | Executor de testes Node.js personalizado | - |
| Cobertura | c8 | 11.x |
| Verificação de Código | ESLint + markdownlint-cli | - |

### Estrutura de Diretórios

```
ECC/
├── agents/        # 60 Agents profissionais
├── commands/      # 75 arquivos de comando
├── skills/        # 16 diretórios de Skills
├── rules/         # 7 documentos de regras (common + específico por linguagem)
├── hooks/         # 3 documentos de Hook
├── mcp/           # 6 configurações de servidor MCP
├── scripts/       # 54 scripts de utilitário
├── README.md
```

---

## 2. Navegação Rápida

| Categoria | Documentação | Descrição |
|----------|---------------|--------------|
| [Visão Geral de Comandos](./commands/) | [commands/](commands/) | Lista completa de 75 comandos |
| [Índice de Agents](./agents/) | [agents/](agents/) | 60 Agents profissionais |
| [Índice de Skills](./skills/) | [skills/](skills/) | 16 Skills por domínio |
| [Índice de Regras](./rules/) | [rules/](rules/) | 7 documentos de regras (common + específico por linguagem) |
| [Índice de Hooks](./hooks/) | [hooks/](hooks/) | Configuração e desenvolvimento do sistema Hook |
| [Índice de MCP](./mcp/) | [mcp/](mcp/) | Configuração e desenvolvimento de servidor MCP |
| [Índice de Scripts](./scripts/) | [scripts/](scripts/) | 54 scripts de utilitário |

---

## 3. Índice de Comandos

Total de **75 comandos**, agrupados por categoria:

### 3.1 Fluxos de Trabalho Principais (6)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/plan` | [01-核心工作流.md](commands/01-核心工作流.md) | Análise de requisitos + avaliação de risco + plano passo a passo |
| `/code-review` | [01-核心工作流.md](commands/01-核心工作流.md) | Revisão de qualidade / segurança / manutenibilidade do código |
| `/build-fix` | [01-核心工作流.md](commands/01-核心工作流.md) | Detecção automática de linguagem + correção incremental de erros de build |
| `/verify` | [01-核心工作流.md](commands/01-核心工作流.md) | Loop de verificação completo: build → lint → teste → verificação de tipo |
| `/quality-gate` | [01-核心工作流.md](commands/01-核心工作流.md) | Executar pipeline de qualidade ECC sob demanda |
| `/tdd` | [01-核心工作流.md](commands/01-核心工作流.md) | Fluxo de trabalho TDD geral |

### 3.2 Testes Relacionados (8)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/go-test` | [02-测试相关.md](commands/02-测试相关.md) | TDD Go (orientado por tabela, 80%+ cobertura) |
| `/kotlin-test` | [02-测试相关.md](commands/02-测试相关.md) | TDD Kotlin (Kotest + Kover) |
| `/rust-test` | [02-测试相关.md](commands/02-测试相关.md) | TDD Rust (cargo test + testes de integração) |
| `/cpp-test` | [02-测试相关.md](commands/02-测试相关.md) | TDD C++ (GoogleTest + gcov/lcov) |
| `/flutter-test` | [02-测试相关.md](commands/02-测试相关.md) | TDD Flutter |
| `/e2e` | [02-测试相关.md](commands/02-测试相关.md) | Gerar + executar testes e2e Playwright |
| `/test-coverage` | [02-测试相关.md](commands/02-测试相关.md) | Relatório de cobertura de teste + análise de lacunas |
| `/python-testing` | [02-测试相关.md](commands/02-测试相关.md) | Melhores práticas de teste Python |

### 3.3 Revisão de Linguagem (7)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/python-review` | [03-语言审查.md](commands/03-语言审查.md) | Python PEP 8, dicas de tipo, segurança |
| `/go-review` | [03-语言审查.md](commands/03-语言审查.md) | Idiomas Go, concorrência, tratamento de erros |
| `/kotlin-review` | [03-语言审查.md](commands/03-语言审查.md) | Segurança nula Kotlin, corrotinas, arquitetura |
| `/rust-review` | [03-语言审查.md](commands/03-语言审查.md) | Propriedade Rust, tempos de vida, unsafe |
| `/cpp-review` | [03-语言审查.md](commands/03-语言审查.md) | Segurança de memória C++, idiomas modernos |
| `/flutter-review` | [03-语言审查.md](commands/03-语言审查.md) | Padrões Flutter/Dart |
| `/fastapi-review` | [03-语言审查.md](commands/03-语言审查.md) | Arquitetura FastAPI, async, Pydantic |

### 3.4 Build e Correção (6)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/go-build` | [04-构建修复.md](commands/04-构建修复.md) | Corrigir erros de build Go + avisos go vet |
| `/kotlin-build` | [04-构建修复.md](commands/04-构建修复.md) | Corrigir erros do compilador Kotlin/Gradle |
| `/rust-build` | [04-构建修复.md](commands/04-构建修复.md) | Corrigir build Rust + problemas do verificador de empréstimo |
| `/cpp-build` | [04-构建修复.md](commands/04-构建修复.md) | Corrigir problemas de CMake + linker C++ |
| `/gradle-build` | [04-构建修复.md](commands/04-构建修复.md) | Corrigir erros Android/KMP Gradle |
| `/flutter-build` | [04-构建修复.md](commands/04-构建修复.md) | Correções de build Flutter |

### 3.5 Planejamento e Arquitetura (7)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/plan-prd` | [05-规划与架构.md](commands/05-规划与架构.md) | Gerador de PRD interativo |
| `/prp-plan` | [05-规划与架构.md](commands/05-规划与架构.md) | Planejamento abrangente de features |
| `/prp-prd` | [05-规划与架构.md](commands/05-规划与架构.md) | Gerador de PRD de fluxo de trabalho PRP |
| `/prp-implement` | [05-规划与架构.md](commands/05-规划与架构.md) | Executar plano PRP + loop de verificação |
| `/prp-pr` | [05-规划与架构.md](commands/05-规划与架构.md) | Criar PR do fluxo de trabalho PRP |
| `/prp-commit` | [05-规划与架构.md](commands/05-规划与架构.md) | Commit verificado PRP |
| `/multi-plan` | [05-规划与架构.md](commands/05-规划与架构.md) | Planejamento colaborativo multi-modelo (Codex + Gemini) |

### 3.6 Gerenciamento de Sessão (5)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/save-session` | [06-会话管理.md](commands/06-会话管理.md) | Salvar estado da sessão |
| `/resume-session` | [06-会话管理.md](commands/06-会话管理.md) | Carregar e restaurar sessão salva |
| `/sessions` | [06-会话管理.md](commands/06-会话管理.md) | Navegar + buscar + gerenciar histórico de sessões |
| `/checkpoint` | [06-会话管理.md](commands/06-会话管理.md) | Criar / verificar checkpoint de workflow |
| `/aside` | [06-会话管理.md](commands/06-会话管理.md) | Responder perguntas secundárias sem perder contexto |

### 3.7 Aprendizado e Melhoria (10)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/learn` | [07-学习与改进.md](commands/07-学习与改进.md) | Extrair padrões reutilizáveis da sessão |
| `/learn-eval` | [07-学习与改进.md](commands/07-学习与改进.md) | Extrair padrões + auto-avaliação de qualidade |
| `/evolve` | [07-学习与改进.md](commands/07-学习与改进.md) | Analisar instinct + sugerir estrutura de evolução |
| `/promote` | [07-学习与改进.md](commands/07-学习与改进.md) | Promover instinct de projeto para escopo global |
| `/instinct-status` | [07-学习与改进.md](commands/07-学习与改进.md) | Mostrar todos os instincts aprendidos |
| `/instinct-export` | [07-学习与改进.md](commands/07-学习与改进.md) | Exportar instinct para arquivo |
| `/instinct-import` | [07-学习与改进.md](commands/07-学习与改进.md) | Importar instinct de arquivo / URL |
| `/skill-create` | [07-学习与改进.md](commands/07-学习与改进.md) | Analisar histórico git + gerar arquivo skill |
| `/skill-health` | [07-学习与改进.md](commands/07-学习与改进.md) | Painel de saúde de composição de Skills |
| `/rules-distill` | [07-学习与改进.md](commands/07-学习与改进.md) | Escanear skills + extrair princípios entre domínios |

### 3.8 Loops e Automação (3)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/loop-start` | [08-循环与自动化.md](commands/08-循环与自动化.md) | Iniciar modo de loop autônomo gerenciado |
| `/loop-status` | [08-循环与自动化.md](commands/08-循环与自动化.md) | Verificar status do loop em execução |
| `/santa-loop` | [08-循环与自动化.md](commands/08-循环与自动化.md) | Loop autônomo estilo Santa |

### 3.9 Gerenciamento de Projetos (6)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/projects` | [09-项目管理.md](commands/09-项目管理.md) | Listar projetos conhecidos + estatísticas de instinct |
| `/harness-audit` | [09-项目管理.md](commands/09-项目管理.md) | Auditar configuração do agent harness |
| `/model-route` | [09-项目管理.md](commands/09-项目管理.md) | Rotear tarefas para modelo apropriado |
| `/pm2` | [09-项目管理.md](commands/09-项目管理.md) | Inicialização do gerenciador de processos PM2 |
| `/setup-pm` | [09-项目管理.md](commands/09-项目管理.md) | Configurar gerenciador de pacotes |
| `/project-init` | [09-项目管理.md](commands/09-项目管理.md) | Inicialização de projeto |

### 3.10 Fluxo de Trabalho PR (6)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/pr` | [10-PR工作流.md](commands/10-PR工作流.md) | Criar PR do GitHub do branch atual |
| `/review-pr` | [10-PR工作流.md](commands/10-PR工作流.md) | Revisar PR do GitHub |
| `/multi-workflow` | [10-PR工作流.md](commands/10-PR工作流.md) | Desenvolvimento colaborativo multi-modelo |
| `/multi-backend` | [10-PR工作流.md](commands/10-PR工作流.md) | Desenvolvimento backend multi-modelo |
| `/multi-frontend` | [10-PR工作流.md](commands/10-PR工作流.md) | Desenvolvimento frontend multi-modelo |
| `/multi-execute` | [10-PR工作流.md](commands/10-PR工作流.md) | Execução colaborativa multi-modelo |

### 3.11 Sistema Hookify (4)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/hookify` | [11-Hookify系统.md](commands/11-Hookify系统.md) | Criar hooks para prevenir maus comportamentos |
| `/hookify-list` | [11-Hookify系统.md](commands/11-Hookify系统.md) | Listar todas as regras hookify configuradas |
| `/hookify-configure` | [11-Hookify系统.md](commands/11-Hookify系统.md) | Ativar / desativar regras hookify interativamente |
| `/hookify-help` | [11-Hookify系统.md](commands/11-Hookify系统.md) | Ajuda do sistema Hookify |

### 3.12 Documentação e Pesquisa (3)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/update-docs` | [12-文档与研究.md](commands/12-文档与研究.md) | Atualizar documentação do projeto |
| `/update-codemaps` | [12-文档与研究.md](commands/12-文档与研究.md) | Regenerar codemaps |
| `/ecc-guide` | [12-文档与研究.md](commands/12-文档与研究.md) | Guia do usuário ECC |

### 3.13 Refatoração e Limpeza (2)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/refactor-clean` | [13-重构与清理.md](commands/13-重构与清理.md) | Excluir código morto + mesclar duplicatas |
| `/auto-update` | [13-重构与清理.md](commands/13-重构与清理.md) | Capacidades de auto-atualização |

### 3.14 Outros Comandos (7)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/jira` | [14-其他命令.md](commands/14-其他命令.md) | Interação com ticket Jira |
| `/gan-build` | [14-其他命令.md](commands/14-其他命令.md) | Operações de build GAN |
| `/gan-design` | [14-其他命令.md](commands/14-其他命令.md) | Operações de design GAN |
| `/prune` | [14-其他命令.md](commands/14-其他命令.md) | Excluir instincts obsoletos (>30 dias) |
| `/security-scan` | [14-其他命令.md](commands/14-其他命令.md) | Varredura de segurança |
| `/feature-dev` | [14-其他命令.md](commands/14-其他命令.md) | Assistente de desenvolvimento de features |
| `/cost-report` | [14-其他命令.md](commands/14-其他命令.md) | Relatório de custos do modelo |

---

## 4. Índice de Agents

Total de **60 Agents**, agrupados por categoria:

| Categoria de Agent | Arquivo | Descrição |
|----------------|------|-------------|
| [Revisão de Código](agents/1.%20代码审查类.md) | [1. 代码审查类.md](agents/1.%20代码审查类.md) | 14 Agents de revisão |
| [Build e Correção](agents/2.%20构建修复类.md) | [2. 构建修复类.md](agents/2.%20构建修复类.md) | 14 Agents de correção de build |
| [Planejamento](agents/3.%20规划类.md) | [3. 规划类.md](agents/3.%20规划类.md) | 5 Agents de planejamento |
| [Testes](agents/4.%20测试类.md) | [4. 测试类.md](agents/4.%20测试类.md) | 2 Agents de teste |
| [Segurança](agents/5.%20安全类.md) | [5. 安全类.md](agents/5.%20安全类.md) | 3 Agents de segurança |
| [Arquitetura](agents/6.%20架构类.md) | [6. 架构类.md](agents/6.%20架构类.md) | 3 Agents de arquitetura |

---

## 5. Índice de Skills

**16 domínios de Skills**, categorizados por domínio:

| Domínio | Arquivo | Descrição |
|--------|------|-------------|
| [Melhores Práticas](skills/最佳实践.md) | [最佳实践.md](skills/最佳实践.md) | Padrões de codificação, tratamento de erros, loops autônomos |
| [Linguagens de Programação](skills/编程语言.md) | [编程语言.md](skills/编程语言.md) | Python/Go/Rust/Kotlin/C++ etc. |
| [Frameworks](skills/框架.md) | [框架.md](skills/框架.md) | Django/Laravel/NestJS/Spring Boot etc. |
| [Testes](skills/测试.md) | [测试.md](skills/测试.md) | TDD/Teste Unitário/Teste de Integração/E2E |
| [Segurança](skills/安全.md) | [安全.md](skills/安全.md) | Revisão de segurança, scanning de vulnerabilidades |
| [Frontend e Design](skills/前端与设计.md) | [前端与设计.md](skills/前端与设计.md) | Desenvolvimento frontend, sistemas de design |
| [Backend e API](skills/后端与API.md) | [后端与API.md](skills/后端与API.md) | Serviços backend, design de API, bancos de dados |
| [Deployment e DevOps](skills/部署与DevOps.md) | [部署与DevOps.md](skills/部署与DevOps.md) | Docker/K8s/estratégias de deploy |
| [Monitoramento e Observabilidade](skills/监控与可观测性.md) | [监控与可观测性.md](skills/监控与可观测性.md) | Observabilidade, diagnóstico de rede |
| [Automação e Scripting](skills/自动化与脚本.md) | [自动化与脚本.md](skills/自动化与脚本.md) | Loops autônomos, aprendizado contínuo, engenharia de agentes |
| [Busca e Recuperação de Dados](skills/搜索与数据获取.md) | [搜索与数据获取.md](skills/搜索与数据获取.md) | Busca Exa, web scraping, MCP |
| [GitHub e Colaboração](skills/GitHub与协作.md) | [GitHub与协作.md](skills/GitHub与协作.md) | Fluxos de trabalho GitHub, revisão de código |
| [AI e Machine Learning](skills/AI与机器学习.md) | [AI与机器学习.md](skills/AI与机器学习.md) | Redes neurais, PyTorch, MLOps |
| [Cloud Native e Infraestrutura](skills/云原生与基础设施.md) | [云原生与基础设施.md](skills/云原生与基础设施.md) | Kubernetes, Docker, Terraform |
| [Skills Especializados](skills/特殊领域技能.md) | [特殊领域技能.md](skills/特殊领域技能.md) | Blockchain, desenvolvimento de jogos, áudio/vídeo, IoT |
| [Toolchain de Desenvolvimento](skills/开发工具链.md) | [开发工具链.md](skills/开发工具链.md) | Frameworks de teste, CI/CD, qualidade de código |
| [Tecnologia de Ponta](skills/前沿技术.md) | [前沿技术.md](skills/前沿技术.md) | AI Agent, computação quântica, edge computing |

---

## 6. Índice de Regras

Total de **7 documentos de regras** (common + específico por linguagem):

| Regras | Arquivo | Descrição |
|-------|------|-------------|
| [Fluxo de Trabalho Git](rules/Git工作流.md) | [Git工作流.md](rules/Git工作流.md) | Padrões de commit Git e fluxo de trabalho PR |
| [Sistema de Hooks](rules/Hooks系统.md) | [Hooks系统.md](rules/Hooks系统.md) | Guia de configuração e uso de hooks |
| [Orquestração de Agents](rules/代理编排.md) | [代理编排.md](rules/代理编排.md) | Padrões de orquestração de agents |
| [Otimização de Performance](rules/性能优化.md) | [性能优化.md](rules/性能优化.md) | Guia de otimização de performance |
| [Estilo de Código](rules/代码风格.md) | [代码风格.md](rules/代码风格.md) | Padrões de estilo de codificação |
| [Regras de Teste](rules/测试规则.md) | [测试规则.md](rules/测试规则.md) | Requisitos de teste (80% cobertura) |
| [Regras de Segurança](rules/安全规则.md) | [安全规则.md](rules/安全规则.md) | Lista de verificação de segurança |

---

## 7. Índice de Hooks

Total de **4 documentos**:

| Documento | Arquivo | Descrição |
|----------|------|-------------|
| [Tipos de Hook](hooks/Hook类型.md) | [Hook类型.md](hooks/Hook类型.md) | Tipos PreToolUse, PostToolUse, Stop |
| [Hooks Embutidos](hooks/内置Hooks.md) | [内置Hooks.md](hooks/内置Hooks.md) | Lista de hooks embutidos e uso |
| [Desenvolvimento Personalizado](hooks/自定义开发.md) | [自定义开发.md](hooks/自定义开发.md) | Guia de desenvolvimento de hooks personalizados |
| [Formato de Configuração](hooks/配置格式.md) | [配置格式.md](hooks/配置格式.md) | Formato de configuração hooks.json |

---

## 8. Índice de MCP

Total de **6 configurações de servidor MCP**:

| Documento | Arquivo | Descrição |
|----------|------|-------------|
| [Formato de Configuração MCP](mcp/MCP配置格式.md) | [MCP配置格式.md](mcp/MCP配置格式.md) | Formato de arquivo de configuração MCP |
| [Servidores Embutidos](mcp/内置服务器.md) | [内置服务器.md](mcp/内置服务器.md) | Servidores MCP embutidos |
| [Desenvolvimento Personalizado](mcp/自定义开发.md) | [自定义开发.md](mcp/自定义开发.md) | Desenvolvimento de servidor MCP personalizado |

---

## 9. Índice de Scripts

Total de **54 scripts de utilitário**:

| Documento | Arquivo | Descrição |
|----------|------|-------------|
| [Scripts de Utilitário](scripts/工具脚本.md) | [工具脚本.md](scripts/工具脚本.md) | ecc.js, install-apply.js etc. |
| [Biblioteca de Funções de Utilitário](scripts/工具函数库.md) | [工具函数库.md](scripts/工具函数库.md) | Biblioteca de funções compartilhadas |
| [Executor de Testes](scripts/测试运行器.md) | [测试运行器.md](scripts/测试运行器.md) | Uso do executor de testes |
| [Scripts de Build](scripts/构建脚本.md) | [构建脚本.md](scripts/构建脚本.md) | Scripts de build |

---

## 10. Guia de Contribuição

### Convenções de Nome de Arquivo

- **Comandos, Skills, Agents, Hooks**: Minúsculas + hífen (ex.: `code-review.md`)
- **Scripts**: camelCase ou kebab-case (ex.: `session-start.js`)
- **Regras**: Organizado por linguagem / tópico

### Processo de Submissão

1. Criar novo arquivo ou modificar arquivo existente
2. Garantir seguir convenções de nomenclatura
3. Executar testes: `node tests/run-all.js`
4. Executar markdown lint: `npx markdownlint-cli '**/*.md' --ignore node_modules`
5. Criar PR para revisão

---

*Sistema de Documentação ECC - Fluxos de trabalho de desenvolvimento de nível de produção para Claude Code*