# Regras de Teste

## Visão Geral das Regras

As regras ECC Regras-de-Teste definem padrões de teste obrigatórios para garantir qualidade de código. Estas regras cobrem desenvolvimento dirigido por testes (TDD), requisitos mínimos de cobertura e especificações de estrutura de teste, garantindo que todo código seja suficientemente verificado.

## Requisitos Centrais

### Cobertura Mínima de Testes: 80%

Devem incluir os seguintes três tipos de teste:

| Tipo de Teste | Descrição | Escopo |
|----------------|-----------|--------|
| Testes Unitários | Funções, utilities, componentes independentes | Individual units |
| Testes de Integração | Endpoints de API, operações de banco de dados | Integration points |
| Testes End-to-End | Fluxos críticos de usuário | Critical user flows |

### Desenvolvimento Dirigido por Testes (TDD)

**Workflow obrigatório**:

```
1. Escrever Teste (RED)   - Primeiro escrever um teste que falha
2. Executar Teste          - Verificar que o teste falha
3. Escrever Implementação Mínima (GREEN) - Escrever código mínimo para fazer o teste passar
4. Executar Teste          - Verificar que o teste passa
5. Refatorar (IMPROVE)    - Melhorar estrutura do código
6. Verificar Cobertura        - Garantir alcançar 80%+
```

## Detalhes de Implementação

### Padrão AAA (Arrange-Act-Assert)

Todos os testes devem seguir estrutura AAA:

```typescript
test('calculates cosine similarity correctly', () => {
  // Arrange - Preparar dados de teste
  const vector1 = [1, 0, 0];
  const vector2 = [0, 1, 0];

  // Act - Executar a operação testada
  const similarity = calculateCosineSimilarity(vector1, vector2);

  // Assert - Verificar resultado
  expect(similarity).toBe(0);
});
```

### Convenções de Nomenclatura de Testes

Usar nomes descritivos que expliquem o comportamento sendo testado:

```typescript
// Recomendado - Descrever comportamento esperado
test('returns empty array when no markets match query', () => {});
test('throws error when API key is missing', () => {});
test('falls back to substring search when Redis is unavailable', () => {});

// Evitar - Nomes vagos
test('test1', () => {});
test('edge case', () => {});
```

### Troubleshooting de Testes

Quando testes falham:

| Passo | Ação |
|-------|------|
| 1 | Usar agent **tdd-guide** |
| 2 | Verificar isolamento de teste |
| 3 | Validar correção de Mocks |
| 4 | Corrigir implementação, não testes (a menos que testes estejam errados) |

### Suporte a Agent

| Agent | Propósito | Quando Usar |
|-------|-----------|-------------|
| **tdd-guide** | Guia de desenvolvimento dirigido por testes | Usar proativamente para nova funcionalidade, correção de bugs |
| **code-reviewer** | Revisão de código | Após escrever código |

## Tratamento de Violações

### Cobertura Insuficiente

- Código com cobertura menor que 80% não deve ser feito merge
- Pipeline CI/CD deve bloquear commits de código com cobertura baixa
- Usar `c8` ou ferramenta similar para gerar relatórios de cobertura

### Falha de Teste

```
1. Analisar causa de falha
2. Diagnosticar se é problema de implementação ou problema de teste
3. Se problema de implementação - Corrigir código de implementação
4. Se problema de teste - Corrigir código de teste
5. Garantir que todos os testes passam antes de fazer commit
```

### Problemas Comuns

| Problema | Causa | Solução |
|----------|-------|---------|
| Flaky tests | Operações assíncronas, condições de corrida | Aumentar wait time, usar mocks |
| Falha de isolamento de teste | Poluição de estado compartilhado | Resetar estado antes de cada teste |
|Mocks incorretos | Valores esperados não coincidem com reais | Validar configuração de mock |

## Regras Relacionadas

| Regra Relacionada | Descrição |
|-------------------|-----------|
| Regras de Estilo de Código | Legibilidade e manutenibilidade de código |
| Regras de Segurança | Requisitos de teste de segurança |
| Regras de revisão de código | Verificações de cobertura de teste |
| Workflow de desenvolvimento | Integração de fluxo TDD |