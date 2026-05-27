# Formato de Configuração de Hooks

## Visão Geral da Estrutura hooks.json

O arquivo de configuração do sistema ECC Hooks usa formato JSON com a seguinte estrutura principal:

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "hooks": {
    "PreToolUse": [...],
    "PostToolUse": [...],
    "Stop": [...],
    "SessionStart": [...],
    "SessionEnd": [...],
    "PreCompact": [...],
    "PostToolUseFailure": [...]
  }
}
```

## Estrutura de Topo

| Campo | Tipo | Descrição |
|------|------|----------|
| `$schema` | string | URL JSON Schema, para validação de configuração |
| `hooks` | object | Objeto contendo todos os tipos de hook |

---

## Estrutura de Array de Hooks

Cada tipo de evento (PreToolUse, PostToolUse etc.) contém um array de hooks, cada objeto de hook tem a seguinte estrutura:

```json
{
  "matcher": "Bash|Edit|Write",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/my-hook.js",
      "async": false,
      "timeout": 30
    }
  ],
  "description": "Descrição do hook",
  "id": "unique-hook-id"
}
```

### Campo matcher

#### Descrição do Tipo
O campo matcher especifica qual tipo de ferramenta dispara o hook, suporta ferramenta única ou múltiplas ferramentas (separadas por `|`).

#### Valores Disponíveis
| Valor matcher | Ferramentas correspondentes |
|---------------|-----------------|
| `*` | Todas as ferramentas |
| `Bash` | Ferramenta Bash |
| `Edit` | Ferramenta Edit |
| `Write` | Ferramenta Write |
| `Read` | Ferramenta Read |
| `MultiEdit` | Ferramenta MultiEdit |
| `Bash\|Edit\|Write` | Bash, Edit ou Write |
| `Edit\|Write\|MultiEdit` | Edit, Write ou MultiEdit |

#### Exemplo de Uso
```json
// Combinar todas as ferramentas
"matcher": "*"

// Apenas ferramenta Bash
"matcher": "Bash"

// Combinar múltiplas ferramentas
"matcher": "Edit|Write|MultiEdit"
```

---

## Estrutura de Array de hooks

O array hooks contém um ou mais objetos de comando de hook:

### Campo type

| Valor type | Descrição |
|-----------|-----------|
| `command` | Executar comando Node.js |

### Campo command

O comando a executar, geralmente caminho de script Node.js.

### Campo async (opcional)

```json
"async": true
```

- `true`: Execução assíncrona, não bloqueia execução de ferramenta
- `false` ou omitido: Execução síncrona, bloqueia execução de ferramenta

### Campo timeout (opcional)

```json
"timeout": 30
```

Tempo máximo de execução (segundos). Recomendado:
- Hooks síncronos: <200ms
- Hooks assíncronos: ≤30 segundos

---

## Exemplo de Configuração Completa

### Exemplo de Hook PreToolUse

```json
{
  "matcher": "Bash",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/pre-bash-dispatcher.js"
    }
  ],
  "description": "Dispatcher de pré-verificação Bash, para verificação de qualidade, tmux, push e GateGuard",
  "id": "pre:bash:dispatcher"
}
```

### Exemplo de Hook PostToolUse

```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/run-with-flags.js post:quality-gate scripts/hooks/quality-gate.js standard,strict",
      "async": true,
      "timeout": 30
    }
  ],
  "description": "Executar verificação de quality gate após edição de arquivo",
  "id": "post:quality-gate"
}
```

### Exemplo de Hook Stop

```json
{
  "matcher": "*",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/run-with-flags.js stop:session-end scripts/hooks/session-end.js minimal,standard,strict",
      "async": true,
      "timeout": 10
    }
  ],
  "description": "Salvar estado da sessão após cada resposta",
  "id": "stop:session-end"
}
```

### Exemplo de Hook SessionStart

```json
{
  "matcher": "*",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/session-start-bootstrap.js"
    }
  ],
  "description": "Carregar contexto prévio com limites e detectar gerenciador de pacotes da nova sessão",
  "id": "session:start"
}
```

---

## Configuração de Evento de Lifecycle

Para eventos de lifecycle como SessionStart, SessionEnd, PreCompact, usar estrutura de configuração diferente:

```json
{
  "description": "Definição de hook de lifecycle para persistência de memória",
  "events": [
    {
      "event": "SessionStart",
      "id": "session:start",
      "script": "scripts/hooks/session-start-bootstrap.js",
      "purpose": "Carregar contexto prévio com limites e detectar estado do projeto no início da sessão",
      "blocking": false
    },
    {
      "event": "PreCompact",
      "id": "pre:compact",
      "script": "scripts/hooks/pre-compact.js",
      "purpose": "Persistir estado da sessão antes da compactação de contexto",
      "blocking": false
    }
  ]
}
```

### Campos de Evento de Lifecycle

| Campo | Descrição |
|------|----------|
| `event` | Tipo de evento (SessionStart, PreCompact, SessionEnd etc.) |
| `id` | Identificador único |
| `script` | Caminho do script |
| `purpose` | Descrição de propósito |
| `blocking` | Se bloqueia (geralmente false) |

---

## Desabilitar Hooks

### Através de Edição de Configuração

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [],
        "description": "Override: permitir toda criação de arquivos .md"
      }
    ]
  }
}
```

### Através de Variável de Ambiente

```bash
# Desabilitar hooks específicos (separados por vírgula)
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# Desabilitar GateGuard
export ECC_GATEGUARD=off
```

---

## Convenções de Nomenclatura de ID de Hook

ECC usa convenção de nomenclatura separada por dois pontos:

| Prefixo | Propósito | Exemplo |
|---------|-----------|---------|
| `pre:` | Hooks PreToolUse | `pre:bash:dispatcher` |
| `post:` | Hooks PostToolUse | `post:quality-gate` |
| `stop:` | Hooks Stop | `stop:session-end` |
| `session:` | Hooks de lifecycle de sessão | `session:start` |

---

## Wrapper run-with-flags.js

ECC usa wrapper `run-with-flags.js` para executar hooks, suportando gating de configuração em runtime:

```
node scripts/hooks/run-with-flags.js <hook-id> <script-path> <profiles>
```

### Descrição de Parâmetros
| Parâmetro | Descrição |
|-----------|-----------||
| `hook-id` | Identificador único do hook |
| `script-path` | Caminho real do script a executar |
| `profiles` | Lista separada por vírgula de arquivos de configuração (minimal, standard, strict) |

### Como Funciona
1. Ler variável de ambiente `ECC_HOOK_PROFILE` (default: standard)
2. Verificar se hook ID está em `ECC_DISABLED_HOOKS`
3. Se permitido, executar script

---

## Tratamento de Caminhos Cross-Platform

Hooks ECC usam scripts Node.js para comportamento cross-platform:

```javascript
const path = require('path');
const homedir = require('os').homedir();

// Resolução de caminho cross-platform
const configDir = path.join(homedir(), '.claude');
const hookScript = path.join(configDir, 'scripts', 'hooks', 'my-hook.js');
```

---

## Validação de Configuração

Recomenda-se usar JSON Schema para validar configuração:
```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json"
}
```