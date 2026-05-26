# 04 - Your First Task

## Learning Objectives

- Understand the complete development workflow
- Master the complete workflow: /ecc:plan → /ecc:code-review → /ecc:build-fix → commit
- Independently complete a simple feature development

---

## Complete Development Workflow

ECC recommends the following development workflow:

```
Step 1: /ecc:plan        → Plan task
Step 2: /ecc:code-review → Review code
Step 3: /ecc:build-fix   → Fix build
Step 4: Commit code      → Git commit
```

---

## Step 1: Plan with /ecc:plan

### Input

Assume we want to implement a simple calculator function:

```
/ecc:plan Implement a calculator supporting addition, subtraction, multiplication, and division
```

### Expected Output

The /ecc:plan command will:

1. **Restate requirements** - "Implement a calculator supporting addition, subtraction, multiplication, and division..."
2. **Risk assessment** - "Need to note: division by zero case..."
3. **Step-by-step plan** -
   - Phase 1: Create calculator core functions
   - Phase 2: Implement four operations
   - Phase 3: Add error handling
   - Phase 4: Write unit tests
4. **Wait for confirmation** - "Please confirm if this plan can proceed"

### After Confirmation

After user confirms, ECC will wait for user input to begin each phase.

### Tips

- Using PRD file mode可以获得更详细的规划
- Complex features recommend writing PRD first, then planning

---

## Step 2: Review with /ecc:code-review

### Input

After code is written, run:

```
/ecc:code-review
```

### Expected Output

/ecc:code-review will:

1. Collect changed files
2. Perform review across 7 categories:
   - Correctness
   - Type safety
   - Pattern compliance
   - Security
   - Performance
   - Completeness
   - Maintainability
3. Generate review report with CRITICAL/HIGH/MEDIUM/LOW levels

### Review Report Example

```
## Code Review Report

### CRITICAL
- [ ] Hardcoded credentials: line 23 in src/auth.js

### HIGH
- [ ] Function too long: lines 45-78 in src/calculator.js (34 lines)
- [ ] Missing error handling: division function in src/calculator.js

### MEDIUM
- [ ] Non-standard naming: variable `tmp` should be `temporary`

### Action Items
1. Remove hardcoded API Key
2. Refactor division function in calculator.js, add division by zero check
3. Split long functions
```

### Tips

- Must run /ecc:code-review before committing
- CRITICAL and HIGH issues must be fixed

---

## Step 3: Fix with /ecc:build-fix

### Input

If build fails, run:

```
/ecc:build-fix
```

### Expected Output

/ecc:build-fix will:

1. **Detect build system** - "Detected TypeScript project"
2. **Run build** - "Running tsc --noEmit..."
3. **Parse errors** - Grouped by file
4. **Fix incrementally** - One error at a time
5. **Verify** - Rebuild after each fix

### Example Output

```
[build-fix] Detected TypeScript project
[build-fix] Running tsc --noEmit...
[build-fix] Found 3 errors:

Error 1: src/calculator.ts:15 - Type 'string' cannot be assigned to type 'number'
Error 2: src/calculator.ts:28 - Property 'result' does not exist
Error 3: src/calculator.ts:42 - Parameter 'b' is never used

[build-fix] Starting to fix error 1...
[build-fix] Fix complete, verifying...
[build-fix] Build successful!
```

### Tips

- Build fixes are incremental, one error at a time
- Can press Ctrl+C to stop anytime

---

## Step 4: Commit Code

### Commit Flow

1. Ensure all tests pass
2. Ensure build succeeds
3. Ensure code review issues are fixed

### Git Commit Convention

ECC uses conventional commits format:

```
<type>: <description>

[optional body]
```

**Types**:
- `feat` - New feature
- `fix` - Bug fix
- `refactor` - Refactoring
- `docs` - Documentation
- `test` - Test
- `chore` - Maintenance
- `perf` - Performance optimization
- `ci` - CI configuration

### Commit Example

```
feat: Implement calculator four basic operations

- Add addition function
- Add subtraction function
- Add multiplication function
- Add division function (with division by zero check)

Fixes: Issue #123
```

### Tips

- Use `/ecc:verify quick` for pre-commit quick verification
- Large features in multiple commits, small features in one commit

---

## Complete Example

### Scenario

Implement a simple string utility function: `truncate(str, maxLength)` that truncates strings to a specified length.

### Step 1: Plan

```
/ecc:plan Implement truncate function to truncate strings to specified length
```

Output:
```
## Requirements Restatement
Implement truncate(str, maxLength) function, replacing parts exceeding maxLength with ...

## Risk Assessment
- Handling when maxLength is less than 3
- Handling when str is empty or null
- Handling Unicode characters

## Step-by-step Plan
1. Create src/string-utils.ts
2. Implement truncate function
3. Add edge case tests
4. Add JSDoc documentation

Please confirm to proceed.
```

### Step 2: Implement

(After user confirms, ECC assists with code implementation)

### Step 3: Review

```
/ecc:code-review
```

Output:
```
## Code Review Report
Status: APPROVED

### Review Results
- Correctness: Pass
- Type safety: Pass
- Pattern compliance: Pass
- Security: Pass
- Performance: Pass
- Completeness: Suggest adding more edge tests
- Maintainability: Pass

### Suggestions
- Add empty string test
- Add null value test
```

### Step 4: Build Verification

```
/ecc:build-fix
```

```
[build-fix] Detected TypeScript project
[build-fix] Running tsc --noEmit...
[build-fix] Build successful!
```

### Step 5: Commit

```
feat: Implement truncate string truncation function

- Support specifying maximum length
- Replace exceeding parts with ...
- Handle empty strings and null values
```

---


## Summary

1. **Complete workflow**: /ecc:plan → implement → /ecc:code-review → /ecc:build-fix → commit
2. **/ecc:plan** is for planning, ensuring correct direction
3. **/ecc:code-review** is for review, finding issues
4. **/ecc:build-fix** is for fixing build errors
5. **Commit** uses conventional commits format

---

## Next Steps

- After completing the course, do [exercises/Getting-Started-Exercises.md](./exercises/Getting-Started-Exercises.md)
- Proceed to Stage 2: Core Command Mastery