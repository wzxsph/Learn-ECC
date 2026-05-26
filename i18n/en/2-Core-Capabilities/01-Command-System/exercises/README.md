# Exercises: Command System Mastery

## Exercise 1: Explore All Command Categories

### Objective
Understand all command categories in Claude Code, and identify key commands in each category.

### Steps

1. **List all available commands**
   - Enter `/` in Claude Code to view all commands
   - Note how commands are organized by category

2. **Explore each category**
   - For each category, try `/help` or `?` for help
   - Record core commands and purposes for each category

3. **Record command list**
   Create your own command quick reference:

   | Category | Command | Purpose | Usage Frequency |
   |----------|---------|---------|-----------------|
   | Development | /plan | Create implementation plan | ★★★ |
   | ... | ... | ... | ... |

### Acceptance Criteria
- [ ] Can list commands from at least 5 categories
- [ ] Recorded 20+ common commands
- [ ] Understood the classification logic of commands

---

## Exercise 2: Combine Multiple Commands

### Objective
Learn how to combine multiple commands to create coherent development workflows.

### Scenario
Fix a hypothetical build error and perform code review.

### Steps

1. **Simulate scenario**
   Assume you are developing a Go project with a build error:

   ```
   # Scenario setup (no actual execution needed)
   Assume the following build errors need fixing:
   - import cycle detected
   - undefined: someFunction
   ```

2. **Command combination exercise**
   Think through and record the following command combinations in order:

   a. First use `/plan` to plan fix steps
   b. Use `/go-build` to attempt build
   c. Use `/build-fix` to fix errors
   d. Use `/code-review` for review
   e. Use `/verify` to verify fix

3. **Record workflow**
   Create your fix workflow document:

   ```markdown
   ## Build Fix Workflow

   1. /plan - Analyze problem,制定修复计划
   2. /build-fix - Execute fix
   3. /code-review - Check fix quality
   4. /verify - Verify functionality
   ```

### Acceptance Criteria
- [ ] Understood the logic of command combinations
- [ ] Created at least 3 custom workflows
- [ ] Recorded best practices for command combinations

---

## Exercise 3: Create Custom Workflows

### Objective
Create automated workflow command combinations based on project needs.

### Scenario
Create a development workflow for a hypothetical Flutter project.

### Steps

1. **Define workflow goals**
   Your Flutter project needs the following workflows:

   - Daily development: Code → Test → Build
   - Code review: Self-check → Review → Fix
   - Release prep: Test → Build → Verify

2. **Design command combinations**

   **Daily Development Workflow**:
   ```
   /flutter-build → /flutter-test → /verify
   ```

   **Code Review Workflow**:
   ```
   /flutter-review → /build-fix → /code-review
   ```

3. **Create workflow document**
   Create `CLAUDE_WORKFLOWS.md` in your project:

   ```markdown
   # Claude Code Workflows

   ## Flutter Development Workflows

   ### Daily Development
   1. Use `/plan` to plan task
   2. Use `/flutter-build` to build
   3. Use `/flutter-test` to test
   4. Use `/verify` to verify

   ### Code Review
   1. Use `/flutter-review` for self-check
   2. Use `/build-fix` to fix issues
   3. Use `/code-review` to request review

   ### Release Prep
   1. Run complete test suite
   2. Execute build verification
   3. Use `/verify` for final confirmation
   ```

4. **Practice verification**
   If you have an actual project, try executing this workflow.

### Acceptance Criteria
- [ ] Designed 3 complete workflows
- [ ] Created workflow documentation
- [ ] Tested at least 1 workflow in an actual project

---

## Exercise Answer Reference

### Exercise 1 Command List Example

| Category | Command | Purpose |
|----------|---------|---------|
| Development | /plan | Implementation planning |
| Development | /tdd | Test-driven development |
| Development | /build-fix | Build fix |
| Development | /verify | Change verification |
| Code Review | /code-review | General code review |
| Code Review | /security-review | Security review |
| Build | /go-build | Go build |
| Build | /rust-build | Rust build |
| Project | /project-init | Project initialization |
| Project | /projects | Project list |
| Skills | /skill-create | Create skill |
| Skills | /learn | Learning extraction |
| Learning | /deep-research | Deep research |

### Exercise 3 Workflow Template

```markdown
# [Project Name] Claude Code Workflows

## Development Flow
- Daily: /plan → [Code] → /verify
- Fix: /build-fix → /code-review → /verify
- Release: /test → /build → /verify

## Usage Instructions
1. Use `/plan` to start any task
2. Run `/verify` after completing code
3. Use `/code-review` for important changes
```

---

## Deep Learning

- Read complete command documentation: [../../../Reference-Docs/commands/01-核心工作流.md](../../../Reference-Docs/commands/01-核心工作流.md)
- View all available command categories
- Learn hidden options and parameters for commands