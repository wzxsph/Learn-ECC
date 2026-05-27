# Executor de Testes

## Visão Geral

O executor de testes ECC é um framework de teste personalizado baseado em Node.js usado para validar funcionalidades do ECC.

## Estrutura de Testes

### Localização dos Testes

Os testes estão localizados no diretório `tests/` com a seguinte estrutura:

```
tests/
├── run-all.js          # Script principal que executa todos os testes
├── unit/               # Testes unitários
├── integration/        # Testes de integração
└── e2e/               # Testes end-to-end
```

### Executar Todos os Testes

```bash
node tests/run-all.js
```

### Executar Testes Específicos

```bash
# Testes unitários apenas
node tests/run-all.js --suite unit

# Testes de integração apenas
node tests/run-all.js --suite integration

# Testes E2E apenas
node tests/run-all.js --suite e2e
```

## Cobertura

A cobertura de código é medida usando `c8` e relatada em formato HTML.

### Gerar Relatório de Cobertura

```bash
npm run test:coverage
```

### Ver Relatório HTML

Após executar testes com cobertura, abra `coverage/index.html` no navegador.

## Métricas de Qualidade

### Limites de Cobertura

| Métrica | Limite Mínimo |
|---------|---------------|
| Branch | 80% |
| Functions | 80% |
| Lines | 80% |
| Statements | 80% |

### Verificar Cobertura

```bash
npm run test:coverage -- --fail-under-line 80
```

## Integração CI/CD

O executor de testes é projetado para integração com pipelines CI/CD.

### Exemplo de Configuração GitHub Actions

```yaml
- name: Run tests
  run: |
    npm install
    node tests/run-all.js --json | tee test-results.json

- name: Upload coverage
  uses: codecov/codecov-action@v3
  with:
    files: ./coverage/lcov.info
```