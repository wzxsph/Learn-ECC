# Sistema de Documentação ECC

ECC (Everything Claude Code) é um plugin do Claude Code que fornece 75 comandos, 60 Agents, 16 Skills, 7 documentos de regras e um sistema completo de configuração Hook/MCP.

---

## 1. Visão Geral do Sistema de Documentação ECC

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
| `/plan` | [01-Workflow-Principal.md](commands/01-Workflow-Principal.md) | Análise de requisitos + avaliação de risco + plano passo a passo |
| `/code-review` | [01-Workflow-Principal.md](commands/01-Workflow-Principal.md) | Revisão de qualidade / segurança / manutenibilidade do código |
| `/build-fix` | [01-Workflow-Principal.md](commands/01-Workflow-Principal.md) | Detecção automática de linguagem + correção incremental de erros de build |
| `/verify` | [01-Workflow-Principal.md](commands/01-Workflow-Principal.md) | Loop de verificação completo: build → lint → teste → verificação de tipo |
| `/quality-gate` | [01-Workflow-Principal.md](commands/01-Workflow-Principal.md) | Executar pipeline de qualidade ECC sob demanda |
| `/tdd` | [01-Workflow-Principal.md](commands/01-Workflow-Principal.md) | Fluxo de trabalho TDD geral |

### 3.2 Testes Relacionados (8)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/go-test` | [02-Testes.md](commands/02-Testes.md) | TDD Go (orientado por tabela, 80%+ cobertura) |
| `/kotlin-test` | [02-Testes.md](commands/02-Testes.md) | TDD Kotlin (Kotest + Kover) |
| `/rust-test` | [02-Testes.md](commands/02-Testes.md) | TDD Rust (cargo test + testes de integração) |
| `/cpp-test` | [02-Testes.md](commands/02-Testes.md) | TDD C++ (GoogleTest + gcov/lcov) |
| `/flutter-test` | [02-Testes.md](commands/02-Testes.md) | TDD Flutter |
| `/e2e` | [02-Testes.md](commands/02-Testes.md) | Gerar + executar testes e2e Playwright |
| `/test-coverage` | [02-Testes.md](commands/02-Testes.md) | Relatório de cobertura de teste + análise de lacunas |
| `/python-testing` | [02-Testes.md](commands/02-Testes.md) | Melhores práticas de teste Python |

### 3.3 Revisão de Linguagem (7)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/python-review` | [03-Revisão-de-Idioma.md](commands/03-Revisão-de-Idioma.md) | Python PEP 8, dicas de tipo, segurança |
| `/go-review` | [03-Revisão-de-Idioma.md](commands/03-Revisão-de-Idioma.md) | Idiomas Go, concorrência, tratamento de erros |
| `/kotlin-review` | [03-Revisão-de-Idioma.md](commands/03-Revisão-de-Idioma.md) | Segurança nula Kotlin, corrotinas, arquitetura |
| `/rust-review` | [03-Revisão-de-Idioma.md](commands/03-Revisão-de-Idioma.md) | Propriedade Rust, tempos de vida, unsafe |
| `/cpp-review` | [03-Revisão-de-Idioma.md](commands/03-Revisão-de-Idioma.md) | Segurança de memória C++, idiomas modernos |
| `/flutter-review` | [03-Revisão-de-Idioma.md](commands/03-Revisão-de-Idioma.md) | Padrões Flutter/Dart |
| `/fastapi-review` | [03-Revisão-de-Idioma.md](commands/03-Revisão-de-Idioma.md) | Arquitetura FastAPI, async, Pydantic |

### 3.4 Build e Correção (6)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/go-build` | [04-Reparo-de-Build.md](commands/04-Reparo-de-Build.md) | Corrigir erros de build Go + avisos go vet |
| `/kotlin-build` | [04-Reparo-de-Build.md](commands/04-Reparo-de-Build.md) | Corrigir erros do compilador Kotlin/Gradle |
| `/rust-build` | [04-Reparo-de-Build.md](commands/04-Reparo-de-Build.md) | Corrigir build Rust + problemas do verificador de empréstimo |
| `/cpp-build` | [04-Reparo-de-Build.md](commands/04-Reparo-de-Build.md) | Corrigir problemas de CMake + linker C++ |
| `/gradle-build` | [04-Reparo-de-Build.md](commands/04-Reparo-de-Build.md) | Corrigir erros Android/KMP Gradle |
| `/flutter-build` | [04-Reparo-de-Build.md](commands/04-Reparo-de-Build.md) | Correções de build Flutter |

### 3.5 Planejamento e Arquitetura (7)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/plan-prd` | [05-Planejamento-Arquitetura.md](commands/05-Planejamento-Arquitetura.md) | Gerador de PRD interativo |
| `/prp-plan` | [05-Planejamento-Arquitetura.md](commands/05-Planejamento-Arquitetura.md) | Planejamento abrangente de features |
| `/prp-prd` | [05-Planejamento-Arquitetura.md](commands/05-Planejamento-Arquitetura.md) | Gerador de PRD de fluxo de trabalho PRP |
| `/prp-implement` | [05-Planejamento-Arquitetura.md](commands/05-Planejamento-Arquitetura.md) | Executar plano PRP + loop de verificação |
| `/prp-pr` | [05-Planejamento-Arquitetura.md](commands/05-Planejamento-Arquitetura.md) | Criar PR do fluxo de trabalho PRP |
| `/prp-commit` | [05-Planejamento-Arquitetura.md](commands/05-Planejamento-Arquitetura.md) | Commit verificado PRP |
| `/multi-plan` | [05-Planejamento-Arquitetura.md](commands/05-Planejamento-Arquitetura.md) | Planejamento colaborativo multi-modelo (Codex + Gemini) |

### 3.6 Gerenciamento de Sessão (5)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/save-session` | [06-Gerenciamento-de-Sessão.md](commands/06-Gerenciamento-de-Sessão.md) | Salvar estado da sessão |
| `/resume-session` | [06-Gerenciamento-de-Sessão.md](commands/06-Gerenciamento-de-Sessão.md) | Carregar e restaurar sessão salva |
| `/sessions` | [06-Gerenciamento-de-Sessão.md](commands/06-Gerenciamento-de-Sessão.md) | Navegar + buscar + gerenciar histórico de sessões |
| `/checkpoint` | [06-Gerenciamento-de-Sessão.md](commands/06-Gerenciamento-de-Sessão.md) | Criar / verificar checkpoint de workflow |
| `/aside` | [06-Gerenciamento-de-Sessão.md](commands/06-Gerenciamento-de-Sessão.md) | Responder perguntas secundárias sem perder contexto |

### 3.7 Aprendizado e Melhoria (10)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/learn` | [07-Aprendizado-Melhoria.md](commands/07-Aprendizado-Melhoria.md) | Extrair padrões reutilizáveis da sessão |
| `/learn-eval` | [07-Aprendizado-Melhoria.md](commands/07-Aprendizado-Melhoria.md) | Extrair padrões + auto-avaliação de qualidade |
| `/evolve` | [07-Aprendizado-Melhoria.md](commands/07-Aprendizado-Melhoria.md) | Analisar instinct + sugerir estrutura de evolução |
| `/promote` | [07-Aprendizado-Melhoria.md](commands/07-Aprendizado-Melhoria.md) | Promover instinct de projeto para escopo global |
| `/instinct-status` | [07-Aprendizado-Melhoria.md](commands/07-Aprendizado-Melhoria.md) | Mostrar todos os instincts aprendidos |
| `/instinct-export` | [07-Aprendizado-Melhoria.md](commands/07-Aprendizado-Melhoria.md) | Exportar instinct para arquivo |
| `/instinct-import` | [07-Aprendizado-Melhoria.md](commands/07-Aprendizado-Melhoria.md) | Importar instinct de arquivo / URL |
| `/skill-create` | [07-Aprendizado-Melhoria.md](commands/07-Aprendizado-Melhoria.md) | Analisar histórico git + gerar arquivo skill |
| `/skill-health` | [07-Aprendizado-Melhoria.md](commands/07-Aprendizado-Melhoria.md) | Painel de saúde de composição de Skills |
| `/rules-distill` | [07-Aprendizado-Melhoria.md](commands/07-Aprendizado-Melhoria.md) | Escanear skills + extrair princípios entre domínios |

### 3.8 Loops e Automação (3)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/loop-start` | [08-Loop-Automação.md](commands/08-Loop-Automação.md) | Iniciar modo de loop autônomo gerenciado |
| `/loop-status` | [08-Loop-Automação.md](commands/08-Loop-Automação.md) | Verificar status do loop em execução |
| `/santa-loop` | [08-Loop-Automação.md](commands/08-Loop-Automação.md) | Loop autônomo estilo Santa |

### 3.9 Gerenciamento de Projetos (6)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/projects` | [09-Gerenciamento-de-Projeto.md](commands/09-Gerenciamento-de-Projeto.md) | Listar projetos conhecidos + estatísticas de instinct |
| `/harness-audit` | [09-Gerenciamento-de-Projeto.md](commands/09-Gerenciamento-de-Projeto.md) | Auditar configuração do agent harness |
| `/model-route` | [09-Gerenciamento-de-Projeto.md](commands/09-Gerenciamento-de-Projeto.md) | Rotear tarefas para modelo apropriado |
| `/pm2` | [09-Gerenciamento-de-Projeto.md](commands/09-Gerenciamento-de-Projeto.md) | Inicialização do gerenciador de processos PM2 |
| `/setup-pm` | [09-Gerenciamento-de-Projeto.md](commands/09-Gerenciamento-de-Projeto.md) | Configurar gerenciador de pacotes |
| `/project-init` | [09-Gerenciamento-de-Projeto.md](commands/09-Gerenciamento-de-Projeto.md) | Inicialização de projeto |

### 3.10 Fluxo de Trabalho PR (6)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/pr` | [10-Workflow-PR.md](commands/10-Workflow-PR.md) | Criar PR do GitHub do branch atual |
| `/review-pr` | [10-Workflow-PR.md](commands/10-Workflow-PR.md) | Revisar PR do GitHub |
| `/multi-workflow` | [10-Workflow-PR.md](commands/10-Workflow-PR.md) | Desenvolvimento colaborativo multi-modelo |
| `/multi-backend` | [10-Workflow-PR.md](commands/10-Workflow-PR.md) | Desenvolvimento backend multi-modelo |
| `/multi-frontend` | [10-Workflow-PR.md](commands/10-Workflow-PR.md) | Desenvolvimento frontend multi-modelo |
| `/multi-execute` | [10-Workflow-PR.md](commands/10-Workflow-PR.md) | Execução colaborativa multi-modelo |

### 3.11 Sistema Hookify (4)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/hookify` | [11-Sistema-Hookify.md](commands/11-Sistema-Hookify.md) | Criar hooks para prevenir maus comportamentos |
| `/hookify-list` | [11-Sistema-Hookify.md](commands/11-Sistema-Hookify.md) | Listar todas as regras hookify configuradas |
| `/hookify-configure` | [11-Sistema-Hookify.md](commands/11-Sistema-Hookify.md) | Ativar / desativar regras hookify interativamente |
| `/hookify-help` | [11-Sistema-Hookify.md](commands/11-Sistema-Hookify.md) | Ajuda do sistema Hookify |

### 3.12 Documentação e Pesquisa (3)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/update-docs` | [12-Documentação-Pesquisa.md](commands/12-Documentação-Pesquisa.md) | Atualizar documentação do projeto |
| `/update-codemaps` | [12-Documentação-Pesquisa.md](commands/12-Documentação-Pesquisa.md) | Regenerar codemaps |
| `/ecc-guide` | [12-Documentação-Pesquisa.md](commands/12-Documentação-Pesquisa.md) | Guia do usuário ECC |

### 3.13 Refatoração e Limpeza (2)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/refactor-clean` | [13-Refatoração-Limpeza.md](commands/13-Refatoração-Limpeza.md) | Excluir código morto + mesclar duplicatas |
| `/auto-update` | [13-Refatoração-Limpeza.md](commands/13-Refatoração-Limpeza.md) | Capacidades de auto-atualização |

### 3.14 Outros Comandos (7)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/jira` | [14-Outros-Comandos.md](commands/14-Outros-Comandos.md) | Interação com ticket Jira |
| `/gan-build` | [14-Outros-Comandos.md](commands/14-Outros-Comandos.md) | Operações de build GAN |
| `/gan-design` | [14-Outros-Comandos.md](commands/14-Outros-Comandos.md) | Operações de design GAN |
| `/prune` | [14-Outros-Comandos.md](commands/14-Outros-Comandos.md) | Excluir instincts obsoletos (>30 dias) |
| `/security-scan` | [14-Outros-Comandos.md](commands/14-Outros-Comandos.md) | Varredura de segurança |
| `/feature-dev` | [14-Outros-Comandos.md](commands/14-Outros-Comandos.md) | Assistente de desenvolvimento de features |
| `/cost-report` | [14-Outros-Comandos.md](commands/14-Outros-Comandos.md) | Relatório de custos do modelo |

---

## 4. Índice de Agents

Total de **60 Agents**, agrupados por categoria:

| Categoria de Agent | Arquivo | Descrição |
|----------------|------|-------------|
| [Revisão de Código](agents/01-Revisão-de-Código.md) | [01-Revisão-de-Código.md](agents/01-Revisão-de-Código.md) | 14 Agents de revisão |
| [Build e Correção](agents/02-Correção-de-Build.md) | [02-Correção-de-Build.md](agents/02-Correção-de-Build.md) | 14 Agents de correção de build |
| [Planejamento](agents/03-Planejamento.md) | [03-Planejamento.md](agents/03-Planejamento.md) | 5 Agents de planejamento |
| [Testes](agents/04-Testes.md) | [04-Testes.md](agents/04-Testes.md) | 2 Agents de teste |
| [Segurança](agents/05-Segurança.md) | [05-Segurança.md](agents/05-Segurança.md) | 3 Agents de segurança |
| [Arquitetura](agents/06-Arquitetura.md) | [06-Arquitetura.md](agents/06-Arquitetura.md) | 3 Agents de arquitetura |

---

## 5. Índice de Skills

**16 domínios de Skills**, categorizados por domínio:

| Domínio | Arquivo | Descrição |
|--------|------|-------------|
| [Melhores Práticas](skills/MelhoresPráticas.md) | [Melhores-Práticas.md](skills/Melhores-Práticas.md) | Padrões de codificação, tratamento de erros, loops autônomos |
| [Linguagens de Programação](skills/LinguagensdeProgramação.md) | [Linguagens-de-Programação.md](skills/Linguagens-de-Programação.md) | Python/Go/Rust/Kotlin/C++ etc. |
| [Frameworks](skills/Frameworks.md) | [Frameworks.md](skills/Frameworks.md) | Django/Laravel/NestJS/Spring Boot etc. |
| [Testes](skills/Testes.md) | [Testes.md](skills/Testes.md) | TDD/Teste Unitário/Teste de Integração/E2E |
| [Segurança](skills/Segurança.md) | [Segurança.md](skills/Segurança.md) | Revisão de segurança, scanning de vulnerabilidades |
| [Frontend e Design](skills/FrontendeDesign.md) | [Frontend-e-Design.md](skills/Frontend-e-Design.md) | Desenvolvimento frontend, sistemas de design |
| [Backend e API](skills/BackendeAPI.md) | [Backend-e-API.md](skills/Backend-e-API.md) | Serviços backend, design de API, bancos de dados |
| [Deployment e DevOps](skills/DeploymenteDevOps.md) | [Implantação-e-DevOps.md](skills/Implantação-e-DevOps.md) | Docker/K8s/estratégias de deploy |
| [Monitoramento e Observabilidade](skills/MonitoramentoeObservabilidade.md) | [Monitoramento-e-Observabilidade.md](skills/Monitoramento-e-Observabilidade.md) | Observabilidade, diagnóstico de rede |
| [Automação e Scripting](skills/AutomaçãoeScripting.md) | [Automação-e-Scripts.md](skills/Automação-e-Scripts.md) | Loops autônomos, aprendizado contínuo, engenharia de agentes |
| [Busca e Recuperação de Dados](skills/BuscaeRecuperaçãodeDados.md) | [Busca-e-Aquisição-de-Dados.md](skills/Busca-e-Aquisição-de-Dados.md) | Busca Exa, web scraping, MCP |
| [GitHub e Colaboração](skills/GitHubeColaboração.md) | [GitHub-e-Colaboração.md](skills/GitHub-e-Colaboração.md) | Fluxos de trabalho GitHub, revisão de código |
| [AI e Machine Learning](skills/AIeMachineLearning.md) | [IA-e-Aprendizado-de-Máquina.md](skills/IA-e-Aprendizado-de-Máquina.md) | Redes neurais, PyTorch, MLOps |
| [Cloud Native e Infraestrutura](skills/CloudNativeeInfraestrutura.md) | [Nativo-de-Nuvem-e-Infraestrutura.md](skills/Nativo-de-Nuvem-e-Infraestrutura.md) | Kubernetes, Docker, Terraform |
| [Skills Especializados](skills/SkillsEspecializados.md) | [Habilidades-de-Domínio-Especializado.md](skills/Habilidades-de-Domínio-Especializado.md) | Blockchain, desenvolvimento de jogos, áudio/vídeo, IoT |
| [Toolchain de Desenvolvimento](skills/ToolchandeDesenvolvimento.md) | [Cadeia-de-Ferramentas-de-Desenvolvimento.md](skills/Cadeia-de-Ferramentas-de-Desenvolvimento.md) | Frameworks de teste, CI/CD, qualidade de código |
| [Tecnologia de Ponta](skills/TecnologiadePonta.md) | [TecnologiadePonta.md](skills/TecnologiadePonta.md) | AI Agent, computação quântica, edge computing |

---

## 6. Índice de Regras

Total de **7 documentos de regras** (common + específico por linguagem):

| Regras | Arquivo | Descrição |
|-------|------|-------------|
| [Fluxo de Trabalho Git](rules/FluxodeTrabalhoGit.md) | [Git-Workflow.md](rules/Git-Workflow.md) | Padrões de commit Git e fluxo de trabalho PR |
| [Sistema de Hooks](rules/SistemadeHooks.md) | [Sistema-Hooks.md](rules/Sistema-Hooks.md) | Guia de configuração e uso de hooks |
| [Orquestração de Agents](rules/OrquestraçãodeAgents.md) | [Orquestração-de-Agentes.md](rules/Orquestração-de-Agentes.md) | Padrões de orquestração de agents |
| [Otimização de Performance](rules/OtimizaçãodePerformance.md) | [Otimização-de-Desempenho.md](rules/Otimização-de-Desempenho.md) | Guia de otimização de performance |
| [Estilo de Código](rules/EstilodeCódigo.md) | [Estilo-de-Código.md](rules/Estilo-de-Código.md) | Padrões de estilo de codificação |
| [Regras de Teste](rules/RegrasdeTeste.md) | [Regras-de-Teste.md](rules/Regras-de-Teste.md) | Requisitos de teste (80% cobertura) |
| [Regras de Segurança](rules/RegrasdeSegurança.md) | [Regras-de-Segurança.md](rules/Regras-de-Segurança.md) | Lista de verificação de segurança |

---

## 7. Índice de Hooks

Total de **4 documentos**:

| Documento | Arquivo | Descrição |
|----------|------|-------------|
| [Tipos de Hook](hooks/TiposdeHook.md) | [Tipos-de-Hook.md](hooks/Tipos-de-Hook.md) | Tipos PreToolUse, PostToolUse, Stop |
| [Hooks Embutidos](hooks/HooksEmbutidos.md) | [Hooks-Incorporados.md](hooks/Hooks-Incorporados.md) | Lista de hooks embutidos e uso |
| [Desenvolvimento Personalizado](hooks/DesenvolvimentoPersonalizado.md) | [Desenvolvimento-Customizado.md](hooks/Desenvolvimento-Customizado.md) | Guia de desenvolvimento de hooks personalizados |
| [Formato de Configuração](hooks/FormadeConfiguração.md) | [Formato-de-Configuração.md](hooks/Formato-de-Configuração.md) | Formato de configuração hooks.json |

---

## 8. Índice de MCP

Total de **6 configurações de servidor MCP**:

| Documento | Arquivo | Descrição |
|----------|------|-------------|
| [Formato de Configuração MCP](mcp/FormadeConfiguraçãoMCP.md) | [Formato-de-Configuração-MCP.md](mcp/Formato-de-Configuração-MCP.md) | Formato de arquivo de configuração MCP |
| [Servidores Embutidos](mcp/ServidoresEmbutidos.md) | [Servidores-Incorporados.md](mcp/Servidores-Incorporados.md) | Servidores MCP embutidos |
| [Desenvolvimento Personalizado](mcp/DesenvolvimentoPersonalizado.md) | [Desenvolvimento-Customizado-MCP.md](mcp/Desenvolvimento-Customizado-MCP.md) | Desenvolvimento de servidor MCP personalizado |

---

## 9. Índice de Scripts

Total de **54 scripts de utilitário**:

| Documento | Arquivo | Descrição |
|----------|------|-------------|
| [Scripts de Utilitário](scripts/ScriptsdeUtilitário.md) | [Scripts-de-Utilitários.md](scripts/Scripts-de-Utilitários.md) | ecc.js, install-apply.js etc. |
| [Biblioteca de Funções de Utilitário](scripts/BibliotecadeFunçõesdeUtilitário.md) | [Biblioteca-de-Funções-de-Utilitários.md](scripts/Biblioteca-de-Funções-de-Utilitários.md) | Biblioteca de funções compartilhadas |
| [Executor de Testes](scripts/ExecutordeTestes.md) | [Executor-de-Testes.md](scripts/Executor-de-Testes.md) | Uso do executor de testes |
| [Scripts de Build](scripts/ScriptsdeBuild.md) | [Scripts-de-Build.md](scripts/Scripts-de-Build.md) | Scripts de build |

---

## 10. Guia de Contribuição

### Convenções de Nome de Arquivo

- **Comandos, Skills, Agents, Hooks**: Minúsculas + hífen (ex.: `code-review.md`)
- **Scripts**: camelCase ou kebab-case (ex.: `session-start.js`)
- **Regras**: Organizado por linguagem / tópico

### Formato de Arquivo de Comando

```yaml
---
description: "Breve descrição"
argument-hint: "[Parâmetro opcional]"
name: command-name
command: true
allowed_tools: ["Bash"]
---
```

### Formato de Arquivo de Agent

```yaml
---
name: agent-name
description: Propósito do Agent
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

### Formato de Arquivo de Skill

```markdown
# Nome do Skill

## When to Use
## How It Works
## Examples
```

### Processo de Submissão

1. Criar novo arquivo ou modificar arquivo existente
2. Garantir seguir convenções de nomenclatura
3. Executar testes: `node tests/run-all.js`
4. Executar markdown lint: `npx markdownlint-cli '**/*.md' --ignore node_modules`
5. Criar PR para revisão

---

*Sistema de Documentação ECC - Fluxos de trabalho de desenvolvimento de nível de produção para Claude Code*