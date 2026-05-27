# Scripts de Build

## Visão Geral

Scripts de build são usados para compilar, bundlar e validar código ECC durante o processo de desenvolvimento.

## Scripts Disponíveis

### build-all.js

Script principal que executa todos os scripts de build em sequência.

```bash
node scripts/build-all.js
```

### build-ecc.js

Compila o código principal do ECC.

```bash
node scripts/build-ecc.js
```

### build-plugins.js

Compila plugins ECC.

```bash
node scripts/build-plugins.js
```

## Processo de Build

### 1. Validação

Antes de compilar, o script valida:
- Arquivos JSON de configuração
- Dependências necessárias
- Estrutura de diretórios

### 2. Compilação

Compila código TypeScript/JavaScript:
- Verifica erros de tipo
- Bundla módulos
- Otimiza para produção

### 3. Geração de Artefatos

Gera artefatos de build:
- Arquivos JavaScript compilados
- Mapas de origem (se habilitados)
- Arquivos de declaração de tipo

## Flags de Build

| Flag | Descrição |
|------|----------|
| `--watch` | Modo de observação (rebuild automático) |
| `--prod` | Build de produção (minificado) |
| `--debug` | Build de debug |
| `--skip-tests` | Pular execução de testes |

## Exemplos

### Build de Desenvolvimento

```bash
node scripts/build-all.js
```

### Build de Produção

```bash
node scripts/build-all.js --prod
```

### Build com Observação

```bash
node scripts/build-all.js --watch
```