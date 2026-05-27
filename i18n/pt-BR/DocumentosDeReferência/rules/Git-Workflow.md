# Workflow Git

## Formato de Mensagens de Commit

```
<tipo>: <descrição>

<corpo opcional>
```

**Tipos (type)**:
- `feat`: nova funcionalidade
- `fix`: correção de bug
- `refactor`: refatoração
- `docs`: documentação
- `test`: testes
- `chore`: tarefas diversas
- `perf`: otimização de performance
- `ci`: CI/CD

> Nota: Atribuição desabilitada globalmente via `~/.claude/settings.json`

## Workflow de Pull Request

Ao criar PR:
1. Analisar histórico completo de commits (não apenas o último commit)
2. Usar `git diff [base-branch]...HEAD` para ver todas as mudanças
3. Draft de resumo abrangente de PR
4. Incluir TODOs do plano de teste
5. Se novo branch, fazer push com flags `-u`


## Nomenclatura de Branches

Usar padrão de nomenclatura git flow:
- `feature/`: novas funcionalidades
- `fix/`: correções
- `refactor/`: refatoração
- `docs/`: documentação

## Workflow de Desenvolvimento

Workflow completo:
1. **Pesquisa & Reutilização** - GitHub search, docs de biblioteca, registros de pacotes
2. **Planejamento** - Usar agent planner para criar plano de implementação
3. **Abordagem TDD** - Escrever testes primeiro, depois implementar
4. **Revisão de Código** - Usar agent code-reviewer
5. **Commit & Push** - Mensagens de commit detalhadas, seguir conventional commits