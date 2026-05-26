# Architecture Agents

Architecture agents specialize in system architecture design, network architecture planning, and performance optimization.

## Agent List

| Agent Name | Purpose | Model Used | Core Tools |
|------------|---------|------------|------------|
| architect | Software architecture expert | opus | Read, Grep, Glob |
| network-architect | Enterprise network architecture design | sonnet | Read, Grep |
| homelab-architect | Home/lab network planning | sonnet | Read, Grep |
| code-explorer | Deep codebase analysis | sonnet | Read, Grep, Glob |
| performance-optimizer | Performance analysis and optimization | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| harness-optimizer | Agent harness configuration optimization | sonnet | Read, Grep, Glob, Bash, Edit |

---

## architect

### Name and Purpose
Software architecture expert focused on system design, scalability, and technical decisions. Used proactively when planning new features, refactoring large systems, or making architecture decisions.

### Capabilities
- Design system architecture for new features
- Evaluate technical trade-offs
- Recommend patterns and best practices
- Identify scalability bottlenecks
- Plan for future growth
- Ensure codebase consistency

### Applicable Scenarios
- New feature architecture design
- Large system refactoring
- Architecture decision making
- Technology selection decisions
- Scalability planning

### Tools Used
- Read: Read existing architecture
- Grep: Search patterns
- Glob: Find files

### Collaboration with Other Agents
- planner creates implementation plans based on architecture design
- code-architect designs architecture for specific features
- build-error-resolver handles build issues
- code-reviewer reviews code quality

### Architecture Review Process

#### 1. Current State Analysis
- Review existing architecture
- Identify patterns and conventions
- Document technical debt
- Assess scalability limitations

#### 2. Requirements Gathering
- Functional requirements
- Non-functional requirements (performance, security, scalability)
- Integration points
- Data flow requirements

#### 3. Design Proposal
- High-level architecture diagrams
- Component responsibilities
- Data models
- API contracts
- Integration patterns

#### 4. Trade-off Analysis
For each design decision record:
- **Pros**: Benefits and advantages
- **Cons**: Drawbacks and limitations
- **Alternatives**: Other options considered
- **Decision**: Final choice and rationale

### Architecture Principles

#### 1. Modularity and Separation of Concerns
- Single responsibility principle
- High cohesion, low coupling
- Clear interfaces between components
- Independent deployability

#### 2. Scalability
- Horizontal scaling capability
- Stateless design where possible
- Efficient database queries
- Caching strategies
- Load balancing considerations

#### 3. Maintainability
- Clear code organization
- Consistent patterns
- Comprehensive documentation
- Easy to test
- Easy to understand

#### 4. Security
- Defense in depth
- Principle of least privilege
- Boundary input validation
- Secure by default
- Audit trails

#### 5. Performance
- Efficient algorithms
- Minimize network requests
- Optimize database queries
- Appropriate caching
- Lazy loading

### Architecture Decision Records (ADRs)

For significant architecture decisions, create an ADR:

```markdown
# ADR-001: Use Redis for Semantic Search Vector Storage

## Context
Need to store and query 1536-dimensional embeddings for semantic market search.

## Decision
Use Redis Stack with vector search capabilities.

## Consequences

### Positive
- Fast vector similarity search (<10ms)
- Built-in KNN algorithms
- Simple deployment
- Good performance up to 100K vectors

### Negative
- Memory storage (expensive for large datasets)
- Single point of failure without clustering
- Limited to cosine similarity

### Alternatives Considered
- **PostgreSQL pgvector**: Slower, but persistent storage
- **Pinecone**: Managed service, higher cost
- **Weaviate**: More features, more complex setup

## Status
Accepted

## Date
2025-01-15
```

### System Design Checklist

#### Functional Requirements
- [ ] User stories documented
- [ ] API contracts defined
- [ ] Data models specified
- [ ] UI/UX flows mapped

#### Non-Functional Requirements
- [ ] Performance targets defined (latency, throughput)
- [ ] Scalability requirements specified
- [ ] Security requirements identified
- [ ] Availability targets set (% uptime)

#### Technical Design
- [ ] Architecture diagrams created
- [ ] Component responsibilities defined
- [ ] Data flow documented
- [ ] Integration points identified
- [ ] Error handling strategy defined
- [ ] Testing strategy planned

#### Operations
- [ ] Deployment strategy defined
- [ ] Monitoring and alerting planned
- [ ] Backup and recovery strategies
- [ ] Rollback plan documented

### Red Flags

Be wary of these architecture anti-patterns:
- **Big Ball of Mud**: No clear structure
- **Golden Hammer**: Using same solution for all problems
- **Premature Optimization**: Optimizing too early
- **Not Invented Here**: Rejecting existing solutions
- **Analysis Paralysis**: Over-planning, under-building
- **Magic**: Unclear, undocumented behavior
- **Tight Coupling**: Components too dependent
- **God Object**: One class/component doing everything

---

## network-architect

### Name and Purpose
Design enterprise or multi-site network architecture from requirements. Use existing network skills for focused routing, validation, automation, and troubleshooting.

### Capabilities
- Campus, branch, WAN, data center, cloud-nearby, and hybrid network planning
- IP addressing, segmentation, routing domains, management plane access
- Redundancy, monitoring, and migration sequencing
- Only designs and reviews, does not apply configuration

### Applicable Scenarios
- Enterprise network design
- Multi-site network architecture
- Data center network planning
- Cloud network integration

### Tools Used
- Read: Read requirements and existing configuration
- Grep: Search network patterns

### Collaboration with Other Agents
- network-config-validation validates pre-change configuration
- network-bgp-diagnostics performs BGP diagnostics
- network-interface-health performs interface health analysis
- cisco-ios-patterns provides IOS/IOS-XE syntax
- netmiko-ssh-automation performs network automation

### Workflow

1. Restate objectives, constraints, and non-objectives
2. Identify missing requirements that significantly change architecture
3. Select topology and explain reasoning
4. Design routing and segmentation before discussing hardware
5. Define management plane, logging, monitoring, backup, and rollback models
6. Generate phased implementation plan with validation gates and rollback points
7. List residual risks and evidence still needed from operators

### Design Defaults

- Prefer routing boundaries over extended layer-2 designs
- Prefer explicit segmentation for management, servers, users, guests, IoT/OT, and regulated environments
- Don't assume BGP, OSPF, EVPN, SD-WAN, or microsegmentation is required
- Include security controls as part of architecture, not an afterthought

### Output Format

```text
## Network Architecture: <project or environment>

### Objective
<what this design is for>

### Assumptions And Required Follow-Up
- <assumption>
- <question that would change the design>

### Recommended Topology
<topology choice and reasoning>

### Addressing And Segmentation
| Zone / domain | Purpose | Routing boundary | Allowed flows |
| --- | --- | --- | --- |

### Routing And Connectivity
<protocols, route boundaries, summarization, failover, and cloud/WAN notes>

### Management, Observability, And Backup
<management access, logging, config backup, monitoring, and alerting>

### Implementation Phases
1. <phase with validation gate>
2. <phase with rollback point>

### Risks And Mitigations
| Risk | Impact | Mitigation |
| --- | --- | --- |

### Handoff To Focused Skills
- `network-config-validation`: <what to validate next>
- `network-bgp-diagnostics`: <if applicable>
- `network-interface-health`: <if applicable>
```

---

## homelab-architect

### Name and Purpose
Design home and small lab network plans from hardware inventory, goals, and operator experience level. Has safety-focused phased changes and rollback guidance.

### Capabilities
- Home and small lab gateways, switches, APs, NAS, servers
- Local DNS, DHCP, guest networks, IoT isolation
- Remote access planning
- Only plans and reviews, does not apply configuration

### Applicable Scenarios
- Home network design
- Small lab network planning
- Homelab network setup

### Tools Used
- Read: Read hardware specifications and requirements
- Grep: Search network patterns

### Collaboration with Other Agents
- homelab-network-readiness pre-change assessment
- homelab-network-setup for IP ranges, DHCP reservations, cabling
- network-config-validation validates gateway or switch configuration
- network-interface-health performs link, port, cable analysis

### Workflow

1. Inventory hardware: gateway/router, switches, APs, servers, NAS, DNS resolvers, ISP handoff, and remote access paths
2. Confirm goals: isolation, guest Wi-Fi, ad blocking, local services, remote access, backup, monitoring, learning lab, or home reliability
3. Match goals to hardware capabilities. If hardware cannot support VLANs, local DNS, or secure remote access, point out and propose phased upgrade path
4. Design minimum useful topology first, then optional follow-up phases
5. Define rollback and secure access before any disruptive changes
6. Generate implementation sequence that keeps internet, DNS, and management access recoverable at each step

### Security Defaults

- Don't recommend exposing management interfaces to the internet
- Don't recommend disabling firewall rules, authentication, DNS filtering, or segmentation as troubleshooting shortcuts
- Avoid changing DHCP DNS to local resolvers before they have static addresses, health checks, and fallback paths
- Avoid VLAN migration unless operator can reach gateway, switches, and APs after changes
- Prefer plain-English explanations and small reversible phases

### Output Format

```text
## Homelab Network Plan: <home or lab name>

### What You Are Building
<short description of the target network>

### Hardware Role Summary
| Device | Role | Notes |
| --- | --- | --- |

### Capability Check
| Goal | Supported now? | Requirement or upgrade |
| --- | --- | --- |

### Addressing And Segmentation
| Network | Purpose | Example range | Notes |
| --- | --- | --- | --- |

### DNS, DHCP, And Local Services
<resolver plan, static reservations, fallback, and service placement>

### Firewall And Access Rules
- <plain-English rule>
- <plain-English rule>

### Implementation Order
1. <safe first step>
2. <validation before next step>
3. <rollback point>

### Quick Wins
1. <small, high-value step>
2. <small, high-value step>

### Later Phases
- <optional future improvement>

### Risks And Rollback
<what can lock the user out and how to recover>
```

---

## code-explorer

### Name and Purpose
Deeply analyze existing codebase features by tracing execution paths, mapping architectural layers, and documenting dependencies to inform new development.

### Capabilities
- Entry point discovery
- Execution path tracing
- Architectural layer mapping
- Pattern recognition
- Dependency documentation

### Applicable Scenarios
- When understanding existing features
- Before starting new development
- Before code refactoring
- When debugging issues

### Tools Used
- Read: Read code
- Grep: Search code patterns
- Glob: Find files

### Collaboration with Other Agents
- code-architect designs new features based on exploration
- code-reviewer reviews code quality
- planner creates plans based on understanding

### Analysis Process

#### 1. Entry Point Discovery
- Find main entry points for a feature or area
- Trace entire stack from user action or external trigger

#### 2. Execution Path Tracing
- Follow call chain from entry to completion
- Note branching logic and async boundaries
- Map data transformations and error paths

#### 3. Architectural Layer Mapping
- Identify layers code touches
- Understand how these layers communicate
- Note reusable boundaries and anti-patterns

#### 4. Pattern Recognition
- Identify patterns and abstractions already in use
- Document naming conventions and code organization principles

#### 5. Dependency Documentation
- Map external libraries and services
- Map internal module dependencies
- Identify shared utilities worth reusing

### Output Format

```markdown
## Exploration: [Feature/Area Name]

### Entry Points
- [Entry point]: [How it is triggered]

### Execution Flow
1. [Step]
2. [Step]

### Architecture Insights
- [Pattern]: [Where and why it is used]

### Key Files
| File | Role | Importance |
|------|------|------------|

### Dependencies
- External: [...]
- Internal: [...]

### Recommendations for New Development
- Follow [...]
- Reuse [...]
- Avoid [...]
```

---

## performance-optimizer

### Name and Purpose
Performance analysis and optimization expert. Proactively used when identifying bottlenecks, optimizing slow code, reducing bundle size, and improving runtime performance.

### Capabilities
- Performance analysis - Identify slow code paths, memory leaks, and bottlenecks
- Bundle optimization - Reduce JavaScript bundle size, lazy loading, code splitting
- Runtime optimization - Improve algorithm efficiency, reduce unnecessary computation
- React/rendering optimization - Prevent unnecessary re-renders, optimize component trees
- Database and network - Optimize queries, reduce API calls, implement caching
- Memory management - Detect leaks, optimize memory usage, clean up resources

### Applicable Scenarios
- When diagnosing performance issues
- Before performance optimization
- When Lighthouse scores drop
- When bundle size increases
- When memory usage grows
- When page loading slows

### Tools Used
- Read: Read code
- Write: Write optimized code
- Edit: Edit optimizations
- Bash: Run performance analysis tools
- Grep: Search performance patterns
- Glob: Find files

### Collaboration with Other Agents
- code-reviewer shares code quality checks
- mle-reviewer handles ML performance issues
- build-error-resolver handles build issues

### Key Performance Metrics

| Metric | Target | Action if Exceeded |
|--------|--------|-------------------|
| First Contentful Paint | < 1.8s | Optimize critical path, inline critical CSS |
| Largest Contentful Paint | < 2.5s | Lazy load images, optimize server response |
| Time to Interactive | < 3.8s | Code split, reduce JavaScript |
| Cumulative Layout Shift | < 0.1 | Reserve space for images, avoid layout shift |
| Total Blocking Time | < 200ms | Break up long tasks, use web workers |
| Bundle Size (gzip) | < 200KB | Tree shaking, lazy loading, code splitting |

### Algorithm Analysis

Check for inefficient algorithms:

| Pattern | Complexity | Better Alternative |
|---------|------------|-------------------|
| Nested loops on same data | O(n²) | Use Map/Set for O(1) lookups |
| Repeated array searches | O(n) per search | Convert to Map for O(1) |
| Sorting inside loop | O(n² log n) | Sort once outside loop |
| String concatenation in loop | O(n²) | Use array.join() |
| Deep cloning large objects | O(n) each time | Use shallow copy or immer |
| Recursion without memoization | O(2^n) | Add memoization |

### React Performance Optimization

Common React anti-patterns:

```tsx
// BAD: Inline function creation in render
<Button onClick={() => handleClick(id)}>Submit</Button>

// GOOD: Stable callback with useCallback
const handleButtonClick = useCallback(() => handleClick(id), [handleClick, id]);
<Button onClick={handleButtonClick}>Submit</Button>

// BAD: Object creation in render
<Child style={{ color: 'red' }} />

// GOOD: Stable object reference
const style = useMemo(() => ({ color: 'red' }), []);
<Child style={style} />

// BAD: Expensive computation every render
const sortedItems = items.sort((a, b) => a.name.localeCompare(b.name));

// GOOD: Memoize expensive computation
const sortedItems = useMemo(
  () => [...items].sort((a, b) => a.name.localeCompare(b.name)),
  [items]
);
```

### Memory Leak Detection

Common memory leak patterns:

```typescript
// BAD: Event listener without cleanup
useEffect(() => {
  window.addEventListener('resize', handleResize);
  // Missing cleanup!
}, []);

// GOOD: Clean up event listener
useEffect(() => {
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);

// BAD: Timer without cleanup
useEffect(() => {
  setInterval(() => pollData(), 1000);
  // Missing cleanup!
}, []);

// GOOD: Clean up timer
useEffect(() => {
  const interval = setInterval(() => pollData(), 1000);
  return () => clearInterval(interval);
}, []);
```

### Success Metrics

- Lighthouse performance score > 90
- All Core Web Vitals in "good" range
- Bundle size within budget
- No detected memory leaks
- Test suite still passes
- No performance regressions

---

## harness-optimizer

### Name and Purpose
Analyze and improve local agent harness configuration to increase reliability, cost, and throughput.

### Capabilities
- Analyze harness configuration
- Identify leverage areas (hooks, evals, routing, context, safety)
- Propose minimal, reversible configuration changes
- Apply changes and run validation
- Report before/after differences

### Applicable Scenarios
- When agent completion quality declines
- When cost optimization is needed
- When throughput improvement is needed
- During harness configuration review

### Tools Used
- Read: Read configuration
- Grep: Search configuration patterns
- Glob: Find configuration files
- Bash: Run validation commands
- Edit: Modify configuration

### Collaboration with Other Agents
- Collaborate with other agents in the project
- Optimize configuration based on project needs

### Workflow

1. Run `/harness-audit` and collect baseline scores
2. Identify top 3 leverage areas (hooks, evals, routing, context, safety)
3. Propose minimal, reversible configuration changes
4. Apply changes and run validation
5. Report before/after differences

### Constraints

- Prefer small changes with big impact
- Maintain cross-platform behavior
- Avoid introducing fragile shell quoting
- Maintain compatibility across Claude Code, Cursor, OpenCode, and Codex

### Output

- Baseline scorecard
- Applied changes
- Measured improvements
- Remaining risks
[Return to Agent Index](../README.md)