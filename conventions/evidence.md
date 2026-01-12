# Evidence Standards

All claims require empirical evidence. Intuition is a starting point, not a conclusion.

## Core Principle

**Never trust "feels right" or "seems like"**. Require concrete, verifiable observations.

## Evidence Requirements by Claim Type

### Bug Root Cause

| Requirement | Minimum |
|-------------|---------|
| Debug outputs | 10+ statements with values |
| Test inputs | 3+ different scenarios |
| Observation type | Direct (not inferred) |

**WRONG:** "The bug is probably in the auth module because that's where similar bugs were before."
**CORRECT:** "Debug output at auth.ts:142 shows token=null. Output at auth.ts:89 shows token='valid'. Invalidation occurs between 89-142."

### Performance Claim

| Requirement | Minimum |
|-------------|---------|
| Baseline measurement | Before change |
| Post-change measurement | After change |
| Sample size | 3+ runs, report variance |

**WRONG:** "This change makes it faster."
**CORRECT:** "Before: 450ms avg (±12ms, n=5). After: 280ms avg (±8ms, n=5). 38% improvement."

### "X is Better Than Y"

| Requirement | Minimum |
|-------------|---------|
| Criteria defined | What "better" means |
| Both options evaluated | Same criteria |
| Tradeoffs acknowledged | Where each wins |

**WRONG:** "React is better than Vue for this project."
**CORRECT:** "For our team's TypeScript expertise and existing component library: React saves ~2 weeks. Vue would require learning curve but has better bundle size. Given timeline, React wins on velocity; Vue wins on performance."

### Refactoring Benefit

| Requirement | Minimum |
|-------------|---------|
| Before metrics | Lines, complexity, coupling |
| After metrics | Same measurements |
| Functional equivalence | Tests pass |

**WRONG:** "This refactor improves readability."
**CORRECT:** "Before: 3 functions, 180 lines, cyclomatic complexity 12. After: 5 functions, 95 lines, complexity 4. Same 23 tests pass."

## Open vs Closed Questions

Research shows open questions have ~70% accuracy vs ~17% for yes/no questions on the same facts. LLMs tend to agree with assertions regardless of truth.

### Verification Questions

**WRONG (closed):**
- "Is the value null?" → Model agrees regardless
- "Did the function fail?" → Confirmation bias
- "Would this cause a race condition?" → Leading

**CORRECT (open):**
- "What is the value?" → Forces factual recall
- "What did the function return?" → Specific observation
- "What is the execution order between threads?" → Concrete sequence

### Decision Validation

**WRONG:** "Is this the right approach?"
**CORRECT:** "What are the alternatives? What does each cost? What does each enable?"

## Evidence Hierarchy

From strongest to weakest:

1. **Observed behavior** - Actual output, logs, measurements
2. **Reproducible test** - Can be verified independently
3. **Code analysis** - Static inspection with specific citations
4. **Documentation claim** - What docs say (may be outdated)
5. **Intuition/experience** - Starting point only, must be validated

## Anti-Patterns

### Argument from Authority
**WRONG:** "The senior engineer said to do it this way."
**CORRECT:** "The senior engineer recommended X because [specific technical reason]. Validating: [evidence]."

### Argument from Popularity
**WRONG:** "Everyone uses this library."
**CORRECT:** "This library has [specific capability] we need. Alternatives [A, B] lack [specific feature]."

### Premature Conclusion
**WRONG:** "After quick investigation, the bug is in the cache."
**CORRECT:** "After 12 debug outputs across 3 test scenarios, evidence points to cache invalidation at cache.ts:89 where TTL check uses milliseconds but expiry is in seconds."

### Unfalsifiable Claims
**WRONG:** "This code could potentially cause issues."
**CORRECT:** "This code causes [specific issue] when [specific condition] as shown by [specific evidence]."

## Template: Evidence Block

When making claims, use this structure:

```
## Claim
[Specific, falsifiable statement]

## Evidence
1. [Observation]: [file:line] shows [value/behavior]
2. [Observation]: [file:line] shows [value/behavior]
3. [Observation]: [file:line] shows [value/behavior]

## Alternatives Considered
- [Alternative A]: Ruled out because [evidence]
- [Alternative B]: Ruled out because [evidence]

## Confidence
[HIGH/MEDIUM/LOW] - [Why this confidence level]
```
