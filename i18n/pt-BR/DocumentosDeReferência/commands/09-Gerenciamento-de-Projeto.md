# Comandos de Gerenciamento de Projeto

## Visão Geral

Comandos de gerenciamento de projeto são usados para configuração de projeto, infraestrutura e gerenciamento de toolchain.

## Lista de Comandos

### /projects

**Propósito**: Listar projetos conhecidos + estatísticas de instinct

**Descrição**: Lista todos os projetos conhecidos do usuário junto com suas estatísticas de instinct relacionadas.

---

### /harness-audit

**Propósito**: Auditar configuração do agent harness

**Descrição**: Verifica e audita a configuração do harness do Claude Code.

**Verifica**:
- Configuração de toolchain
- Configurações de agent
- Status do servidor MCP
- Configuração de regras

---

### /model-route

**Propósito**: Rotear tarefas para modelo apropriado

**Descrição**: Seleciona o modelo mais apropriado baseado no tipo de tarefa (Haiku/Sonnet/Opus).

**Guia de Seleção de Modelo**:
- **Haiku**: Tarefas leves, geração de código, invocações frequentes
- **Sonnet**: Trabalho principal de desenvolvimento, tarefas de codificação complexas
- **Opus**: Decisões de arquitetura, reasoning profundo, tarefas de pesquisa e análise

---

### /pm2

**Propósito**: Inicialização do gerenciador de processos PM2

**Descrição**: Inicializa configuração do gerenciador de processos PM2.

---

### /setup-pm

**Propósito**: Configurar gerenciador de pacotes

**Descrição**: Configura o gerenciador de pacotes usado pelo projeto.

**Gerenciadores de Pacotes Suportados**:
- npm
- pnpm
- yarn
- bun

**Pode ser configurado via variável de ambiente** `CLAUDE_PACKAGE_MANAGER`

---

### /project-init

**Propósito**: Inicialização de projeto

**Descrição**: Inicializa estrutura e configuração padrão de novo projeto.

---

## Comandos Relacionados

- `/projects` - Listar projetos
- `/harness-audit` - Auditar configuração
- `/model-route` - Rotear modelo