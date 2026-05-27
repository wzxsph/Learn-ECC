# Hooks Embutidos

## Visão Geral

O plugin ECC fornece abundantes implementações de hooks embutidos, localizados no diretório `ECC/scripts/hooks/`.

## Hooks PreToolUse Embutidos

### pre:bash:dispatcher

**Tipo**: PreToolUse
**matcher**: Bash
**Descrição**: Dispatcher de pré-verificação Bash, integra verificação de qualidade, lembrete de tmux, lembrete de git push e verificação de GateGuard

**Funcionalidades**:
- Bloqueador de servidor de desenvolvimento - Bloquear execução de `npm run dev` etc. fora do tmux
- Remento de tmux - Sugerir uso de tmux para comandos de longa execução (npm test, cargo build, docker)
- Remento de git push - Lembrar revisar mudanças antes de `git push`
- Verificação de qualidade pré-commit - Executar verificação de qualidade: lint de arquivos staged, validar formato de mensagem de commit, detectar console.log/debugger/chaves

**Códigos de Saída**:
- 0: Alerta mas continua
- 2: Bloquear execução

---

### pre:write:doc-file-warning

**Tipo**: PreToolUse
**matcher**: Write
**Descrição**: Alertar criação de arquivos de documentação não padrão

**Funcionalidades**:
- Permitir arquivos padrão: README, CLAUDE, CONTRIBUTING, CHANGELOG, LICENSE, SKILL
- Permitir diretórios: docs/, skills/
- Alertar outros arquivos .md/.txt
- Tratamento de caminhos cross-platform

**Código de Saída**: 0 (apenas alerta)

---

### pre:edit-write:suggest-compact

**Tipo**: PreToolUse
**matcher**: Edit|Write
**Descrição**: Sugerir compactação manual `/compact` em intervalos lógicos (aproximadamente cada 50 chamadas de ferramenta)

**Funcionalidades**:
- Rastrear contagem de chamadas de ferramenta
- Alertar compactação de contexto em intervalos apropriados
- Não bloqueante, apenas sugere

**Código de Saída**: 0 (sugestão)

---

### pre:observe:continuous-learning

**Tipo**: PreToolUse
**matcher**: *
**Descrição**: Registrar observações de intenção de ferramenta para suportar sinais de aprendizado contínuo

**Funcionalidades**:
- Capturar observações de uso de ferramenta
- Usado para extração de padrão e análise
- Execução assíncrona, não bloqueia

**Código de Saída**: 0

---

### pre:governance-capture

**Tipo**: PreToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**Descrição**: Capturar eventos de governança (chaves, violações de política, solicitações de aprovação)

**Funcionalidades**:
- Detectar vazamento de secrets/chaves
- Capturar violações de política
- Registrar solicitações de aprovação

**Condição de Habilitar**: `ECC_GOVERNANCE_CAPTURE=1`

**Código de Saída**: 0

---

### pre:config-protection

**Tipo**: PreToolUse
**matcher**: Write|Edit|MultiEdit
**Descrição**: Bloquear modificação de arquivos de configuração de linter/formatter

**Funcionalidades**:
- Bloquear modificação de .eslintrc, .prettierrc etc.
- Orientar correção de código ao invés de enfraquecer configuração

**Código de Saída**: 0

---

### pre:mcp-health-check

**Tipo**: PreToolUse
**matcher**: *
**Descrição**: Verificar estado de saúde do servidor MCP antes da execução de ferramenta MCP

**Funcionalidades**:
- Verificar estado de servidores MCP
- Bloquear chamadas para servidores MCP não saudáveis

**Código de Saída**: 0

---

### pre:edit-write:gateguard-fact-force

**Tipo**: PreToolUse
**matcher**: Edit|Write|MultiEdit
**Descrição**: GateGuard de fato forçado: bloquear primeira edição/escrita de cada arquivo, requer investigação antes de permitir

**Funcionalidades**:
- Bloquear primeira edição
- Requerir confirmação: imports, schema de dados, instruções do usuário
- Garantir razão suficiente antes de modificar

**Código de Saída**: 0

---

## Hooks PostToolUse Embutidos

### post:bash:dispatcher

**Tipo**: PostToolUse
**matcher**: Bash
**Descrição**: Dispatcher de pós-verificação Bash, para logging, notificações de PR e build

**Funcionalidades**:
- Log de PR - Registrar URL do PR e comandos de revisão após `gh pr create`
- Análise de build - Análise em background após comandos de build (assíncrono, não bloqueante)

**Código de Saída**: 0

---

### post:quality-gate

**Tipo**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Descrição**: Executar verificação rápida de qualidade após edição de arquivo

**Funcionalidades**:
- Verificação de qualidade de código
- Execução assíncrona
- Não bloqueia operação de edição

**Código de Saída**: 0

---

### post:edit:design-quality-check

**Tipo**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Descrição**: Alertar desvio de UI de edição frontend para aparência genérica de template

**Funcionalidades**:
- Detectar qualidade de design de UI
- Prevenir aparência muito genérica de template

**Código de Saída**: 0

---

### post:edit:accumulator

**Tipo**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Descrição**: Registrar caminhos de arquivos JS/TS editados, para formatação e verificação de tipo em lote no Stop

**Funcionalidades**:
- Acumular lista de arquivos editados
- Para uso por stop:format-typecheck

**Código de Saída**: 0

---

### post:edit:console-warn

**Tipo**: PostToolUse
**matcher**: Edit
**Descrição**: Alertar instruções console.log após edição

**Funcionalidades**:
- Detectar console.log no código
- Alertar desenvolvedores para limpar código de debug

**Código de Saída**: 0

---

### post:governance-capture

**Tipo**: PostToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**Descrição**: Capturar eventos de governança do output de ferramenta

**Funcionalidades**:
- Analisar output de ferramenta
- Detectar problemas potenciais

**Condição de Habilitar**: `ECC_GOVERNANCE_CAPTURE=1`

**Código de Saída**: 0

---

### post:session-activity-tracker

**Tipo**: PostToolUse
**matcher**: *
**Descrição**: Registrar estatísticas de chamadas de ferramenta e atividade de arquivo por sessão

**Funcionalidades**:
- Rastrear uso de ferramenta
- Registro de atividade de arquivo
- Para métricas ECC2

**Código de Saída**: 0

---

### post:observe:continuous-learning

**Tipo**: PostToolUse
**matcher**: *
**Descrição**: Registrar resultados de execução de ferramenta para suportar sinais de aprendizado contínuo

**Funcionalidades**:
- Capturar resultados de execução de ferramenta
- Suportar análise de padrões

**Código de Saída**: 0

---

### post:ecc-metrics-bridge

**Tipo**: PostToolUse
**matcher**: *
**Descrição**: Manter agregação de métricas de sessão em execução

**Funcionalidades**:
- Rastrear métricas de token e custo
- Para barra de status e monitor de contexto

**Código de Saída**: 0

---

### post:ecc-context-monitor

**Tipo**: PostToolUse
**matcher**: *
**Descrição**: Injetar avisos de agente quando contexto está se esgotando, alto custo, scope creep ou loop de ferramenta

**Funcionalidades**:
- Monitoramento de uso de contexto
- Avisos de custo
- Detecção de scope creep
- Detecção de loop de ferramenta

**Código de Saída**: 0

---

### post:mcp-health-check

**Tipo**: PostToolUseFailure
**matcher**: *
**Descrição**: Rastrear chamadas de ferramenta MCP falhadas, marcar servidores não saudáveis e tentar reconectar

**Funcionalidades**:
- Rastreamento de falhas
- Marcação de estado de saúde
- Tentativa automática de reconectar

**Código de Saída**: 0

---

## Hooks Stop Embutidos

### stop:format-typecheck

**Tipo**: Stop
**matcher**: *
**Descrição**: Formatar (Biome/Prettier) e verificar tipos (tsc) em lote de todos os arquivos JS/TS editados na resposta atual

**Funcionalidades**:
- Formatação Prettier/Biome
- Verificação de tipo TypeScript
- Executar no Stop uma vez, não após cada edição

**Código de Saída**: 0

**Timeout**: 300 segundos

---

### stop:check-console-log

**Tipo**: Stop
**matcher**: *
**Descrição**: Verificar console.log nos arquivos modificados após cada resposta

**Funcionalidades**:
- Escanear arquivos modificados
- Reportar uso de console.log

**Código de Saída**: 0

---

### stop:session-end

**Tipo**: Stop
**matcher**: *
**Descrição**: Persistir estado da sessão após cada resposta (Stop carrega transcript_path)

**Funcionalidades**:
- Salvamento de estado de sessão
- Persistência de contexto

**Código de Saída**: 0

---

### stop:evaluate-session

**Tipo**: Stop
**matcher**: *
**Descrição**: Avaliar sessão para extrair padrões aprendíveis

**Funcionalidades**:
- Reconhecimento de padrões
- Suporte a aprendizado contínuo
- Execução assíncrona

**Código de Saída**: 0

---

### stop:cost-tracker

**Tipo**: Stop
**matcher**: *
**Descrição**: Rastrear métricas de token e custo por sessão

**Funcionalidades**:
- Estatísticas de uso de token
- Estimativa de custo
- Telemetria leve de custo de execução

**Código de Saída**: 0

---

### stop:desktop-notify

**Tipo**: Stop
**matcher**: *
**Descrição**: Enviar notificação de desktop macOS/WSL quando Claude responde, contendo resumo de tarefa

**Funcionalidades**:
- Notificação de desktop
- Exibição de resumo de tarefa

**Código de Saída**: 0

---

## Hooks SessionStart Embutidos

### session:start

**Tipo**: SessionStart
**matcher**: *
**Descrição**: Carregar contexto prévio com limites e detectar gerenciador de pacotes da nova sessão

**Funcionalidades**:
- Carregar contexto prévio com limites
- Detecção de estado do projeto
- Detecção de gerenciador de pacotes (npm/pnpm/yarn/bun)

**Variáveis de Ambiente**:
- `ECC_SESSION_START_MAX_CHARS`: Controlar tamanho de contexto adicional (default 8000 caracteres)
- `ECC_SESSION_START_CONTEXT=off`: Desabilitar completamente contexto adicional

**Código de Saída**: 0

---

## Hooks PreCompact Embutidos

### pre:compact

**Tipo**: PreCompact
**matcher**: *
**Descrição**: Salvar estado antes da compactação de contexto

**Funcionalidades**:
- Persistência de estado de sessão
- Preparar contexto para compactação

**Código de Saída**: 0

---

## Hooks SessionEnd Embutidos

### session:end:marker

**Tipo**: SessionEnd
**matcher**: *
**Descrição**: Marcador de lifecycle de fim de sessão

**Funcionalidades**:
- Marcador de evento de lifecycle
- Log de limpeza

**Código de Saída**: 0

---

## Referência Rápida de Hooks Embutidos

### Hooks PreToolUse

| ID | Matcher | Funcionalidade | Bloqueante |
|----|---------|------|------|
| pre:bash:dispatcher | Bash | Verificações de qualidade/tmux/push/GateGuard | Pode bloquear |
| pre:write:doc-file-warning | Write | Alerta de arquivo de documentação | Não |
| pre:edit-write:suggest-compact | Edit\|Write | Sugerir compactação | Não |
| pre:observe:continuous-learning | * | Aprendizado contínuo | Não |
| pre:governance-capture | Bash\|Write\|Edit\|MultiEdit | Captura de governança | Não |
| pre:config-protection | Write\|Edit\|MultiEdit | Proteção de configuração | Não |
| pre:mcp-health-check | * | Verificação de saúde MCP | Pode bloquear |
| pre:edit-write:gateguard-fact-force | Edit\|Write\|MultiEdit | GateGuard de primeira edição | Pode bloquear |

### Hooks PostToolUse

| ID | Matcher | Funcionalidade | Assíncrono |
|----|---------|------|-----|
| post:bash:dispatcher | Bash | Log de PR/notificações de build | Sim |
| post:quality-gate | Edit\|Write\|MultiEdit | Verificação de quality gate | Sim |
| post:edit:design-quality-check | Edit\|Write\|MultiEdit | Verificação de qualidade de design | Não |
| post:edit:accumulator | Edit\|Write\|MultiEdit | Acumulador de edição | Não |
| post:edit:console-warn | Edit | Alerta de console.log | Não |
| post:governance-capture | Bash\|Write\|Edit\|MultiEdit | Captura de governança | Não |
| post:session-activity-tracker | * | Rastreamento de atividade de sessão | Não |
| post:observe:continuous-learning | * | Aprendizado contínuo | Sim |
| post:ecc-metrics-bridge | * | Ponte de métricas | Não |
| post:ecc-context-monitor | * | Monitor de contexto | Não |
| post:mcp-health-check | * (PostToolUseFailure) | Verificação de saúde MCP | Não |

### Hooks Stop

| ID | Matcher | Funcionalidade | Timeout |
|----|---------|------|------|
| stop:format-typecheck | * | Formatação e verificação de tipo em lote | 300s |
| stop:check-console-log | * | Verificação de console.log | 30s |
| stop:session-end | * | Persistência de estado de sessão | 10s |
| stop:evaluate-session | * | Avaliação de sessão | 10s |
| stop:cost-tracker | * | Rastreamento de custo | 10s |
| stop:desktop-notify | * | Notificação de desktop | 10s |

### Hooks de Lifecycle

| ID | Evento | Funcionalidade |
|----|-------|------|
| session:start | SessionStart | Carregar contexto e detectar gerenciador de pacotes |
| pre:compact | PreCompact | Salvar estado antes de compactação |
| session:end:marker | SessionEnd | Marcador de fim de sessão |