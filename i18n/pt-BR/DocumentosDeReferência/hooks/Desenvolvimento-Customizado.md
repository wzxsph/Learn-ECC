# Desenvolvimento Customizado de Hooks

## Visão Geral

Este guia apresenta como desenvolver hooks customizados para o Sistema-Hooks do ECC, incluindo estrutura básica, melhores práticas e requisito de exit 0.

## Estrutura Básica

### Modelo Mínimo de Hook

```javascript
// my-custom-hook.js
let data = '';
process.stdin.on('data', chunk => data += chunk);
process.stdin.on('end', () => {
  const input = JSON.parse(data);

  // Acessar informações da ferramenta
  const toolName = input.tool_name;        // "Edit", "Bash", "Write" etc.
  const toolInput = input.tool_input;      // Parâmetros específicos da ferramenta
  const toolOutput = input.tool_output;    // Apenas disponível em PostToolUse

  // Aviso (não bloqueante): escrever para stderr
  console.error('[Hook] Mensagem de aviso');

  // Bloquear (apenas PreToolUse): exit code 2
  // process.exit(2);

  // Sempre outputar dados originais para stdout
  console.log(data);
});
```

---

## Requisitos de Exit Code

### Regra-Chave: exit 0 em erros não-críticos

**Importante**: Todos os hooks devem fazer exit 0 em erros não-críticos, para evitar bloqueio inesperado da execução da ferramenta.

| Exit Code | Significado | Cenário de Uso |
|-----------|-------------|----------------|
| 0 | Sucesso | Continuar execução, ou aviso não-crítico |
| 2 | Bloquear | Apenas para bloqueio crítico em PreToolUse |
| Outros não-zero | Erro | Apenas logar, **não usar** |

### Por que exit codes não-zero são ruins?

Se o hook sai com exit code não-zero (exceto 2), o Claude Code marcará toda a chamada de ferramenta como falha, o que pode causar:
- Execução de ferramenta interrompida inesperadamente
- Experiência do usuário prejudicada
- Problemas difíceis de debug

### Tratamento Correto de Erros

```javascript
// Exemplo errado - não fazer isso
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // lógica de processamento
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    process.exit(1);  // Erro: vai bloquear a ferramenta
  }
  console.log(data);
});

// Exemplo correto - fazer isso
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // lógica de processamento
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    // Erro não-crítico, exit 0 para continuar
    console.log(data);
    return;
  }
  console.log(data);
});
```

---

## Modos de Input de Hook

### Interface HookInput

```typescript
interface HookInput {
  tool_name: string;          // Nome da ferramenta
  tool_input: {               // Parâmetros de input da ferramenta
    command?: string;         // Bash: comando
    file_path?: string;        // Edit/Write/Read: caminho do arquivo
    old_string?: string;       // Edit: texto substituído
    new_string?: string;       // Edit: texto de substituição
    content?: string;          // Write: conteúdo do arquivo
  };
  tool_output?: {             // Apenas em PostToolUse
    output?: string;          // Output do comando/ferramenta
  };
}
```

### Acessar Informações da Ferramenta

```javascript
process.stdin.on('end', () => {
  const input = JSON.parse(data);

  // Obter nome da ferramenta
  console.log('Tool:', input.tool_name);

  // Processar baseado no tipo de ferramenta
  if (input.tool_name === 'Bash') {
    console.log('Command:', input.tool_input.command);
  }

  if (input.tool_name === 'Edit' || input.tool_name === 'Write') {
    console.log('File:', input.tool_input.file_path);
  }

  // PostToolUse pode acessar output
  if (input.tool_output) {
    console.log('Output:', input.tool_output.output);
  }
});
```

---

## Receitas Comuns de Hook

### Aviso de Comentários TODO/FIXME

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const ns=i.tool_input?.new_string||'';if(/TODO|FIXME|HACK/.test(ns)){console.error('[Hook] New TODO/FIXME added - consider creating an issue')}console.log(d)})\""
  }],
  "description": "Avisar ao adicionar comentários TODO/FIXME"
}
```

### Bloquear Criação de Arquivos Muito Grandes

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const c=i.tool_input?.content||'';const lines=c.split('\\n').length;if(lines>800){console.error('[Hook] BLOCKED: File exceeds 800 lines ('+lines+' lines)');console.error('[Hook] Split into smaller, focused modules');process.exit(2)}console.log(d)})\""
  }],
  "description": "Bloquear criação de arquivos com mais de 800 linhas"
}
```

### Auto-formatar Arquivos Python com ruff Após Editar

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/\\.py$/.test(p)){const{execFileSync}=require('child_process');try{execFileSync('ruff',['format',p],{stdio:'pipe'})}catch(e){}}console.log(d)})\""
  }],
  "description": "Auto-formatar arquivos Python com ruff após editar"
}
```

### Lembrar de Criar Testes ao Adicionar Novos Arquivos Fonte

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"const fs=require('fs');let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/src\\/.*\\.(ts|js)$/.test(p)&&!/\\.test\\.|\\.spec\\./.test(p)){const testPath=p.replace(/\\.(ts|js)$/,'.test.$1');if(!fs.existsSync(testPath)){console.error('[Hook] No test file found for: '+p);console.error('[Hook] Expected: '+testPath);console.error('[Hook] Consider writing tests first (/tdd)')}}console.log(d)})\""
  }],
  "description": "Lembrar de criar testes ao adicionar novos arquivos fonte"
}
```

---

## Hooks Assíncronos

Para hooks que não devem bloquear o fluxo principal (como análise em background):

```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node my-slow-hook.js",
    "async": true,
    "timeout": 30
  }]
}
```

### Observações sobre Hooks Assíncronos

- Hooks assíncronos executam em background
- Não podem bloquear execução de ferramenta
- Devem completar em 30 segundos
- Adequados para: logging, análise, telemetria

---

## Hooks de Bloqueio

Para situações que devem bloquear execução de ferramenta (PreToolUse apenas):

```javascript
// Em PreToolUse, bloquear
process.exit(2);  // Exit code 2 significa bloquear
```

### Quando Usar Bloqueio

- Verificações de segurança falhando (como detectar chave)
- Violações de política rígida
- Operações que podem causar perda de dados

### Quando NÃO Usar Bloqueio

- Problemas de estilo de código (usar aviso ao invés disso)
- Verificações não-críticas
- Sugestões

---

## Configuração em Runtime

### Usar run-with-flags.js

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"const p=require('path');const r=(()=>{var e=process.env.CLAUDE_PLUGIN_ROOT;if(e&&e.trim())return e.trim();var p=require('path'),f=require('fs'),h=require('os').homedir(),d=p.join(h,'.claude'),q=p.join('scripts','lib','utils.js');if(f.existsSync(p.join(d,q)))return d;for(var s of [['ecc'],['ecc@ecc'],['marketplaces','ecc'],['everything-claude-code'],['everything-claude-code@everything-claude-code'],['marketplaces','everything-claude-code']]){var l=p.join(d,'plugins',...s);if(f.existsSync(p.join(l,q)))return l}try{for(var g of ['ecc','everything-claude-code']){var b=p.join(d,'plugins','cache',g);for(var o of f.readdirSync(b,{withFileTypes:true})){if(!o.isDirectory())continue;for(var v of f.readdirSync(p.join(b,o.name),{withFileTypes:true})){if(!v.isDirectory())continue;var c=p.join(b,o.name,v.name);if(f.existsSync(p.join(c,q)))return c}}}}catch(x){}return d})();const s=p.join(r,'scripts/hooks/plugin-hook-bootstrap.js');process.env.CLAUDE_PLUGIN_ROOT=r;process.argv.splice(1,0,s);require(s)\" node scripts/hooks/run-with-flags.js my-hook scripts/hooks/my-hook.js standard,strict"
  }]
}
```

### Versão Simplificada

Usar caminho de script direto (se o root do plugin for conhecido):

```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/my-hook.js"
  }]
}
```

---

## Considerações Cross-Platform

### Tratamento de Caminhos

```javascript
const path = require('path');
const os = require('os');

// Usar path.join ao invés de caminhos hardcoded
const configPath = path.join(os.homedir(), '.claude', 'config.json');

// Detectar plataforma
if (process.platform === 'win32') {
  // Tratamento específico para Windows
}
```

### Output de Processo

```javascript
// Usar console.error para output de aviso (mostra para usuário)
console.error('[Hook] Warning: some issue detected');

// Usar console.log para output de dados originais (obrigatório)
console.log(data);

// Nunca usar console.log para mensagens de texto (vai quebrar fluxo de dados)
```

---

## Melhores Práticas de Performance

### Manter Rápido

- Hooks PreToolUse: <200ms
- Hooks PostToolUse síncronos: <1 segundo
- Hooks assíncronos: <30 segundos

### Evitar Operações Bloqueantes

```javascript
// Ruim: leitura de arquivo síncrona
const content = fs.readFileSync('large-file.txt', 'utf8');

// Bom: leitura assíncrona ou lazy loading
fs.readFile('large-file.txt', 'utf8', (err, content) => {
  // processar
});
```

### Cache de Resultados

```javascript
// Cache de operações caras
let cachedResult = null;
let cacheTime = 0;
const CACHE_TTL = 60000; // 1 minuto

function getCachedResult() {
  const now = Date.now();
  if (!cachedResult || now - cacheTime > CACHE_TTL) {
    cachedResult = expensiveOperation();
    cacheTime = now;
  }
  return cachedResult;
}
```

---

## Testando Hooks

### Teste Manual

```bash
# Testar hook PreToolUse
echo '{"tool_name":"Bash","tool_input":{"command":"echo test"}}' | node scripts/hooks/my-hook.js

# Testar hook PostToolUse
echo '{"tool_name":"Bash","tool_input":{"command":"echo test"},"tool_output":{"output":"test\n"}}' | node scripts/hooks/my-hook.js
```

### Formato de Output de Teste

```javascript
// Output stdout correto (JSON)
console.log(data);  // Dados de input originais

// Output stderr correto (avisos)
console.error('[Hook] Warning message');

// Maneira incorreta - não outputar outras coisas para stdout
console.log('Some message');  // Isso vai quebrar o fluxo de dados
```

---

## Debugging de Hooks

### Adicionar Output de Debug

```javascript
const DEBUG = process.env.DEBUG_HOOKS === '1';

process.stdin.on('end', () => {
  if (DEBUG) {
    console.error('[DEBUG] Received input:', data);
  }
  // lógica de processamento
  if (DEBUG) {
    console.error('[DEBUG] Sending output');
  }
  console.log(data);
});
```

### Problemas Comuns

| Problema | Causa | Solução |
|----------|-------|---------|
| Ferramenta bloqueada inesperadamente | Hook exit com código não-zero (exceto 2) | Usar exit 0 e avisos stderr |
| Travando | Hook não terminou | Garantir sempre outputar para stdout |
| Dados corrompidos | Output para stdout não é JSON | Apenas outputar data original |
| Problemas de performance | Hook muito lento | Otimizar ou tornar assíncrono |

---

## Integrando ao hooks.json

### Adicionar Novo Hook

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "node scripts/hooks/my-new-hook.js"
          }
        ],
        "description": "Descrição do meu novo hook",
        "id": "pre:custom:my-new-hook"
      }
    ]
  }
}
```

### Desabilitar Hooks Embutidos

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [],
        "description": "Desabilitar aviso de arquivos de documentação"
      }
    ]
  }
}
```

---

## Resumo de Melhores Práticas

1. **Sempre exit 0 em erros não-críticos** - Não usar códigos de saída não-zero (exceto 2)
2. **Usar console.error para avisos** - Não usar console.log
3. **Manter rápido** - PreToolUse <200ms
4. **Sempre outputar data original para stdout** - Não mudar fluxo de dados
5. **Usar processamento assíncrono para operações longas** - Definir async: true
6. **Tratamento de caminhos cross-platform** - Usar path.join
7. **Testar formato de output** - Garantir stdout é JSON original
8. **Documentar hook** - Adicionar description clara