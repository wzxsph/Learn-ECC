# Comandos de Gerenciamento de Sessão

## Visão Geral

Comandos de gerenciamento de sessão são usados para salvar, restaurar e gerenciar estado de sessões do Claude Code. Estes comandos são essenciais para manter contexto em sessões longas, transferir trabalho entre sessões e rastrear progresso.

## Lista de Comandos

### /save-session

**Propósito**: Salvar estado de sessão - escrever contexto completo da sessão atual em arquivo datado

**Descrição**: Salvar todo contexto da sessão atual em arquivo datado no diretório `~/.claude/session-data/`, incluindo: o que foi construído, o que funcionou, o que falhou, trabalho restante, estado de arquivos, registros de decisões e blockers pendentes.

**Convenções de Nomenclatura de Arquivos de Sessão**:
- Formato: `YYYY-MM-DD-<short-id>-session.tmp`
- Caracteres válidos: letras `a-z`/`A-Z`, números `0-9`, hífens `-`, underscores `_`
- Comprimento mínimo: 1 caractere (não recomendado muito curto)
- Recomendado: 8+ caracteres de letras/ números/hífens em combinação para evitar conflitos no mesmo dia

**Quando Usar**:
- No fim do trabalho (antes de encerrar sessão)
- Quando próximo do limite de contexto (salvar primeiro, abrir nova sessão)
- Após resolver problema complexo (salvar padrões de sucesso)
- Quando precisar passar para sessão futura

**Fluxo de Trabalho**:
1. **Coletar Contexto** - Ler todos os arquivos modificados, revisar discussões
2. **Criar Diretório** - `mkdir -p ~/.claude/session-data`
3. **Escrever Arquivo** - Criar arquivo de sessão com timestamp
4. **Exibir ao Usuário** - Mostrar conteúdo e solicitar confirmação

**Formato de Arquivo de Sessão**:

```markdown
# Session: YYYY-MM-DD

**Started:** [tempo aproximado se conhecido]
**Last Updated:** [tempo atual]
**Project:** [nome do projeto ou caminho]
**Topic:** [resumo de uma linha do que sessão tratou]

---

## What We Are Building
[1-3 parágrafos descrevendo feature, correção de bug ou tarefa]

## What WORKED (with evidence)
- **[coisa que funciona]** — confirmado por: [evidência específica]

## What Did NOT Work (and why)
- **[abordagem tentada]** — falhou porque: [razão exata / mensagem de erro]

## What Has NOT Been Tried Yet
- [abordagem / ideia]

## Current State of Files
| File              | Status         | Notes                      |
| ----------------- | -------------- | -------------------------- |
| `path/to/file.ts` | PASS: Complete | [o que faz]                |

## Decisions Made
- **[decisão]** — razão: [por que isso foi escolhido sobre alternativas]

## Blockers & Open Questions
- [blocker / pergunta aberta]

## Exact Next Step
[The single most important thing to do when resuming]
```

**Princípios Chave**:
- Seção "What Did NOT Work" é a mais importante - sessões futuras vão tentar novamente métodos que falharam cegamente
- Cada sessão em arquivo independente - nunca anexar a sessões anteriores
- Arquivo para leitura por sessões futuras via `/resume-session`

**Melhores-Práticas**:
- Ao salvar no meio (não no fim), marcar claramente projetos em andamento
- Incluir mensagens de erro específicas e razões de falha ("threw X error because Y")
- Próximo passo específico deve ser preciso o suficiente para não precisar pensar sobre onde começar

---

### /resume-session

**Propósito**: Restaurar sessão salva

**Descrição**: Carregar e restaurar contexto de sessão previamente salva de `~/.claude/session-data/`.

**Cenários de Uso**:
- Ao iniciar nova sessão
- Quando precisar continuar trabalho anterior
- Quando precisar revisar processo de resolução de problemas anterior

---

### /sessions

**Propósito**: Navegar+buscar+gerenciar histórico de sessões

**Descrição**: Navegar, buscar e gerenciar todo histórico de sessões salvas.

**Funcionalidades**:
- Listar todas as sessões
- Buscar por data
- Gerenciar aliases de sessões
- Limpar sessões antigas

---

### /checkpoint

**Propósito**: Criar/verificar checkpoint de workflow

**Descrição**: Criar checkpoint em pontos-chave de workflow, garantindo que progresso seja rastreável.

**Cenários de Uso**:
- Quando etapas importantes forem concluídas
- Quando necessidade de verificar marcos importantes
- Em tarefas multi-fase

---

### /aside

**Propósito**: Responder perguntas laterais sem perder contexto

**Descrição**: Alternar para modo de diálogo lateral, processar questões secundárias e retornar perfeitamente à tarefa principal.

**Cenários de Uso**:
- Quando precisar fazer uma pergunta rápida
- Quando necessidade de consultar documentação
- Quando necessidade de executar tarefa auxiliar

---

## Comandos Relacionados

- `/save-session` - Salvar sessão atual
- `/resume-session` - Restaurar sessão
- `/sessions` - Gerenciar histórico de sessões