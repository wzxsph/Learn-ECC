# Testing Agents

Testing agents specialize in test-driven development, end-to-end testing, test coverage analysis, and quality assurance.

## Agent List

| Agent Name | Purpose | Model Used | Core Tools |
|------------|---------|------------|------------|
| tdd-guide | TDD test-driven development expert | sonnet | Read, Write, Edit, Bash, Grep |
| e2e-runner | End-to-end testing expert | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| mle-reviewer | ML engineering code review (training/inference/monitoring) | sonnet | Read, Grep, Glob, Bash |
| pr-test-analyzer | PR test coverage analysis | sonnet | Read, Grep, Glob, Bash |

---

## tdd-guide

### Name and Purpose
Test-driven development expert that enforces a test-first methodology. Proactively used when writing new features, fixing bugs, or refactoring code. Ensures 80%+ test coverage.

### Capabilities
- Enforce test-first methodology
- Guide Red-Green-Refactor cycles
- Ensure 80%+ test coverage
- Write comprehensive test suites (unit, integration, E2E)
- Catch edge cases before implementation

### Applicable Scenarios
- When writing new features
- When fixing bugs
- When refactoring code
- When TDD workflow is needed

### Tools Used
- Read: Read existing code
- Write: Write tests
- Edit: Modify tests and implementation
- Bash: Run test commands
- Grep: Search test patterns

### Collaboration with Other Agents
- code-reviewer reviews code after TDD cycle
- build-error-resolver fixes test build errors
- e2e-runner performs E2E testing for critical user flows

### TDD Workflow

#### 1. Write Tests First (RED)
Write failing tests that describe expected behavior.

#### 2. Run Tests - Verify it FAIL
```bash
npm test
```

#### 3. Write Minimal Implementation (GREEN)
Write only enough code to make tests pass.

#### 4. Run Tests - Verify it PASS

#### 5. Refactor (IMPROVE)
Remove duplicates, improve names, optimize - tests must stay green.

#### 6. Verify Coverage
```bash
npm run test:coverage
# Requirement: 80%+ branch, function, line, and statement coverage
```

### Must-Test Edge Cases

1. **Null/Undefined** input
2. **Empty** arrays/strings
3. **Invalid type** passed
4. **Boundary values** (min/max)
5. **Error paths** (network failure, database errors)
6. **Race conditions** (concurrent operations)
7. **Large data** (10k+ items for performance)
8. **Special characters** (Unicode, emoji, SQL characters)

### Anti-Patterns - Must Avoid

- Testing implementation details (internal state) instead of behavior
- Tests depending on each other (shared state)
- Too few assertions (pass but verify nothing)
- External dependencies not mocked (Supabase, Redis, OpenAI, etc.)

### Quality Checklist

- [ ] All public functions have unit tests
- [ ] All API endpoints have integration tests
- [ ] Critical user flows have E2E tests
- [ ] Edge cases covered (null, empty, invalid)
- [ ] Error paths tested (not just happy path)
- [ ] External dependencies mocked
- [ ] Tests are independent (no shared state)
- [ ] Assertions are specific and meaningful
- [ ] Coverage is 80%+

### v1.8 Eval-Driven TDD Appendix

Integrate eval-driven development into the TDD process:

1. Define capability + regression evals before implementation
2. Run baseline and capture failure characteristics
3. Implement minimal passing changes
4. Re-run tests and evals; report pass@1 and pass@3
5. Critical path releases should reach pass^3 stability before merging

---

## e2e-runner

### Name and Purpose
End-to-end testing expert using Vercel Agent Browser (preferred) and Playwright fallback. Proactively used for generating, maintaining, and running E2E tests.

### Capabilities
- Test journey creation - Write tests for user flows
- Test maintenance - Keep tests synchronized with UI changes
- Flaky test management - Identify and isolate unstable tests
- Test reporting - Generate HTML reports and JUnit XML
- CI/CD integration - Ensure tests run reliably in pipelines
- Screenshot/video/trace management

### Applicable Scenarios
- When critical user journeys need validation
- When end-to-end validation is needed
- When running E2E tests in CI/CD pipelines
- When integration issues are discovered

### Tools Used
- Read: Read page content
- Write: Write tests
- Edit: Modify tests
- Bash: Run Playwright/Agent Browser commands
- Grep: Search tests
- Glob: Find test files

### Collaboration with Other Agents
- tdd-guide writes unit/integration tests
- code-reviewer reviews test quality
- mle-reviewer reviews ML-related tests

### Primary Tool: Agent Browser

**Prefer Agent Browser over raw Playwright** - Semantic selectors, AI optimization, auto-waiting, built on Playwright.

```bash
# Setup
npm install -g agent-browser && agent-browser install

# Core workflow
agent-browser open https://example.com
agent-browser snapshot -i          # Get elements with refs [ref=e1]
agent-browser click @e1            # Click by ref
agent-browser fill @e2 "text"     # Fill input by ref
agent-browser wait visible @e5     # Wait for element visible
agent-browser screenshot result.png
```

### Fallback: Playwright

When Agent Browser is unavailable, use Playwright directly.

```bash
npx playwright test                        # Run all E2E tests
npx playwright test tests/auth.spec.ts     # Run specific file
npx playwright test --headed               # Visible browser
npx playwright test --debug                # Use inspector debug
npx playwright test --trace on             # Run with trace
npx playwright show-report                 # View HTML report
```

### Workflow

#### 1. Plan
- Identify critical user journeys (auth, core features, payments, CRUD)
- Define scenarios: happy path, edge cases, error cases
- Prioritize by risk: HIGH (financial, auth), MEDIUM (search, navigation), LOW (UI polish)

#### 2. Create
- Use Page Object Model (POM) pattern
- Prefer `data-testid` selectors
- Add assertions at key steps
- Capture screenshots at key moments
- Use appropriate waits (never `waitForTimeout`)

#### 3. Execute
- Run locally 3-5 times to check for flakiness
- Isolate flaky tests with `test.fixme()` or `test.skip()`
- Upload artifacts to CI

### Key Principles

- **Use semantic selectors**: `[data-testid="..."]` > CSS selectors > XPath
- **Wait for conditions, not time**: `waitForResponse()` > `waitForTimeout()`
- **Auto-waiting built in**: `page.locator().click()` waits automatically; raw `page.click()` does not
- **Isolate tests**: Each test should be independent; no shared state
- **Fail fast**: Use `expect()` assertions at every key step
- **Trace on retry**: Configure `trace: 'on-first-retry'` for debugging failures

### Flaky Test Handling

```typescript
// Isolate
test('flaky: market search', async ({ page }) => {
  test.fixme(true, 'Flaky - Issue #123')
})

// Identify flakiness
// npx playwright test --repeat-each=10
```

Common causes: Race conditions (use auto-waiting selectors), network timing (wait for response), animation timing (wait for `networkidle`).

### Success Metrics

- All critical journeys pass (100%)
- Overall pass rate > 95%
- Flakiness rate < 5%
- Test duration < 10 minutes
- Artifacts uploaded and accessible

---

## mle-reviewer

### Name and Purpose
Production machine learning engineering review expert focused on data contracts, feature pipelines, training reproducibility, offline/online evaluation, model serving, monitoring, and rollback.

### Capabilities
- Problem definition and decision quality review
- Metrics, thresholds, and error analysis review
- Data contract and leakage checks
- Training reproducibility verification
- Evaluation and promotion process review
- Serving and deployment security review
- Monitoring and incident response planning

### Applicable Scenarios
- When ML, MLOps, model training code changes
- When inference, feature store, evaluation code changes
- When making model promotion decisions

### Tools Used
- Read: Read ML code and configuration
- Grep: Search patterns
- Glob: Find files
- Bash: Run pytest, ruff, mypy

### Collaboration with Other Agents
- python-reviewer handles Python style, types, error handling
- pytorch-build-resolver handles tensor/CUDA/training failures
- database-reviewer handles feature tables, label storage
- security-reviewer handles secrets, PII, pickle security
- performance-optimizer handles latency, memory, GPU utilization
- build-error-resolver handles CI, dependencies, native extension failures
- pr-test-analyzer analyzes test coverage
- silent-failure-hunter discovers pipeline silent failures
- e2e-runner performs product flow E2E testing
- a11y-architect reviews prediction accessibility
- doc-updater updates documentation

### Key Review Areas

#### Problem Definition and Decision Quality
- Does the change start from user or system decisions, not model architecture preferences?
- Are stakeholders and failure costs clear?
- Do metric selections follow error budget?

#### Metrics, Thresholds, and Error Analysis
- Is baseline compared with current production behavior?
- Are thresholds and configuration as product decisions?
- Are false positives and false negatives directly checked?

#### Data Contracts and Leakage
- Are entity granularity, primary keys, timestamps clear?
- Do splits respect time, user/entity grouping?
- Are feature joins time-point correct?
- Are sensitive attributes excluded or handled properly?

#### Training Reproducibility
- Can training be run from code, configuration, dataset version, seed?
- Are hyperparameters, preprocessing, dependency versions recorded?
- Is randomness and GPU non-determinism intentionally handled?

#### Evaluation and Promotion
- Are metrics compared with baseline and current production model?
- Are promotion gates declared before selection?
- Do slice metrics cover important cohorts?

#### Serving and Deployment
- Are training/serving transformations shared or equivalently tested?
- Do input schemas reject stale, missing, invalid features?
- Do inference paths have timeout, resource limits, fallback logic?

#### Monitoring and Incident Response
- Does monitoring cover service health, feature drift, prediction drift?
- Do logs contain sufficient identifiers?
- Is rollback named for previous artifact, configuration, data dependencies?

### Common Blockers

- Random train/test splits on time-related or user-related data
- Feature generation using fields unavailable at prediction time
- Offline metric improvements with critical slice regressions
- Training preprocessing manually copied to serving code
- Model version not in prediction logs

### Approval Criteria

- **APPROVE**: No critical/high MLE risks, relevant tests or evaluation gates passing
- **APPROVE WITH WARNINGS**: Only medium issues with clear follow-up actions
- **BLOCK**: Any possible leakage, non-reproducible promotion, unsafe serving behavior, missing production deployment rollback, sensitive data exposure, or critical evaluation gaps

---

## pr-test-analyzer

### Name and Purpose
Review Pull Request test coverage quality and completeness, emphasizing behavioral coverage and real bug prevention.

### Capabilities
- Identify changed code mapping
- Verify behavioral coverage
- Evaluate test quality
- Discover coverage gaps

### Applicable Scenarios
- During PR review
- When evaluating test coverage
- When verifying test sufficiency

### Tools Used
- Read: Read changed code and tests
- Grep: Search tests
- Glob: Find test files
- Bash: Run test commands

### Collaboration with Other Agents
- tdd-guide improves tests
- e2e-runner provides end-to-end coverage
- code-reviewer performs code quality review

### Analysis Process

#### 1. Identify Changed Code
- Map changed functions, classes, and modules
- Locate corresponding tests
- Identify new untested code paths

#### 2. Behavioral Coverage
- Check each feature has tests
- Verify edge cases and error paths
- Ensure important integrations are covered

#### 3. Test Quality
- Prioritize meaningful assertions over no-throw checks
- Flag unstable patterns
- Check isolation and test name clarity

#### 4. Coverage Gaps
Rate by impact:
- critical (critical)
- important (important)
- nice-to-have (nice-to-have)

### Output Format

1. Coverage summary
2. Key gaps
3. Improvement suggestions
4. Positive observations
[Return to Agent Index](../README.md)