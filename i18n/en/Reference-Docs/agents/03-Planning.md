# Planning Agents

Planning agents specialize in feature planning, technical architecture design, and implementation step decomposition.

## Agent List

| Agent Name | Purpose | Model Used | Core Tools |
|------------|---------|------------|------------|
| planner | Complex feature implementation planning expert | opus | Read, Grep, Glob |
| code-architect | Feature architecture design expert | sonnet | Read, Grep, Glob, Bash |
| pr-test-analyzer | PR test coverage analysis | sonnet | Read, Grep, Glob, Bash |
| type-design-analyzer | Type design analysis | sonnet | Read, Grep, Glob |
| comment-analyzer | Code comment analysis | sonnet | Read, Grep, Glob |

---

## planner

### Name and Purpose
Planning expert for complex feature implementation and refactoring. Used proactively when users request feature implementation, architecture changes, or complex refactoring.

### Capabilities
- Requirements analysis and detailed implementation plan creation
- Decompose complex features into manageable steps
- Identify dependencies and potential risks
- Suggest optimal implementation order
- Consider edge cases and error scenarios

### Applicable Scenarios
- New feature implementation planning
- Architecture change planning
- Complex refactoring planning
- Technical debt cleanup planning

### Tools Used
- Read: Read existing code and documentation
- Grep: Search relevant patterns and existing implementations
- Glob: Find relevant files

### Collaboration with Other Agents
- Generated plans further refined by code-architect
- Generated plans used by tdd-guide to write tests
- Generated plans reviewed by code-reviewer

### Planning Process

1. **Requirements Analysis**
   - Fully understand feature request
   - Ask clarifying questions if needed
   - Identify success criteria
   - List assumptions and constraints

2. **Architecture Review**
   - Analyze existing codebase structure
   - Identify affected components
   - Review similar implementations
   - Consider reusable patterns

3. **Step Decomposition**
   - Create detailed steps containing:
     - Clear, specific actions
     - File paths and locations
     - Dependencies between steps
     - Estimated complexity
     - Potential risks

4. **Implementation Order**
   - Order by dependencies
   - Group related changes
   - Minimize context switching
   - Support incremental testing

### Planning Output Format

```markdown
# Implementation Plan: [Feature Name]

## Overview
[2-3 sentence summary]

## Requirements
- [Requirement 1]
- [Requirement 2]

## Architecture Changes
- [Change 1: File path and description]
- [Change 2: File path and description]

## Implementation Steps

### Phase 1: [Phase Name]
1. **[Step Name]** (File: path/to/file.ts)
   - Action: Specific action to take
   - Reason: Why this step is needed
   - Dependency: None / Requires step X
   - Risk: Low/Medium/High

2. **[Step Name]** (File: path/to/file.ts)
   ...

### Phase 2: [Phase Name]
...

## Test Strategy
- Unit tests: [Files to test]
- Integration tests: [Flows to test]
- E2E tests: [User journeys to test]

## Risks and Mitigation
- **Risk**: [Description]
  - Mitigation: [How to handle]

## Success Criteria
- [ ] Criterion 1
- [ ] Criterion 2
```

---

## code-architect

### Name and Purpose
Design feature architecture based on deep understanding of existing codebase. Provide implementation blueprints with specific files, interfaces, data flow, and build order.

### Capabilities
- Research existing code organization and naming conventions
- Identify existing architectural patterns
- Document testing patterns and existing boundaries
- Understand dependency graph before proposing new abstractions
- Design feature architecture that naturally fits current patterns

### Applicable Scenarios
- New feature architecture design
- Existing feature refactoring design
- Code organization optimization

### Tools Used
- Read: Read existing code
- Grep: Search patterns and conventions
- Glob: Find relevant files
- Bash: Run diagnostic commands

### Collaboration with Other Agents
- Architecture design based on planner's plans
- Provide blueprints for code implementation
- Collaborate with build-error-resolver to fix build issues

### Design Process

1. **Pattern Analysis**
   - Research existing code organization and naming conventions
   - Identify existing architectural patterns
   - Note testing patterns and existing boundaries
   - Understand dependencies before proposing new abstractions

2. **Architecture Design**
   - Design features to naturally fit current patterns
   - Choose the simplest architecture that meets requirements
   - Avoid speculative abstractions (unless already used by codebase)

3. **Implementation Blueprint**
   For each important component provide:
   - File path
   - Purpose
   - Key interfaces
   - Dependencies
   - Data flow role

4. **Build Order**
   Order implementation by dependencies:
   1. Types and interfaces
   2. Core logic
   3. Integration layer
   4. UI
   5. Tests
   6. Documentation

### Output Format

```markdown
## Architecture: [Feature Name]

### Design Decisions
- Decision 1: [Rationale]
- Decision 2: [Rationale]

### Files to Create
| File | Purpose | Priority |
|------|---------|----------|
| | | |

### Files to Modify
| File | Change | Priority |
|------|--------|----------|
| | | |

### Data Flow
[Description]

### Build Order
1. Step 1
2. Step 2
```

---

## pr-test-analyzer
(See [4. Testing Agents](./4.%20Testing%20Agents.md) for details)
---

## type-design-analyzer

### Name and Purpose
Analyze whether type design makes illegal states difficult or impossible to represent.

### Capabilities
- Encapsulation evaluation
- Invariant expression analysis
- Invariant usefulness evaluation
- Enforcement validation

### Applicable Scenarios
- Type system design review
- API design review
- Domain modeling review

### Tools Used
- Read: Read type definitions
- Grep: Search type usage
- Glob: Find type files

### Collaboration with Other Agents
- Collaborate with code-reviewer for code review
- Collaborate with code-architect for architecture design

### Evaluation Criteria

1. **Encapsulation**
   - Are internals hidden?
   - Can invariants be violated from outside?

2. **Invariant Expression**
   - Do types encode business rules?
   - Are impossible states prevented at the type level?

3. **Invariant Usefulness**
   - Do these invariants prevent real bugs?
   - Are they aligned with the domain?

4. **Enforcement**
   - Are invariants enforced by the type system?
   - Is there a simple escape hatch?

### Output Format

For each reviewed type:
- Type name and location
- Four-dimensional ratings
- Overall evaluation
- Specific improvement suggestions

---

## comment-analyzer

### Name and Purpose
Analyze code comment accuracy, completeness, maintainability, and comment rot risk.

### Capabilities
- Fact accuracy verification
- Completeness checking
- Long-term value evaluation
- Misleading element identification

### Applicable Scenarios
- During code review
- When evaluating documentation quality
- When identifying comment technical debt

### Tools Used
- Read: Read code and comments
- Grep: Search comments and code
- Glob: Find source files

### Collaboration with Other Agents
- Collaborate with code-reviewer for code review
- Collaborate with doc-updater for documentation updates

### Analysis Framework

1. **Fact Accuracy**
   - Verify claims against code
   - Check parameter and return descriptions against implementation
   - Flag outdated references

2. **Completeness**
   - Check complex logic has sufficient explanation
   - Verify important side effects and edge cases are documented
   - Ensure public APIs have sufficient comments

3. **Long-term Value**
   - Flag comments that merely restate code
   - Identify fragile comments that rot quickly
   - Discover TODO/FIXME/HACK debt

4. **Misleading Elements**
   - Comments contradicting code
   - Outdated references to removed behavior
   - Over-promising or under-describing behavior

### Output Format

Advisory findings grouped by severity:
- Inaccurate
- Stale
- Incomplete
- Low-value
[Return to Agent Index](../README.md)