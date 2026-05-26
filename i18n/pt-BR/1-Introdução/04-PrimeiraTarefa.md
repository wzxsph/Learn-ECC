# 04 - Primeira Tarefa

## Objetivos de Aprendizado

- Entender o processo completo de desenvolvimento
- Dominar o processo completo /ecc:plan → /ecc:code-review → /ecc:build-fix → Commit
- Ser capaz de desenvolver uma funcionalidade simples de forma independente

---

## Processo Completo de Desenvolvimento

ECC recomenda usar o seguinte processo de desenvolvimento:

```
Step 1: /ecc:plan        → Planejar tarefa
Step 2: /ecc:code-review → Revisar código
Step 3: /ecc:build-fix   → Reparar build
Step 4: Commitar código     → Git commit
```

---

## Step 1: Usar /ecc:plan para Planejar

### Entrada

Suponha que queremos implementar uma funcionalidade de calculadora simples:

```
/ecc:plan Implementar uma calculadora que suporta quatro operações: adição, subtração, multiplicação, divisão
```

### Saída Esperada

O comando /ecc:plan vai:

1. **Repetir requisitos** - "Implementar uma calculadora que suporta quatro operações básicas: adição, subtração, multiplicação, divisão..."
2. **Avaliar riscos** - "需要注意：除法除以零的情况..." (Precisa notar: situação de divisão por zero...)
3. **Plano faseado** -
   - Fase 1: Criar função núcleo da calculadora
   - Fase 2: Implementar quatro operações
   - Fase 3: Adicionar tratamento de erros
   - Fase 4: Escrever testes unitários
4. **Aguardar confirmação** - "Por favor confirme se o plano pode começar"

### Após Confirmação

Após a confirmação do usuário, ECC aguardará entrada do usuário para iniciar cada fase.

### Dicas

- Usar modo arquivo PRD para planejamento mais detalhado
- Funcionalidades complexas recomenda-se primeiro escrever PRD depois planejar

---

## Step 2: Usar /ecc:code-review para Revisar

### Entrada

Após escrever o código, executar:

```
/ecc:code-review
```

### Saída Esperada

/ecc:code-review vai:

1. Coletar arquivos alterados
2. Revisar 7 categorias:
   - Correção
   - Segurança de tipos
   - Conformidade com padrões
   - Segurança
   - Performance
   - Completude
   - Manutenibilidade
3. Gerar relatório de revisão, marcando níveis CRITICAL/HIGH/MEDIUM/LOW

### Exemplo de Relatório de Revisão

```
## Relatório de Revisão de Código

### CRITICAL
- [ ] Credenciais codificadas: src/auth.js linha 23

### HIGH
- [ ] Função muito longa: src/calculator.js linhas 45-78 (34 linhas)
- [ ] Tratamento de erros ausente: função divisão em src/calculator.js

### MEDIUM
- [ ] Convenção de nomenclatura não seguida: variável `tmp` deveria ser `temporary`

### Sugestões de Ação
1. Remover API Key codificada
2. Refatorar função divisão em calculator.js, adicionar verificação de zero
3. Dividir função muito longa
```

### Dicas

- Certifique-se de executar /ecc:code-review antes de commit
- Problemas CRITICAL e HIGH devem ser corrigidos

---

## Step 3: Usar /ecc:build-fix para Reparar

### Entrada

Se o build falhar, executar:

```
/ecc:build-fix
```

### Saída Esperada

/ecc:build-fix vai:

1. **Detectar sistema de build** - "Projeto TypeScript detectado"
2. **Executar build** - "Executando tsc --noEmit..."
3. **Analisar erros** - Agrupados por arquivo
4. **Reparar progressivamente** - Um erro por vez
5. **Verificar** - Reconstruir após cada reparo

### Exemplo de Saída

```
[build-fix] Projeto TypeScript detectado
[build-fix] Executando tsc --noEmit...
[build-fix] 3 erros encontrados:

Erro 1: src/calculator.ts:15 - Tipo 'string' não pode ser atribuído a tipo 'number'
Erro 2: src/calculator.ts:28 - Propriedade 'result' não existe
Erro 3: src/calculator.ts:42 - Parâmetro 'b' nunca usado

[build-fix] Começando a reparar erro 1...
[build-fix] Reparo concluído, verificando...
[build-fix] Build bem sucedido!
```

### Dicas

- Reparo de build é incremental, repare um erro por vez
- Pode parar a qualquer momento com Ctrl+C

---

## Step 4: Commitar Código

### Processo de Commit

1. Garantir que todos os testes passam
2. Garantir que build succeeds
3. Garantir que problemas de revisão de código foram corrigidos

### Padrão de Commit Git

ECC usa formato conventional commits:

```
<type>: <description>

[optional body]
```

**Tipos de Type**:
- `feat` - Nova funcionalidade
- `fix` - Correção de bug
- `refactor` - Refatoração
- `docs` - Documentação
- `test` - Teste
- `chore` - Manutenção rotineira
- `perf` - Otimização de performance
- `ci` - Configuração de CI

### Exemplo de Commit

```
feat: Implementar quatro operações básicas da calculadora

- Adicionar função de adição
- Adicionar função de subtração
- Adicionar função de multiplicação
- Adicionar função de divisão (com verificação de zero)

Resolve: Issue #123
```

### Dicas

- Usar `/ecc:verify quick` para verificação rápida antes de commit
- Funcionalidades grandes em múltiplos commits, pequenas em um commit

---

## Exemplo Completo

### Cenário

Implementar uma função utilitária de string simples: `truncate(str, maxLength)`, que trunca string para comprimento especificado.

### Step 1: Planear

```
/ecc:plan Implementar função truncate, que trunca string para comprimento especificado
```

Saída:
```
## Repetição de Requisitos
Implementar função truncate(str, maxLength), parte além de maxLength é substituída por ...

## Avaliação de Riscos
- Tratamento quando maxLength menor que 3
- Tratamento quando str é vazio ou null
- Tratamento de caracteres Unicode

## Plano Faseado
1. Criar src/string-utils.ts
2. Implementar função truncate
3. Adicionar testes de casos extremos
4. Adicionar documentação JSDoc

Por favor confirme se pode começar.
```

### Step 2: Implementar

(Após confirmação do usuário, ECC auxilia na implementação do código)

### Step 3: Revisar

```
/ecc:code-review
```

Saída:
```
## Relatório de Revisão de Código
Status: APPROVED

### Resultados da Revisão
- Correção: Aprovado
- Segurança de tipos: Aprovado
- Conformidade com padrões: Aprovado
- Segurança: Aprovado
- Performance: Aprovado
- Completude: Recomenda adicionar mais testes de casos extremos
- Manutenibilidade: Aprovado

### Recomendações
- Adicionar teste de string vazia
- Adicionar teste de valor null
```

### Step 4: Verificação de Build

```
/ecc:build-fix
```

```
[build-fix] Projeto TypeScript detectado
[build-fix] Executando tsc --noEmit...
[build-fix] Build bem sucedido!
```

### Step 5: Commit

```
feat: Implementar função truncate de truncamento de string

- Suporta comprimento máximo especificado
- Parte excedente substituída por ...
- Trata strings vazias e valores null
```

---


## Resumo

1. **Processo Completo**: /ecc:plan → Implementar → /ecc:code-review → /ecc:build-fix → Commit
2. **/ecc:plan** 用于规划，确保方向正确
3. **/ecc:code-review** 用于审查，发现问题
4. **/ecc:build-fix** 用于修复构建错误
5. **提交** 使用 conventional commits 格式

---

## Próximos Passos

- Após completar o curso, fazer [exercises/入门Exercícios.md](./exercises/入门Exercícios.md)
- Entrar em Phase 2: Avanço de Comandos Essenciais