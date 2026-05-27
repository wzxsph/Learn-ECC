# Comandos de Aprendizado e Melhoria

## Visão Geral

Comandos de aprendizado e melhoria são usados para extrair padrões de sessões, gerenciar instinct e knowledge organizacional.

## Lista de Comandos

### /learn

**Propósito**: Extrair padrões reutilizáveis da sessão atual e salvar como skills candidatas

**Descrição**: Analisa a sessão atual, extraindo quaisquer padrões que valham a pena salvar como skills. Executar `/learn` pode ser feito a qualquer momento durante a sessão.

**Tipos de Conteúdo Extraído**:

| Tipo | Descrição |
|---|---|
| **Padrões de Solução de Erro** | Qual erro? Qual a causa raiz? O que corrigiu? O padrão é reutilizável para erros similares? |
| **Técnicas de Debug** | Passos de debug não óbvios, combinações de ferramentas eficazes, padrões de diagnóstico |
| **Workarounds** | Quirks de biblioteca, limitações de API, correções específicas de versão |
| **Padrões Específicos de Projeto** | Convenções descobertas de codebase, decisões de arquitetura, padrões de integração |

**Formato de Saída**: Salvo em `~/.claude/skills/learned/[pattern-name].md`

```markdown
# [Nome Descritivo do Padrão]

**Extraído:** [Data]
**Contexto:** [Breve descrição de quando isso se aplica]

## Problema
[O que esse padrão resolve - seja específico]

## Solução
[O padrão/técnica/workaround]

## Exemplo
[Exemplo de código se aplicável]

## Quando Usar
[Condições de gatilho - o que deveria ativar esse skill]
```

**Fluxo de Trabalho**:
1. Revisar padrões na sessão que podem ser extraídos
2. Identificar os aprendizados mais valiosos/reutilizáveis
3. Draft de arquivo skill
4. Pedir confirmação do usuário antes de salvar
5. Salvar em `~/.claude/skills/learned/`

**Melhores Práticas**:
- Não extrair correções triviais (erros de digitação, erros de sintaxe simples)
- Não extrair problemas únicos (ex: interrupção de API específica)
- Focar em padrões que economizam tempo em sessões futuras
- Manter skills focados - um padrão por skill

---

### /learn-eval

**Propósito**: Extrair padrões + auto-avaliação de qualidade

**Descrição**: Extrai padrões enquanto faz avaliação de qualidade simultaneamente.

---

### /evolve

**Propósito**: Analisar instinct + sugerir estrutura de evolução

**Descrição**: Analisa instinct aprendidos, fornecendo sugestões de evolução estrutural.

---

### /promote

**Propósito**: Promover instinct de projeto para escopo global

**Descrição**: Promove instinct específico de projeto para conhecimento globalmente disponível.

---

### /instinct-status

**Propósito**: Mostrar todos os instincts aprendidos

**Descrição**: Mostra status e confiança de todos os instincts atuais.

---

### /instinct-export

**Propósito**: Exportar instinct para arquivo

**Descrição**: Exporta instinct para formato compartilhável.

---

### /instinct-import

**Propósito**: Importar instinct de arquivo/URL

**Descrição**: Importa instinct de arquivo ou URL para o sistema.

---

### /skill-create

**Propósito**: Analisar histórico git + gerar arquivo skill

**Descrição**: Analisa histórico git do projeto, extrai padrões reutilizáveis e gera arquivos skill.

**Fluxo de Trabalho**:
1. Analisar histórico de commits
2. Identificar padrões repetitivos
3. Gerar arquivo skill
4. Verificar e salvar

---

### /skill-health

**Propósito**: Painel de saúde de composição de Skills

**Descrição**: Mostra status de saúde e estatísticas de uso da composição de skills.

---

### /rules-distill

**Propósito**: Escanear skills + extrair princípios entre domínios

**Descrição**: Escaneia todos os skills, extraindo princípios universais entre domínios.

---

## Comandos Relacionados

- `/learn` - Extrair padrões
- `/skill-create` - Gerar skill do histórico git
- `/instinct-status` - Ver status do instinct