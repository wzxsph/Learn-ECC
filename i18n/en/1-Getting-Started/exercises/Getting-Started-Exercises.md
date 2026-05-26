# Getting Started Exercises

Complete the following exercises to reinforce ECC fundamentals.

## Exercise 1: Environment Verification

**Objective**: Confirm ECC is correctly installed

**Steps**:
1. Run test suite: `node tests/run-all.js`
2. Verify all tests pass
3. List `.claude/` directory structure

**Acceptance Criteria**:
- All tests pass
- Can see agents, skills, hooks, rules directories

---

## Exercise 2: Using Slash Commands

**Objective**: Get familiar with common slash commands

**Steps**:
1. Start Claude Code
2. Execute the following commands in order, observe output:
   - `/help` or `/ecc:plan`
   - `/ecc:code-review`
3. Record the output format for each command

**Acceptance Criteria**:
- Understand each command's function and output format

---

## Exercise 3: Create a Simple Agent

**Objective**: Understand Agent file format

**Steps**:
1. View existing Agent files in `agents/` directory
2. Reference the format of `planner.md`
3. Create a simple `hello-agent.md` that outputs a greeting

**Acceptance Criteria**:
- New Agent file format is correct
- Contains YAML frontmatter (name, description, tools, model)

---

## Exercise 4: Running Hooks

**Objective**: Understand Hook mechanism

**Steps**:
1. View Hook configurations in `hooks/` directory
2. Run `node scripts/hooks/run-with-flags.js --help` (if supported)
3. Understand Hook trigger timing and execution logic

**Acceptance Criteria**:
- Can describe how Hooks work
- Understand the difference between PreToolUse and PostToolUse

---

## Exercise 5: Comprehensive Task

**Objective**: Complete a small project

**Task**: Create a name formatting utility

**Functional Requirements**:
- `formatName(name)` - Format as "Last, First"
- `initials(name)` - Return abbreviation

**Execution Flow**:
1. Use `/ecc:plan` to plan
2. Use `/tdd` to develop
3. Use `/ecc:code-review` to review

**Acceptance Criteria**:
- Fully implement both functions
- Include test cases
- Pass code review

---

[Return to Getting Started README](../README.md)