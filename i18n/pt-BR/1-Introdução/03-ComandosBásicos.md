# 03 - Comandos Básicos

## Tipos de Comandos

ECC oferece três tipos de comandos:

### 1. Comandos com Barra (Slash Commands)

Usar digitando diretamente:

| Comando | Função | Exemplo |
|------|------|------|
| `/ecc:plan` | Criar plano de implementação | `/ecc:plan "Adicionar autenticação de usuário"` |
| `/ecc:code-review` | Revisão de código | `/ecc:code-review` |
| `/ecc:build-fix` | Reparo de erros de build | `/ecc:build-fix` |
| `/ecc:learn` | Extrair padrões da sessão | `/ecc:learn` |
| `/ecc:skill-create` | Gerar Skill do histórico Git | `/ecc:skill-create` |
| `/ecc:sessions` | Gerenciamento de histórico de sessões | `/ecc:sessions` |

Nota: Versões instaladas manualmente podem usar comandos sem prefixo (como `[/plan](../DocumentosDeReferência/commands/01-Workflow-Principal.md)`), instalações via plugin usam prefixo `/ecc:`.

### 2. Skills (Superfície Principal de Workflow)

Skills são a principal forma de chamada de workflow do ECC:

| Skill | Função |
|-------|------|
| `[tdd-workflow](../DocumentosDeReferência/skills/测试.md)` | Desenvolvimento orientado a testes |
| `[e2e-testing](../DocumentosDeReferência/skills/测试.md)` | Testes end-to-end |
| `security-review` | Revisão de segurança |
| `verification-loop` | Loop de verificação |
| `search-first` | Workflow de pesquisa primeiro |

Forma de chamada:
```
Usar skill [tdd-workflow](../DocumentosDeReferência/skills/测试.md)
```

### 3. Comandos de Agent

Executar subtarefas através de Agents:

| Agent | Uso |
|-------|------|
| `planner` | Planejamento de implementação |
| `code-reviewer` | Revisão de código |
| `build-error-resolver` | Reparo de erros de build |
| `security-reviewer` | Revisão de segurança |
| `architect` | Design de arquitetura |

Forma de chamada:
```
@planner
```

## Workflows Frequentes

### Desenvolver Nova Funcionalidade

```
/ecc:plan "Adicionar funcionalidade de autenticação de usuário"
→ planner cria blueprint de implementação
Usar skill [tdd-workflow](../DocumentosDeReferência/skills/测试.md)
→ tdd-guide força teste primeiro
/ecc:code-review
→ code-reviewer verifica código
```

### Reparar Bug

```
Usar skill [tdd-workflow](../DocumentosDeReferência/skills/测试.md)
→ tdd-guide: Escrever teste de reprodução
→ Implementar reparo, verificar teste passando
/ecc:code-review
→ code-reviewer: Verificar regressão
```

### Preparar para Produção

```
/ecc:security-scan
→ security-reviewer: Auditoria OWASP Top 10
Usar skill [e2e-testing](../DocumentosDeReferência/skills/测试.md)
→ e2e-runner: Testar fluxos críticos de usuário
/ecc:test-coverage
→ Verificar 80%+ de cobertura
```

## Comandos MCP

| Comando | Função |
|------|------|
| `/mcp` | Gerenciar servidor MCP |
| `/ecc:status` | Verificar status |

## Próximos Passos

- [Primeira Tarefa](./04-PrimeiraTarefa.md) - Completar sua primeira tarefa
- [Retornar ao diretório de iniciantes](../README.md)