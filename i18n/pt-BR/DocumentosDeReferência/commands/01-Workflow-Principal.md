# Comandos de Workflow Principal

Este documento apresenta os comandos no ECC para workflows de desenvolvimento principais.

---

## /plan

**Propósito**: Resumir requisitos, avaliar riscos e criar plano de implementação passo a passo. **Aguardar confirmação do usuário** antes de tocar qualquer código.

**Como Usar**:
```
/plan [descrição de feature | caminho do arquivo PRD]
```

**Cenários de Uso**:
- Iniciar novo desenvolvimento de feature
- Fazer mudanças de arquitetura significativas
- Trabalho de refatoração complexo
- Quando requisitos são incertos ou vagos

**Descrição Detalhada**:
Este comando irá:
1. **Resumir requisitos** - Expressar claramente o que precisa ser construído
2. **Identificar riscos** - Revelar problemas e obstáculos potenciais
3. **Criar plano passo a passo** - Decompor a implementação em fases
4. **Aguardar confirmação** - Deve obter aprovação explícita do usuário antes de continuar

Suporta vários modos de entrada:
| Entrada | Modo | Comportamento |
|---------|------|---------------|
| `path/to/name.prd.md` | Modo PRD | Ler PRD, selecionar próximo milestone de entrega a trabalhar |
| Outro caminho markdown | Modo de Referência | Ler arquivo como contexto e gerar plano inline |
| Texto livre | Modo Conversa | Gerar plano inline |
| Entrada vazia | Modo Esclarecimento | Perguntar o que deve ser planejado |

---

## /code-review

**Propósito**: Revisão completa de código de mudanças não commitadas localmente ou PRs do GitHub.

**Como Usar**:
```
/code-review                 # Modo de revisão local
/code-review [número PR | link PR]  # Modo de revisão de PR
```

**Cenários de Uso**:
- Revisão completa antes de fazer commit
- Revisão de PR para descobrir problemas potenciais
- Aprender melhores práticas do codebase

**Modo de Revisão Local**:
1. Coletar arquivos alterados (`git diff --name-only HEAD`)
2. Verificar problemas de segurança (credenciais hardcoded, SQL injection, XSS, etc.)
3. Verificar problemas de qualidade de código (funções >50 linhas, arquivos >800 linhas, etc.)
4. Gerar relatório com anotações de nível de severidade

**Modo de Revisão de PR**:
1. Obter metadados e diff do PR
2. Verificar padrões e documentos de planejamento relevantes do projeto
3. Ler arquivos de mudança completos
4. Aplicar 7 categorias de revisão (correção, type safety, conformidade de padrão, segurança, performance, completude, manutenibilidade)
5. Executar comandos de verificação (type check, lint, test, build)
6. Publicar comentários de revisão no GitHub

---

## /build-fix

**Propósito**: Detectar sistema de build e corrigir erros de build/tipo incrementalmente.

**Como Usar**:
```
/build-fix
```

**Cenários de Uso**:
- Quando o build falha
- Erros de tipo TypeScript/JavaScript
- Erros de compilação
- Problemas de dependência

**Fluxo de Trabalho**:
1. **Detectar sistema de build** - Selecionar comando de build apropriado baseado no tipo de arquivo
2. **Analisar erros** - Agrupar por arquivo e ordem de dependência
3. **Corrigir incrementalmente** - Corrigir um erro por vez
4. **Verificar** - Executar build novamente após cada correção

Sistemas de build suportados:

| Indicador | Comando de Build |
|-----------|-----------------|
| `package.json` + script `build` | `npm run build` |
| `tsconfig.json` (TypeScript) | `npx tsc --noEmit` |
| `Cargo.toml` | `cargo build` |
| `pom.xml` | `mvn compile` |
| `build.gradle` | `./gradlew compileJava` |
| `go.mod` | `go build ./...` |
| `pyproject.toml` | `python -m compileall` ou `mypy` |

---

## /verify

**Propósito**: Verificar se mudanças de código funcionam conforme esperado. Usado para verificar PRs, confirmar correções ou testar mudanças.

**Como Usar**:
```
/verify [quick]           # Verificação rápida (recomendado)
/verify full             # Verificação completa
```

**Cenários de Uso**:
- Verificar mudanças de PR
- Confirmar que a correção funciona
- Testar mudanças manualmente
- Verificação local antes de fazer push

**Passos de Verificação**:
1. Executar testes relevantes
2. Verificar type safety
3. Verificar se o build é bem-sucedido
4. Verificar linting

---

## /quality-gate

**Propósito**: Executar pipeline de qualidade ECC para um arquivo ou escopo de projeto e relatar passos de correção.

**Como Usar**:
```
/quality-gate [path|.] [--fix] [--strict]
```

**Cenários de Uso**:
- Executar verificação de qualidade sob demanda
- Integrar em pipeline CI
- Garantir que código está em conformidade com padrões do projeto

**Passos do Pipeline**:
1. Detectar linguagem/ferramenta alvo
2. Executar verificação de formatter
3. Executar verificação de lint/type
4. Gerar lista de correções concisa

---

## /tdd

**Propósito**: Workflow de desenvolvimento dirigido por testes. Segue o ciclo vermelho-verde-refatorar.

**Como Usar**:
```
/tdd                      # Iniciar workflow TDD
```

**Cenários de Uso**:
- Implementar nova feature
- Corrigir bug (escrever teste que falha primeiro)
- Adicionar cobertura de teste
- Aprender metodologia TDD

**Ciclo TDD**:
```
RED    → Escrever um teste que falha
GREEN  → Escrever código mínimo para fazer o teste passar
REFACTOR → Melhorar código, testes permanecem verdes
REPEAT → Próximo caso de teste
```

**Melhores Práticas**:
- **Escreva testes primeiro** - Antes de qualquer implementação
- **Execute testes após cada mudança** - Verificar progresso
- **Teste comportamento, não implementação** - Focar em interface não detalhes
- **Inclua casos de borda** - Null, vazio, máximo, etc.

---

## Integração de Comandos

```
Requisitos não claros → /plan-prd (criar PRD)
     ↓
Requisitos claros → /plan (criar plano de implementação)
     ↓
Implementar com /tdd (ou /feature-dev)
     ↓
/build-fix para corrigir erros de build
     ↓
/code-review para revisão de código
     ↓
/quality-gate para quality gate
     ↓
/verify para verificar
     ↓
/pr para criar PR
```