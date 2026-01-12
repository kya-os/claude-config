---
name: plan
description: Create implementation plans for complex features with decision capture
---

# Plan Skill

Creates structured implementation plans for complex tasks. Captures decisions, alternatives, and rationale to preserve context.

## When to Use

Use when task has:
- Multiple milestones with dependencies
- Architectural decisions requiring documentation
- Risk of "LLM-generated technical debt" without upfront thought

Skip when:
- Single-step with obvious implementation
- Quick fix or minor change
- Already well-specified by user

## Workflow

### Phase 1: Context Gathering

1. **Understand the request** - Restate in your own words
2. **Explore the codebase** - Find relevant files, existing patterns
3. **Identify constraints** - Technical limitations, project conventions
4. **Surface assumptions** - List what you're assuming, ask for confirmation

### Phase 2: Approach Design

1. **Generate options** - At least 2 approaches with tradeoffs
2. **Evaluate against constraints** - Which options are viable?
3. **Recommend** - State preference with rationale
4. **Get approval** - User confirms approach before planning details

### Phase 3: Plan Writing

Write plan to specified file with this structure:

```markdown
# Plan: [Feature Name]

## Summary
[One paragraph describing what this plan accomplishes]

## Decision Log

| Decision | Options Considered | Choice | Rationale |
|----------|-------------------|--------|-----------|
| [Key decision] | [A, B, C] | [Choice] | [Why] |

## Known Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| [Risk description] | Low/Med/High | Low/Med/High | [How to handle] |

## Milestones

### Milestone 1: [Name]
**Goal:** [What this achieves]
**Files:** [Files to modify/create]
**Dependencies:** [What must exist first]

#### Changes
[Detailed description of changes]

### Milestone 2: [Name]
...

## Testing Strategy
[How to verify this works]

## Rollback Plan
[How to undo if things go wrong]
```

### Phase 4: Review

Before finalizing:
- [ ] All non-trivial decisions have Decision Log entries
- [ ] Known Risks section acknowledges uncertainties
- [ ] Milestones have clear dependencies
- [ ] No temporal contamination in plan text

## Evidence Requirements

Plans must include concrete evidence for:

1. **Why this approach?** - Decision Log with alternatives considered
2. **Why these milestones?** - Dependencies justify ordering
3. **What could go wrong?** - Known Risks with mitigations

## Output

After creating plan:

```
## Plan Created

**File:** [path to plan file]
**Milestones:** [count]
**Key Decisions:** [count logged]
**Known Risks:** [count identified]

**Next Steps:**
1. Review plan for completeness
2. `/clear` context
3. Execute plan by milestone
```

## Execution (After /clear)

When executing a plan:

1. **Read the plan file** - Restore context
2. **Check for reconciliation** - Has code changed since plan?
3. **Execute by milestone** - One at a time, verify before proceeding
4. **Update CLAUDE.md** - If new patterns were introduced
5. **Log deviations** - If you diverged from plan, document why

## Anti-Patterns

### Planning Without Evidence
**WRONG:** "We should use Redis for caching"
**CORRECT:** "Decision: Cache layer. Options: Redis (existing infra), in-memory (simpler), none (acceptable latency?). Choice: Redis - already in stack, team knows it."

### Waterfall Plans
**WRONG:** 15-milestone plan with no checkpoints
**CORRECT:** 3-5 milestones, each independently verifiable

### Undocumented Assumptions
**WRONG:** Proceed assuming X without stating it
**CORRECT:** "Assumption: API supports batch requests. User to confirm before Milestone 2."
