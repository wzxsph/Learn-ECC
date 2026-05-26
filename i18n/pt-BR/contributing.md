# Learn-ECC Guia de Contribuição

Bem-vindo para contribuir com conteúdo para Learn-ECC! Este guia explica como adicionar novo conteúdo, melhorar conteúdo existente ou enviar alterações.

## Tipos de Contribuição

### 1. Adicionar Novo Conteúdo

Tipos de conteúdo que podem ser contribuídos:
- Novo capítulo ou módulo
- Novos exercícios
- Novos projetos práticos
- Complementos para cheatsheets
- Melhorias de tradução

### 2. Melhorar Conteúdo Existente

- Corrigir erros
- Melhorar clareza das explicações
- Adicionar mais exemplos
- Complementar respostas de exercícios
- Atualizar informações desatualizadas

### 3. Relatar Problemas

- Erros de digitação ou gramática
- Exemplos de código não funcionam
- Links quebrados
- Conteúdo faltando

## Padrões de Formato de Conteúdo

### Formato Markdown

Todo conteúdo deve usar formato Markdown, contendo as seguintes seções:

```markdown
# Título

## Objetivos de Aprendizado

Após completar esta seção, você será capaz de:
- Objetivo 1
- Objetivo 2

---

## Conteúdo Principal

### Subtítulo

Conteúdo...

---

## Exercícios

### Exercício 1

Descrição da tarefa...

### Método de Verificação

1. Passo de verificação 1
2. Passo de verificação 2

---

## Próximos Passos

- Próximo capítulo: [Link](./next.md)
- Retornar: [Índice](../README.md)
```

### Nomenclatura de Arquivos

- Use nomenclatura em chinês
- Use hífen `-` para separar palavras
- Nomenclatura de arquivos de capítulo: `01-NomeDoCapítulo.md`, `02-NomeDoCapítulo.md`
- Nomenclatura de arquivos de exercício: `Exercício-Nome.md`

### Padrões de Links

- Links de documentação: `./DocumentosDeReferência/NomeDoArquivo.md`
- Links internos do curso: `./NomeDoCapítulo.md`
- Caminhos relativos: Calculados a partir do local do arquivo atual

### Exemplos de Código

```javascript
// Anotação de linguagem de código
function example() {
  return "Hello ECC"
}
```

```bash
# Exemplo de linha de comando
node tests/run-all.js
```

### Formato de Tabelas

| Cabeçalho1 | Cabeçalho2 | Cabeçalho3 |
|-------|-------|-------|
| Conteúdo1 | Conteúdo2 | Conteúdo3 |

## Enviando Alterações

### Passos

1. Fork do repositório
2. Criar branch: `git checkout -b feature/NovaDescriçãoDeConteúdo`
3. Fazer alterações
4. Commit: `git commit -m "feat: Adicionar novo conteúdo"`
5. Push para remote: `git push origin feature/NovaDescriçãoDeConteúdo`
6. Criar Pull Request

### Padrão de Mensagens de Commit

```
<type>: <description>

<body opcional>
```

Tipos de Type:
- `feat`: Nova funcionalidade
- `fix`: Correção de bugs
- `docs`: Atualização de documentação
- `test`: Atualização de testes
- `refactor`: Refatoração

## Verificação de Qualidade

Antes de fazer commit, por favor verifique:

- [ ] Formato Markdown correto
- [ ] Todos os links funcionando
- [ ] Exemplos de código executáveis
- [ ] Exercícios possuem métodos de verificação
- [ ] Chinês sem erros de digitação
- [ ] Em conformidade com padrões de formato

## Feedback de Problemas

Se encontrar problemas ou sugestões, crie um Issue contendo:
- Descrição do problema
- Localização do arquivo
- Forma de melhoria sugerida

---

Obrigado pela sua contribuição!