# Biblioteca de Funções de Utilitários

## Visão Geral

A biblioteca de funções utilitárias (`lib/utils.js`) fornece funções auxiliares usadas por vários scripts ECC para operações comuns como manipulação de caminho, formatação e operações de sistema de arquivos.

## Localização

A biblioteca está localizada em: `scripts/lib/utils.js`

## Funções Disponíveis

### Funções de Caminho

#### pathExists(path)
Verifica se um caminho existe no sistema de arquivos.

```javascript
const { pathExists } = require('./lib/utils');
pathExists('/caminho/para/arquivo'); // true/false
```

#### ensureDir(dir)
Garante que um diretório existe, criando-o se necessário.

```javascript
const { ensureDir } = require('./lib/utils');
ensureDir('/caminho/para/diretorio');
```

### Funções de Arquivo

#### readJsonFile(filepath)
Lê e faz parse de um arquivo JSON.

```javascript
const { readJsonFile } = require('./lib/utils');
const config = readJsonFile('./config.json');
```

#### writeJsonFile(filepath, data)
Escreve dados como JSON em arquivo.

```javascript
const { writeJsonFile } = require('./lib/utils');
writeJsonFile('./output.json', { key: 'value' });
```

#### copyFile(src, dest)
Copia arquivo de source para destination.

```javascript
const { copyFile } = require('./lib/utils');
copyFile('./source.js', './dest.js');
```

### Funções de Formatação

#### formatBytes(bytes)
Formata bytes em string legível (KB, MB, GB, etc.).

```javascript
const { formatBytes } = require('./lib/utils');
formatBytes(1024); // "1 KB"
formatBytes(1048576); // "1 MB"
```

#### formatDuration(ms)
Formata duração em milissegundos para string legível.

```javascript
const { formatDuration } = require('./lib/utils');
formatDuration(60000); // "1m"
formatDuration(3600000); // "1h"
```

### Funções de Console

#### log(message, level)
Outputa mensagem formatada para console.

```javascript
const { log } = require('./lib/utils');
log('Info message', 'info');
log('Warning message', 'warn');
log('Error message', 'error');
```

#### table(data)
Outputa dados em formato de tabela no console.

```javascript
const { table } = require('./lib/utils');
table([{ name: 'Alice', age: 30 }, { name: 'Bob', age: 25 }]);
```

## Uso em Scripts

Para usar a biblioteca em seus próprios scripts:

```javascript
const {
  pathExists,
  ensureDir,
  readJsonFile,
  writeJsonFile,
  formatBytes,
  log
} = require('./scripts/lib/utils');

// Verificar e criar diretório
if (!pathExists('./output')) {
  ensureDir('./output');
}

// Ler configuração
const config = readJsonFile('./config.json');

// Processar e salvar resultado
const result = processData(config);
writeJsonFile('./result.json', result);

// Log do progresso
log('Processamento concluído', 'info');
```