# Regras de Estilo de Código

## Visão Geral das Regras

As regras ECC Estilo-de-Código definem princípios centrais de organização de código e melhores práticas. Estas regras garantem que o código mantenha legibilidade, manutenibilidade e consistência, cobrindo tudo desde imutabilidade até convenções de nomenclatura.

## Requisitos Centrais

### Imutabilidade (Princípio-Chave)

**Obrigatório**: Sempre criar novos objetos, nunca modificar existentes.

```typescript
// Exemplo errado - modifica objeto original
function modify(user, name) {
  user.name = name;  // muda o objeto original
  return user;
}

// Exemplo correto - retorna novo objeto
function update(user, name) {
  return { ...user, name };  // cria nova cópia
}
```

**Razão**: Dados imutáveis previnem efeitos colaterais escondidos, tornam debugging mais fácil e suportam concorrência segura.

### Princípios de Design Centrais

| Princípio | Descrição | Prática |
|-----------|-----------|---------|
| **KISS** | Keep It Simple | Preferir solução mais simples que funcione, evitar otimização prematura, clareza sobre esperteza |
| **DRY** | Don't Repeat Yourself | Extrair lógica repetida para funções ou classes utilitárias compartilhadas, evitar drift de implementação de copy-paste |
| **YAGNI** | You Aren't Gonna Need It | Não construir funcionalidades ou abstrações especulativas, começar simples, refatorar quando necessário |

### Organização de Arquivos

| Requisito | Descrição |
|-----------|-----------|
| Muitos arquivos pequenos > Poucos arquivos grandes | Alta coesão, baixo acoplamento |
| Linhas típicas | 200-400 linhas, máximo 800 |
| Módulos grandes | Dividir | Extrair funções utilitárias de módulos grandes |
| Forma de organizar | Por funcionalidade/domínio, não por tipo |

### Tratamento de Erros

| Requisito | Descrição |
|-----------|-----------|
| Tratamento explícito | Tratar erros explicitamente em cada nível |
| Código UI | Fornecer mensagens de erro amigáveis ao usuário |
| Lado servidor | Registrar contexto de erro detalhado |
| Comportamento proibido | Não engolir erros silenciosamente |

### Validação de Input

| Requisito | Descrição |
|-----------|-----------|
| Validação de fronteira | Validar todo input na fronteira do sistema |
| Validação Schema | Usar validação baseada em schema onde possível |
| Fail fast | Falhar com mensagens de erro claras |
| Dados externos | Nunca confiar em dados externos (respostas de API, input de usuário, conteúdo de arquivo) |

## Detalhes de Implementação

### Convenções de Nomenclatura

| Tipo | Convenção | Exemplo |
|------|-----------|---------|
| Variáveis e funções | camelCase + nomes descritivos | `calculateTotal`, `userName` |
| Booleanos | Prefixo is/has/should/can | `isActive`, `hasPermission` |
| Interfaces, tipos, componentes | PascalCase | `UserProfile`, `ButtonComponent` |
| Constantes | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT`, `API_TIMEOUT` |
| Custom hooks | camelCase + prefixo use | `useUserData`, `useFetch` |

### Code Smells

#### Aninhamento Profundo

Mais de 4 níveis de aninhamento deve ser refatorado com early returns:

```typescript
// Evitar
function processOrder(order) {
  if (order) {
    if (order.items) {
      if (order.items.length > 0) {
        // lógica de processamento
      }
    }
  }
}

// Recomendado
function processOrder(order) {
  if (!order || !order.items || order.items.length === 0) {
    return;
  }
  // lógica de processamento
}
```

#### Números Mágicos

Usar constantes nomeadas ao invés de números mágicos:

```typescript
// Evitar
setTimeout(callback, 300000);

// Recomendado
const SESSION_TIMEOUT_MS = 5 * 60 * 1000; // 5 minutos
setTimeout(callback, SESSION_TIMEOUT_MS);
```

#### Funções Longas

Dividir funções grandes em funções menores com responsabilidades claras:

| Tamanho da Função | Recomendação |
|-------------------|---------------|
| <50 linhas | Ideal |
| 50-100 linhas | Aceitável, considerar dividir |
| >100 linhas | Deve ser dividida |

## Tratamento de Violações

### Checklist de Qualidade de Código

Devem ser verificados antes de marcar o trabalho como completo:

- [ ] Código legível e bem nomeado
- [ ] Funções pequenas (<50 linhas)
- [ ] Arquivos focados (<800 linhas)
- [ ] Sem aninhamento profundo (>4 níveis)
- [ ] Tratamento de erros apropriado
- [ ] Sem valores hardcoded (usar constantes ou configuração)
- [ ] Usar padrões imutáveis (sem mutation)

### Níveis de Severidade

| Nível | Significado | Ação |
|-------|-------------|------|
| CRITICAL | Vulnerabilidade de segurança ou risco de perda de dados | **Bloquear** - Deve ser corrigido antes de fazer merge |
| HIGH | Bug ou problema significativo de qualidade | **Alertar** - Deve ser corrigido antes de fazer merge |
| MEDIUM | Problema de manutenibilidade | **Informar** - Sugere correção |
| LOW | Problema de estilo ou sugestão pequena | **Nota** - Opcional |

## Regras Relacionadas

| Regra Relacionada | Descrição |
|-------------------|-----------|
| Regras de Teste | Requisitos de cobertura de testes |
| Regras de Segurança | Itens de verificação de segurança |
| Regras de revisão de código | Padrões de revisão detalhados |
| Workflow de desenvolvimento | Fluxo TDD e processo de revisão de código |