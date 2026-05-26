# Learn-ECC Guia de Contribuição

Contribuir conteúdo para o Learn-ECC é bem-vindo! Este guia explica como adicionar novos conteúdos, melhorar conteúdos existentes ou enviar alterações.

## Tipos de Contribuição

### 1. Adicionar Novo Conteúdo

Tipos de conteúdo que você pode contribuir:
- Novos capítulos ou módulos
- Novos exercícios
- Novos projetos práticos
- Complementação de cheatsheets
- Melhorias em traduções

### 2. Melhorar Conteúdo Existente

- Corrigir erros
- Melhorar clareza das explicações
- Adicionar mais exemplos
- Complementar respostas de exercícios
- Atualizar informações desatualizadas

### 3. Relatar Problemas

- Erros de digitação ou gramática
- Exemplos de código que não funcionam
- Links quebrados
- Conteúdo ausente

## Padrões de Formato de Conteúdo

### Formato Markdown

Todo conteúdo deve ser escrito em formato Markdown, incluindo as seguintes seções:

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

1. Etapa de verificação 1
2. Etapa de verificação 2

---

## Próximos Passos

- Próximo capítulo: [Link](./next.md)
- Retornar: [Índice](../README.md)
```

### Nomenclatura de Arquivos

- Use nomes em português
- Use hífen `-` para separar palavras
- Nomenclatura de arquivos de capítulo: `01-NomeDoCapítulo.md`, `02-NomeDoCapítulo.md`
- Nomenclatura de arquivos de exercício: `Exercício-Nome.md`

### Padrões de Link

- Links de referência: `./DocumentosDeReferência/nomedoarquivo.md`
- Links dentro do curso: `./nomedo capítulo.md`
- Caminhos relativos: calculados a partir da localização do arquivo atual

### Exemplos de Código

```javascript
// Etiqueta de linguagem de código
function example() {
  return "Hello ECC"
}
```

```bash
# Exemplo de linha de comando
node tests/run-all.js
```

### Formato de Tabela

| Cabeçalho 1 | Cabeçalho 2 | Cabeçalho 3 |
|-------------|-------------|-------------|
| Conteúdo 1  | Conteúdo 2  | Conteúdo 3  |

## Enviar Alterações

### Passos

1. Fork do repositório
2. Criar branch: `git checkout -b feature/descrição-do-novo-conteúdo`
3. Fazer alterações
4. Commitar: `git commit -m "feat: adicionar novo conteúdo"`
5. Push para remote: `git push origin feature/descrição-do-novo-conteúdo`
6. Criar Pull Request

### Padrões de Mensagem de Commit

```
<type>: <description>

<optional body>
```

Tipos de Type:
- `feat`: Nova funcionalidade
- `fix`: Correção de bug
- `docs`: Atualização de documentação
- `test`: Atualização de teste
- `refactor`: Refatoração

## Controle de Qualidade

Antes de enviar, verifique:

- [ ] Formato Markdown correto
- [ ] Todos os links funcionam
- [ ] Exemplos de código executáveis
- [ ] Exercícios têm método de verificação
- [ ] Sem erros de digitação em português
- [ ] Em conformidade com padrões de formato

## Feedback de Problemas

Se encontrar um problema ou sugestão, crie um Issue incluindo:
- Descrição do problema
- Localização do arquivo
- Método de melhoria proposto

---

Obrigado pela sua contribuição!