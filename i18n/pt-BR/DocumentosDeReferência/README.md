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
| `/plan` | [01-FluxosDeTrabalhoPrincipais.md](commands/01-FluxosDeTrabalhoPrincipais.md) | Análise de requisitos + avaliação de risco + plano passo a passo |
| `/code-review` | [01-FluxosDeTrabalhoPrincipais.md](commands/01-FluxosDeTrabalhoPrincipais.md) | Revisão de qualidade / segurança / manutenibilidade do código |
| `/build-fix` | [01-FluxosDeTrabalhoPrincipais.md](commands/01-FluxosDeTrabalhoPrincipais.md) | Detecção automática de linguagem + correção incremental de erros de build |
| `/verify` | [01-FluxosDeTrabalhoPrincipais.md](commands/01-FluxosDeTrabalhoPrincipais.md) | Loop de verificação completo: build → lint → teste → verificação de tipo |
| `/quality-gate` | [01-FluxosDeTrabalhoPrincipais.md](commands/01-FluxosDeTrabalhoPrincipais.md) | Executar pipeline de qualidade ECC sob demanda |
| `/tdd` | [01-FluxosDeTrabalhoPrincipais.md](commands/01-FluxosDeTrabalhoPrincipais.md) | Fluxo de trabalho TDD geral |

### 3.2 Testes Relacionados (8)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/go-test` | [02-TestesRelacionados.md](commands/02-TestesRelacionados.md) | TDD Go (orientado por tabela, 80%+ cobertura) |
| `/kotlin-test` | [02-TestesRelacionados.md](commands/02-TestesRelacionados.md) | TDD Kotlin (Kotest + Kover) |
| `/rust-test` | [02-TestesRelacionados.md](commands/02-TestesRelacionados.md) | TDD Rust (cargo test + testes de integração) |
| `/cpp-test` | [02-TestesRelacionados.md](commands/02-TestesRelacionados.md) | TDD C++ (GoogleTest + gcov/lcov) |
| `/flutter-test` | [02-TestesRelacionados.md](commands/02-TestesRelacionados.md) | TDD Flutter |
| `/e2e` | [02-TestesRelacionados.md](commands/02-TestesRelacionados.md) | Gerar + executar testes e2e Playwright |
| `/test-coverage` | [02-TestesRelacionados.md](commands/02-TestesRelacionados.md) | Relatório de cobertura de teste + análise de lacunas |
| `/python-testing` | [02-TestesRelacionados.md](commands/02-TestesRelacionados.md) | Melhores práticas de teste Python |

### 3.3 Revisão de Linguagem (7)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/python-review` | [03-RevisãoDeLinguagem.md](commands/03-RevisãoDeLinguagem.md) | Python PEP 8, dicas de tipo, segurança |
| `/go-review` | [03-RevisãoDeLinguagem.md](commands/03-RevisãoDeLinguagem.md) | Idiomas Go, concorrência, tratamento de erros |
| `/kotlin-review` | [03-RevisãoDeLinguagem.md](commands/03-RevisãoDeLinguagem.md) | Segurança nula Kotlin, corrotinas, arquitetura |
| `/rust-review` | [03-RevisãoDeLinguagem.md](commands/03-RevisãoDeLinguagem.md) | Propriedade Rust, tempos de vida, unsafe |
| `/cpp-review` | [03-RevisãoDeLinguagem.md](commands/03-RevisãoDeLinguagem.md) | Segurança de memória C++, idiomas modernos |
| `/flutter-review` | [03-RevisãoDeLinguagem.md](commands/03-RevisãoDeLinguagem.md) | Padrões Flutter/Dart |
| `/fastapi-review` | [03-RevisãoDeLinguagem.md](commands/03-RevisãoDeLinguagem.md) | Arquitetura FastAPI, async, Pydantic |

### 3.4 Build e Correção (6)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/go-build` | [04-BuildECorreção.md](commands/04-BuildECorreção.md) | Corrigir erros de build Go + avisos go vet |
| `/kotlin-build` | [04-BuildECorreção.md](commands/04-BuildECorreção.md) | Corrigir erros do compilador Kotlin/Gradle |
| `/rust-build` | [04-BuildECorreção.md](commands/04-BuildECorreção.md) | Corrigir build Rust + problemas do verificador de empréstimo |
| `/cpp-build` | [04-BuildECorreção.md](commands/04-BuildECorreção.md) | Corrigir problemas de CMake + linker C++ |
| `/gradle-build` | [04-BuildECorreção.md](commands/04-BuildECorreção.md) | Corrigir erros Android/KMP Gradle |
| `/flutter-build` | [04-BuildECorreção.md](commands/04-BuildECorreção.md) | Correções de build Flutter |

### 3.5 Planejamento e Arquitetura (7)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/plan-prd` | [05-PlanejamentoEArquitetura.md](commands/05-PlanejamentoEArquitetura.md) | Gerador de PRD interativo |
| `/prp-plan` | [05-PlanejamentoEArquitetura.md](commands/05-PlanejamentoEArquitetura.md) | Planejamento abrangente de features |
| `/prp-prd` | [05-PlanejamentoEArquitetura.md](commands/05-PlanejamentoEArquitetura.md) | Gerador de PRD de fluxo de trabalho PRP |
| `/prp-implement` | [05-PlanejamentoEArquitetura.md](commands/05-PlanejamentoEArquitetura.md) | Executar plano PRP + loop de verificação |
| `/prp-pr` | [05-PlanejamentoEArquitetura.md](commands/05-PlanejamentoEArquitetura.md) | Criar PR do fluxo de trabalho PRP |
| `/prp-commit` | [05-PlanejamentoEArquitetura.md](commands/05-PlanejamentoEArquitetura.md) | Commit verificado PRP |
| `/multi-plan` | [05-PlanejamentoEArquitetura.md](commands/05-PlanejamentoEArquitetura.md) | Planejamento colaborativo multi-modelo (Codex + Gemini) |

### 3.6 Gerenciamento de Sessão (5)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/save-session` | [06-GerenciamentoDeSessão.md](commands/06-GerenciamentoDeSessão.md) | Salvar estado da sessão |
| `/resume-session` | [06-GerenciamentoDeSessão.md](commands/06-GerenciamentoDeSessão.md) | Carregar e restaurar sessão salva |
| `/sessions` | [06-GerenciamentoDeSessão.md](commands/06-GerenciamentoDeSessão.md) | Navegar + buscar + gerenciar histórico de sessões |
| `/checkpoint` | [06-GerenciamentoDeSessão.md](commands/06-GerenciamentoDeSessão.md) | Criar / verificar checkpoint de workflow |
| `/aside` | [06-GerenciamentoDeSessão.md](commands/06-GerenciamentoDeSessão.md) | Responder perguntas secundárias sem perder contexto |

### 3.7 Aprendizado e Melhoria (10)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/learn` | [07-AprendizadoEMelhoria.md](commands/07-AprendizadoEMelhoria.md) | Extrair padrões reutilizáveis da sessão |
| `/learn-eval` | [07-AprendizadoEMelhoria.md](commands/07-AprendizadoEMelhoria.md) | Extrair padrões + auto-avaliação de qualidade |
| `/evolve` | [07-AprendizadoEMelhoria.md](commands/07-AprendizadoEMelhoria.md) | Analisar instinct + sugerir estrutura de evolução |
| `/promote` | [07-AprendizadoEMelhoria.md](commands/07-AprendizadoEMelhoria.md) | Promover instinct de projeto para escopo global |
| `/instinct-status` | [07-AprendizadoEMelhoria.md](commands/07-AprendizadoEMelhoria.md) | Mostrar todos os instincts aprendidos |
| `/instinct-export` | [07-AprendizadoEMelhoria.md](commands/07-AprendizadoEMelhoria.md) | Exportar instinct para arquivo |
| `/instinct-import` | [07-AprendizadoEMelhoria.md](commands/07-AprendizadoEMelhoria.md) | Importar instinct de arquivo / URL |
| `/skill-create` | [07-AprendizadoEMelhoria.md](commands/07-AprendizadoEMelhoria.md) | Analisar histórico git + gerar arquivo skill |
| `/skill-health` | [07-AprendizadoEMelhoria.md](commands/07-AprendizadoEMelhoria.md) | Painel de saúde de composição de Skills |
| `/rules-distill` | [07-AprendizadoEMelhoria.md](commands/07-AprendizadoEMelhoria.md) | Escanear skills + extrair princípios entre domínios |

### 3.8 Loops e Automação (3)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/loop-start` | [08-LoopsEAutomação.md](commands/08-LoopsEAutomação.md) | Iniciar modo de loop autônomo gerenciado |
| `/loop-status` | [08-LoopsEAutomação.md](commands/08-LoopsEAutomação.md) | Verificar status do loop em execução |
| `/santa-loop` | [08-LoopsEAutomação.md](commands/08-LoopsEAutomação.md) | Loop autônomo estilo Santa |

### 3.9 Gerenciamento de Projetos (6)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/projects` | [09-GerenciamentoDeProjetos.md](commands/09-GerenciamentoDeProjetos.md) | Listar projetos conhecidos + estatísticas de instinct |
| `/harness-audit` | [09-GerenciamentoDeProjetos.md](commands/09-GerenciamentoDeProjetos.md) | Auditar configuração do agent harness |
| `/model-route` | [09-GerenciamentoDeProjetos.md](commands/09-GerenciamentoDeProjetos.md) | Rotear tarefas para modelo apropriado |
| `/pm2` | [09-GerenciamentoDeProjetos.md](commands/09-GerenciamentoDeProjetos.md) | Inicialização do gerenciador de processos PM2 |
| `/setup-pm` | [09-GerenciamentoDeProjetos.md](commands/09-GerenciamentoDeProjetos.md) | Configurar gerenciador de pacotes |
| `/project-init` | [09-GerenciamentoDeProjetos.md](commands/09-GerenciamentoDeProjetos.md) | Inicialização de projeto |

### 3.10 Fluxo de Trabalho PR (6)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/pr` | [10-FluxoDeTrabalhoPR.md](commands/10-FluxoDeTrabalhoPR.md) | Criar PR do GitHub do branch atual |
| `/review-pr` | [10-FluxoDeTrabalhoPR.md](commands/10-FluxoDeTrabalhoPR.md) | Revisar PR do GitHub |
| `/multi-workflow` | [10-FluxoDeTrabalhoPR.md](commands/10-FluxoDeTrabalhoPR.md) | Desenvolvimento colaborativo multi-modelo |
| `/multi-backend` | [10-FluxoDeTrabalhoPR.md](commands/10-FluxoDeTrabalhoPR.md) | Desenvolvimento backend multi-modelo |
| `/multi-frontend` | [10-FluxoDeTrabalhoPR.md](commands/10-FluxoDeTrabalhoPR.md) | Desenvolvimento frontend multi-modelo |
| `/multi-execute` | [10-FluxoDeTrabalhoPR.md](commands/10-FluxoDeTrabalhoPR.md) | Execução colaborativa multi-modelo |

### 3.11 Sistema Hookify (4)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/hookify` | [11-SistemaHookify.md](commands/11-SistemaHookify.md) | Criar hooks para prevenir maus comportamentos |
| `/hookify-list` | [11-SistemaHookify.md](commands/11-SistemaHookify.md) | Listar todas as regras hookify configuradas |
| `/hookify-configure` | [11-SistemaHookify.md](commands/11-SistemaHookify.md) | Ativar / desativar regras hookify interativamente |
| `/hookify-help` | [11-SistemaHookify.md](commands/11-SistemaHookify.md) | Ajuda do sistema Hookify |

### 3.12 Documentação e Pesquisa (3)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/update-docs` | [12-DocumentaçãoEPesquisa.md](commands/12-DocumentaçãoEPesquisa.md) | Atualizar documentação do projeto |
| `/update-codemaps` | [12-DocumentaçãoEPesquisa.md](commands/12-DocumentaçãoEPesquisa.md) | Regenerar codemaps |
| `/ecc-guide` | [12-DocumentaçãoEPesquisa.md](commands/12-DocumentaçãoEPesquisa.md) | Guia do usuário ECC |

### 3.13 Refatoração e Limpeza (2)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/refactor-clean` | [13-RefatoraçãoELimpeza.md](commands/13-RefatoraçãoELimpeza.md) | Excluir código morto + mesclar duplicatas |
| `/auto-update` | [13-RefatoraçãoELimpeza.md](commands/13-RefatoraçãoELimpeza.md) | Capacidades de auto-atualização |

### 3.14 Outros Comandos (7)

| Comando | Arquivo | Finalidade |
|--------|------|-------|
| `/jira` | [14-OutrosComandos.md](commands/14-OutrosComandos.md) | Interação com ticket Jira |
| `/gan-build` | [14-OutrosComandos.md](commands/14-OutrosComandos.md) | Operações de build GAN |
| `/gan-design` | [14-OutrosComandos.md](commands/14-OutrosComandos.md) | Operações de design GAN |
| `/prune` | [14-OutrosComandos.md](commands/14-OutrosComandos.md) | Excluir instincts obsoletos (>30 dias) |
| `/security-scan` | [14-OutrosComandos.md](commands/14-OutrosComandos.md) | Varredura de segurança |
| `/feature-dev` | [14-OutrosComandos.md](commands/14-OutrosComandos.md) | Assistente de desenvolvimento de features |
| `/cost-report` | [14-OutrosComandos.md](commands/14-OutrosComandos.md) | Relatório de custos do modelo |

---

## 4. Índice de Agents

Total de **60 Agents**, agrupados por categoria:

| Categoria de Agent | Arquivo | Descrição |
|----------------|------|-------------|
| [Revisão de Código](agents/1.%20RevisãoDeCódigo.md) | [1. RevisãoDeCódigo.md](agents/1.%20RevisãoDeCódigo.md) | 14 Agents de revisão |
| [Build e Correção](agents/2.%20BuildECorreção.md) | [2. BuildECorreção.md](agents/2.%20BuildECorreção.md) | 14 Agents de correção de build |
| [Planejamento](agents/3.%20Planejamento.md) | [3. Planejamento.md](agents/3.%20Planejamento.md) | 5 Agents de planejamento |
| [Testes](agents/4.%20Testes.md) | [4. Testes.md](agents/4.%20Testes.md) | 2 Agents de teste |
| [Segurança](agents/5.%20Segurança.md) | [5. Segurança.md](agents/5.%20Segurança.md) | 3 Agents de segurança |
| [Arquitetura](agents/6.%20Arquitetura.md) | [6. Arquitetura.md](agents/6.%20Arquitetura.md) | 3 Agents de arquitetura |

---

## 5. Índice de Skills

**16 domínios de Skills**, categorizados por domínio:

| Domínio | Arquivo | Descrição |
|--------|------|-------------|
| [Melhores Práticas](skills/MelhoresPráticas.md) | [MelhoresPráticas.md](skills/MelhoresPráticas.md) | Padrões de codificação, tratamento de erros, loops autônomos |
| [Linguagens de Programação](skills/LinguagensdeProgramação.md) | [LinguagensdeProgramação.md](skills/LinguagensdeProgramação.md) | Python/Go/Rust/Kotlin/C++ etc. |
| [Frameworks](skills/Frameworks.md) | [Frameworks.md](skills/Frameworks.md) | Django/Laravel/NestJS/Spring Boot etc. |
| [Testes](skills/Testes.md) | [Testes.md](skills/Testes.md) | TDD/Teste Unitário/Teste de Integração/E2E |
| [Segurança](skills/Segurança.md) | [Segurança.md](skills/Segurança.md) | Revisão de segurança, scanning de vulnerabilidades |
| [Frontend e Design](skills/FrontendeDesign.md) | [FrontendeDesign.md](skills/FrontendeDesign.md) | Desenvolvimento frontend, sistemas de design |
| [Backend e API](skills/BackendeAPI.md) | [BackendeAPI.md](skills/BackendeAPI.md) | Serviços backend, design de API, bancos de dados |
| [Deployment e DevOps](skills/DeploymenteDevOps.md) | [DeploymenteDevOps.md](skills/DeploymenteDevOps.md) | Docker/K8s/estratégias de deploy |
| [Monitoramento e Observabilidade](skills/MonitoramentoeObservabilidade.md) | [MonitoramentoeObservabilidade.md](skills/MonitoramentoeObservabilidade.md) | Observabilidade, diagnóstico de rede |
| [Automação e Scripting](skills/AutomaçãoeScripting.md) | [AutomaçãoeScripting.md](skills/AutomaçãoeScripting.md) | Loops autônomos, aprendizado contínuo, engenharia de agentes |
| [Busca e Recuperação de Dados](skills/BuscaeRecuperaçãodeDados.md) | [BuscaeRecuperaçãodeDados.md](skills/BuscaeRecuperaçãodeDados.md) | Busca Exa, web scraping, MCP |
| [GitHub e Colaboração](skills/GitHubeColaboração.md) | [GitHubeColaboração.md](skills/GitHubeColaboração.md) | Fluxos de trabalho GitHub, revisão de código |
| [AI e Machine Learning](skills/AIeMachineLearning.md) | [AIeMachineLearning.md](skills/AIeMachineLearning.md) | Redes neurais, PyTorch, MLOps |
| [Cloud Native e Infraestrutura](skills/CloudNativeeInfraestrutura.md) | [CloudNativeeInfraestrutura.md](skills/CloudNativeeInfraestrutura.md) | Kubernetes, Docker, Terraform |
| [Skills Especializados](skills/SkillsEspecializados.md) | [SkillsEspecializados.md](skills/SkillsEspecializados.md) | Blockchain, desenvolvimento de jogos, áudio/vídeo, IoT |
| [Toolchain de Desenvolvimento](skills/ToolchandeDesenvolvimento.md) | [ToolchandeDesenvolvimento.md](skills/ToolchandeDesenvolvimento.md) | Frameworks de teste, CI/CD, qualidade de código |
| [Tecnologia de Ponta](skills/TecnologiadePonta.md) | [TecnologiadePonta.md](skills/TecnologiadePonta.md) | AI Agent, computação quântica, edge computing |

---

## 6. Índice de Regras

Total de **7 documentos de regras** (common + específico por linguagem):

| Regras | Arquivo | Descrição |
|-------|------|-------------|
| [Fluxo de Trabalho Git](rules/FluxodeTrabalhoGit.md) | [FluxodeTrabalhoGit.md](rules/FluxodeTrabalhoGit.md) | Padrões de commit Git e fluxo de trabalho PR |
| [Sistema de Hooks](rules/SistemadeHooks.md) | [SistemadeHooks.md](rules/SistemadeHooks.md) | Guia de configuração e uso de hooks |
| [Orquestração de Agents](rules/OrquestraçãodeAgents.md) | [OrquestraçãodeAgents.md](rules/OrquestraçãodeAgents.md) | Padrões de orquestração de agents |
| [Otimização de Performance](rules/OtimizaçãodePerformance.md) | [OtimizaçãodePerformance.md](rules/OtimizaçãodePerformance.md) | Guia de otimização de performance |
| [Estilo de Código](rules/EstilodeCódigo.md) | [EstilodeCódigo.md](rules/EstilodeCódigo.md) | Padrões de estilo de codificação |
| [Regras de Teste](rules/RegrasdeTeste.md) | [RegrasdeTeste.md](rules/RegrasdeTeste.md) | Requisitos de teste (80% cobertura) |
| [Regras de Segurança](rules/RegrasdeSegurança.md) | [RegrasdeSegurança.md](rules/RegrasdeSegurança.md) | Lista de verificação de segurança |

---

## 7. Índice de Hooks

Total de **4 documentos**:

| Documento | Arquivo | Descrição |
|----------|------|-------------|
| [Tipos de Hook](hooks/TiposdeHook.md) | [TiposdeHook.md](hooks/TiposdeHook.md) | Tipos PreToolUse, PostToolUse, Stop |
| [Hooks Embutidos](hooks/HooksEmbutidos.md) | [HooksEmbutidos.md](hooks/HooksEmbutidos.md) | Lista de hooks embutidos e uso |
| [Desenvolvimento Personalizado](hooks/DesenvolvimentoPersonalizado.md) | [DesenvolvimentoPersonalizado.md](hooks/DesenvolvimentoPersonalizado.md) | Guia de desenvolvimento de hooks personalizados |
| [Formato de Configuração](hooks/FormadeConfiguração.md) | [FormadeConfiguração.md](hooks/FormadeConfiguração.md) | Formato de configuração hooks.json |

---

## 8. Índice de MCP

Total de **6 configurações de servidor MCP**:

| Documento | Arquivo | Descrição |
|----------|------|-------------|
| [Formato de Configuração MCP](mcp/FormadeConfiguraçãoMCP.md) | [FormadeConfiguraçãoMCP.md](mcp/FormadeConfiguraçãoMCP.md) | Formato de arquivo de configuração MCP |
| [Servidores Embutidos](mcp/ServidoresEmbutidos.md) | [ServidoresEmbutidos.md](mcp/ServidoresEmbutidos.md) | Servidores MCP embutidos |
| [Desenvolvimento Personalizado](mcp/DesenvolvimentoPersonalizado.md) | [DesenvolvimentoPersonalizado.md](mcp/DesenvolvimentoPersonalizado.md) | Desenvolvimento de servidor MCP personalizado |

---

## 9. Índice de Scripts

Total de **54 scripts de utilitário**:

| Documento | Arquivo | Descrição |
|----------|------|-------------|
| [Scripts de Utilitário](scripts/ScriptsdeUtilitário.md) | [ScriptsdeUtilitário.md](scripts/ScriptsdeUtilitário.md) | ecc.js, install-apply.js etc. |
| [Biblioteca de Funções de Utilitário](scripts/BibliotecadeFunçõesdeUtilitário.md) | [BibliotecadeFunçõesdeUtilitário.md](scripts/BibliotecadeFunçõesdeUtilitário.md) | Biblioteca de funções compartilhadas |
| [Executor de Testes](scripts/ExecutordeTestes.md) | [ExecutordeTestes.md](scripts/ExecutordeTestes.md) | Uso do executor de testes |
| [Scripts de Build](scripts/ScriptsdeBuild.md) | [ScriptsdeBuild.md](scripts/ScriptsdeBuild.md) | Scripts de build |

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