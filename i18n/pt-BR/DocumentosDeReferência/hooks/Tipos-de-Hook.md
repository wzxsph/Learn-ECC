# Tipos de Hook

## Visão Geral

ECC Sistema-Hooks é um mecanismo de automação orientado por eventos, disparado antes e depois da execução de ferramentas do Claude Code, usado para impor qualidade de código, descobrir erros cedo e automatizar verificações repetitivas.

```
Requisição do usuário → Claude seleciona ferramenta → PreToolUse hook executa → Ferramenta executa → PostToolUse hook executa
```

## Tipos de Hook Detalhados

### 1. Hook PreToolUse

#### Descrição do Tipo
Hooks PreToolUse executam **antes** da execução da ferramenta, podem bloquear (exit code 2) ou alertar (stderr output sem bloquear).

#### Momento de Disparo
- Antes de qualquer ferramenta (Edit, Write, Bash, Read etc.) executar
- Adequado para verificações pré-execução de todas as operações de ferramenta

#### Descrição de Parâmetros
```typescript
interface HookInput {
  tool_name: string;          // Nome da ferramenta: "Bash", "Edit", "Write" etc.
  tool_input: {
    command?: string;         // Bash: comando a executar
    file_path?: string;        // Edit/Write/Read: caminho do arquivo alvo
    old_string?: string;       // Edit: texto substituído
    new_string?: string;       // Edit: texto de substituição
    content?: string;          // Write: conteúdo do arquivo
  };
}
```

#### Códigos de Saída
| Código de Saída | Significado | Comportamento |
|---------|------|------|
| 0 | Sucesso | Continuar execução, sem output de aviso |
| 2 | Bloquear | Bloquear execução da ferramenta |
| Outro não-zero | Erro | Logar, mas não bloquear |

#### Exemplos de Uso

**Bloqueador de servidor de desenvolvimento (bloquear executar npm run dev fora do tmux):**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/dev-server-check.js"
  }],
  "description": "Bloquear execução de servidor de desenvolvimento fora do tmux"
}
```

**Aviso de arquivo de documentação (bloquear arquivos .md não padrão):**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/doc-file-warning.js"
  }],
  "description": "Alertar criação de arquivos de documentação não padrão"
}
```

#### Observações
- PreToolUse é hook **bloqueante**, deve executar rapidamente (<200ms)
- Não fazer chamadas de rede em hooks PreToolUse
- Usar bloqueio (exit 2) com cautela, apenas para verificações críticas de segurança

---

### 2. Hook PostToolUse

#### Descrição do Tipo
Hooks PostToolUse executam **depois** da execução da ferramenta, podem analisar output mas não podem bloquear execução da ferramenta.

#### Momento de Disparo
- Após qualquer ferramenta completar
- Pode acessar resultado da execução da ferramenta

#### Descrição de Parâmetros
```typescript
interface HookInput {
  tool_name: string;          // Nome da ferramenta
  tool_input: {               // Igual ao PreToolUse
    command?: string;
    file_path?: string;
    old_string?: string;
    new_string?: string;
    content?: string;
  };
  tool_output?: {             // Apenas disponível em PostToolUse
    output?: string;          // Output do comando/ferramenta
  };
}
```

#### Códigos de Saída
| Código de Saída | Significado |
|---------|------|
| 0 | Sucesso, continuar execução |
| Não-zero | Erro, logar mas não bloquear execução da ferramenta |

#### Exemplos de Uso

**Log de PR (registrar URL do PR após gh pr create):**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/pr-logger.js",
    "async": true,
    "timeout": 30
  }],
  "description": "Registrar URL e comandos de revisão após criação de PR"
}
```

**Verificação de quality gate (executar verificação de qualidade após edição):**
```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/quality-gate.js",
    "async": true,
    "timeout": 30
  }],
  "description": "Executar verificação de quality gate após edição de arquivo"
}
```

#### Observações
- Hooks PostToolUse **não podem bloquear** execução da ferramenta
- Podem ser configurados como assíncronos (async: true) para evitar bloqueio
- Hooks assíncronos devem completar em 30 segundos

---

### 3. Hook Stop

#### Descrição do Tipo
Hooks Stop executam após cada resposta do Claude, usados para processamento em lote, verificações finais e persistência de estado de sessão.

#### Momento de Disparo
- Após cada requisição de usuário ser processada e Claude gerar resposta
- Ocorre na última resposta da sessão

#### Descrição de Parâmetros
Hooks Stop recebem o mesmo input que PostToolUse, incluindo histórico completo de chamadas de ferramenta.

#### Exemplos de Uso

**Verificação de log de console (verificar console.log nos arquivos modificados após resposta):**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/check-console-log.js"
  }],
  "description": "Verificar se há console.log nos arquivos modificados"
}
```

**Formatação e verificação de tipo em lote:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/stop-format-typecheck.js",
    "timeout": 300
  }],
  "description": "Formatar e verificar tipos de todos os arquivos JS/TS editados na resposta"
}
```

**Persistência de estado de sessão:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/session-end.js",
    "async": true,
    "timeout": 10
  }],
  "description": "Salvar estado da sessão"
}
```

#### Observações
- Hooks Stop executam após cada resposta, adequados para operações em lote
- Podem usar modo async para operações não bloqueantes
- Formatação/verificação de tipo são adequadas para executar em Stop em lote

---

### 4. Hook SessionStart

#### Descrição do Tipo
Hooks SessionStart disparam no início do ciclo de vida da sessão, usados para carregar contexto prévio e detectar estado do projeto.

#### Momento de Disparo
- Quando nova sessão começa
- Quando Claude Code inicia ou nova sessão é criada

#### Exemplo de Uso

**Carregar contexto prévio e detectar gerenciador de pacotes:**
```json
{
  "event": "SessionStart",
  "id": "session:start",
  "script": "scripts/hooks/session-start-bootstrap.js",
  "purpose": "Carregar contexto prévio com limites e detectar estado do projeto no início da sessão",
  "blocking": false
}
```

#### Observações
- Hooks de lifecycle, devem completar rapidamente (<30 segundos)
- Podem controlar tamanho de contexto adicional via variável de ambiente `ECC_SESSION_START_MAX_CHARS`
- Podem desabilitar contexto adicional completamente com `ECC_SESSION_START_CONTEXT=off`

---

### 5. Hook SessionEnd

#### Descrição do Tipo
Hooks SessionEnd disparam no fim da sessão, usados para persistir resumo de fim de sessão e limpeza.

#### Momento de Disparo
- Quando sessão termina
- Quando metadados de transcript da sessão estão disponíveis

#### Exemplo de Uso

**Marcador de fim de sessão:**
```json
{
  "event": "SessionEnd",
  "id": "session:end",
  "script": "scripts/hooks/session-end.js",
  "purpose": "Persistir resumo de fim de sessão quando metadados de transcript estão disponíveis",
  "blocking": false
}
```

#### Observações
- Principalmente execução assíncrona, não deve bloquear fim de sessão
- Marcadores de lifecycle para coleta de métricas e limpeza

---

### 6. Hook PreCompact

#### Descrição do Tipo
Hooks PreCompact disparam antes da compactação de contexto, usados para salvar estado.

#### Momento de Disparo
- Antes da compactação de contexto
- Quando janela de contexto está quase cheia

#### Exemplo de Uso

**Salvar estado antes de compactação de contexto:**
```json
{
  "event": "PreCompact",
  "id": "pre:compact",
  "script": "scripts/hooks/pre-compact.js",
  "purpose": "Persistir estado da sessão antes da compactação de contexto",
  "blocking": false
}
```

#### Observações
- Adequado para salvar estado de tarefas longas
- Não bloqueia, não afeta fluxo de compactação

---

### 7. Hook PostToolUseFailure

#### Descrição do Tipo
Hooks PostToolUseFailure disparam após falha de execução de ferramenta.

#### Momento de Disparo
- Após qualquer chamada de ferramenta falhar

#### Exemplo de Uso

**Verificação de saúde MCP (rastrear chamadas MCP falhadas):**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/mcp-health-check.js"
  }],
  "description": "Rastrear chamadas de ferramenta MCP falhadas, marcar servidores não saudáveis e tentar reconectar"
}
```

#### Observações
- Pode ser usado para lógica de retry ou rastreamento de saúde
- Não muda estado de falha da ferramenta

---

## Tabela Comparativa de Tipos de Hook

| Tipo | Momento de Disparo | Pode Bloquear | Pode Ser Assíncrono | Uso Típico |
|------|----------|----------|----------|----------|
| PreToolUse | Antes da execução da ferramenta | Sim (exit 2) | Opcional (síncrono) | Verificações de qualidade, validação de segurança |
| PostToolUse | Após execução da ferramenta | Não | Opcional | Log, análise |
| Stop | Após cada resposta | Não | Opcional | Formatação em lote, resumo de sessão |
| SessionStart | Início da sessão | Não | Não | Carregar contexto, detectar projeto |
| SessionEnd | Fim da sessão | Não | Não | Persistir resumo, limpeza |
| PreCompact | Antes de compactação | Não | Não | Salvar estado |
| PostToolUseFailure | Após falha de ferramenta | Não | Não | Verificação de saúde, retry |

---

## Variáveis de Ambiente para Controle

Podem controlar comportamento de hooks através de variáveis de ambiente:

```bash
# minimal | standard | strict (default: standard)
export ECC_HOOK_PROFILE=standard

# Desabilitar hooks específicos (separados por vírgula)
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# Desabilitar GateGuard
export ECC_GATEGUARD=off

# Controlar tamanho de contexto adicional de SessionStart (default: 8000 caracteres)
export ECC_SESSION_START_MAX_CHARS=4000

# Desabilitar completamente contexto adicional de SessionStart
export ECC_SESSION_START_CONTEXT=off
```