# Git Workflow

## Commit Message Format
```
<type>: <description>

<optional body>
```

**Types (type)**:
- `feat`: New feature
- `fix`: Bug fix
- `refactor`: Refactoring
- `docs`: Documentation
- `test`: Test
- `chore`: Miscellaneous
- `perf`: Performance optimization
- `ci`: CI/CD

> Note: Attribution globally disabled via `~/.claude/settings.json`

## Pull Request Workflow

When creating PRs:
1. Analyze full commit history (not just latest commit)
2. Use `git diff [base-branch]...HEAD` to see all changes
3. Draft comprehensive PR summary
4. Include test plan with TODOs
5. If new branch, push with `-u` flag


## Branch Naming

Use standard git flow branch naming:
- `feature/`: New feature
- `fix/`: Fix
- `refactor/`: Refactoring
- `docs/`: Documentation

## Development Process

Full development process:
1. **Research & Reuse** - GitHub search, library docs, package registries
2. **Plan** - Use planner agent to create implementation plan
3. **TDD Approach** - Write tests first, then implement
4. **Code Review** - Use code-reviewer agent
5. **Commit & Push** - Detailed commit messages, follow conventional commits