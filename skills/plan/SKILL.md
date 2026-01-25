---
name: plan
description: Create implementation plans with roadmap context and decision capture
---

# Plan Skill

Creates structured implementation plans for complex tasks. Loads business context from references and captures decisions for future context preservation.

## When to Use

Use when task has:
- Multiple milestones with dependencies
- Architectural decisions requiring documentation
- Cross-repo or cross-team implications
- Risk of "LLM-generated technical debt" without upfront thought

Skip when:
- Single-step with obvious implementation
- Quick fix or minor change
- Already well-specified by user

## Workflow

### Phase 1: Context Loading

**Before any planning, read from `references/`:**

1. **roadmap.md** - Current quarter OKRs, active initiatives
2. **strategy.md** - Product direction, what's in/out of scope

This ensures plans align with business goals. Flag misalignment early.

### Phase 2: Context Gathering

1. **Understand the request** - Restate in your own words
2. **Check roadmap alignment** - Does this fit current priorities?
3. **Explore the codebase** - Find relevant files, existing patterns
4. **Identify constraints** - Technical limitations, project conventions
5. **Surface assumptions** - List what you're assuming, ask for confirmation

### Phase 3: Approach Design

1. **Generate options** - At least 2 approaches with tradeoffs
2. **Evaluate against constraints** - Which options are viable?
3. **Check cross-repo impact** - Does this affect shared packages?
4. **Recommend** - State preference with rationale
5. **Get approval** - User confirms approach before planning details

### Phase 4: Plan Writing

Write plan with this structure:

```markdown
# Plan: [Feature Name]

## Summary
[One paragraph describing what this plan accomplishes]

## Roadmap Alignment
- **Quarter Goal:** [Which OKR this serves]
- **Initiative:** [Which active initiative, if any]
- **Dependencies:** [What must exist first]

## Decision Log

| Decision | Options Considered | Choice | Rationale |
|----------|-------------------|--------|-----------|
| [Key decision] | [A, B, C] | [Choice] | [Why] |

## Cross-Repo Impact

| Package/Repo | Impact | Action Required |
|--------------|--------|-----------------|
| @kya-os/contracts | New types needed | Add types first |

## Known Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| [Risk] | Low/Med/High | Low/Med/High | [How to handle] |

## Milestones

### Milestone 1: [Name]
**Goal:** [What this achieves]
**Files:** [Files to modify/create]
**Dependencies:** [What must exist first]

#### Changes
[Detailed description]

## Testing Strategy
[How to verify this works]

## Rollback Plan
[How to undo if things go wrong]

## Package Publishing (if applicable)
- [ ] Packages to bump: [list]
- [ ] Breaking changes: [yes/no]
- [ ] Template updates needed: [yes/no]
```

### Phase 5: Review with Lenses

Before finalizing, mentally apply review lenses:

| Lens | Check |
|------|-------|
| Roadmap | Does this align with current OKRs? |
| Product Value | Does this solve real user problems? |
| System Design | Is this architecturally sound? |
| DevX | Will future-us hate past-us? |
| Type Parity | (if API) Does this comply with contracts? |

Flag conflicts between lenses for human decision.

### Phase 6: Output

After creating plan:

```
## Plan Created

**File:** [path to plan file]
**Milestones:** [count]
**Key Decisions:** [count logged]
**Known Risks:** [count identified]
**Cross-Repo Impact:** [packages affected]

**Alignment Check:**
- [x] Fits Q[X] roadmap
- [x] No scope creep beyond initiative
- [ ] Requires [X] decision from human

**Next Steps:**
1. Review plan for completeness
2. Resolve flagged decisions
3. `/clear` context
4. Execute plan by milestone
```

## Execution (After /clear)

When executing a plan:

1. **Read the plan file** - Restore context
2. **Check for reconciliation** - Has code changed since plan?
3. **Execute by milestone** - One at a time, verify before proceeding
4. **Flag cross-repo needs** - "This milestone requires contracts update first"
5. **Update CLAUDE.md** - If new patterns were introduced
6. **Log deviations** - If you diverged from plan, document why

## Anti-Patterns

### Planning Without Business Context
**WRONG:** Jump straight to technical design
**CORRECT:** Read roadmap.md first, check alignment, then design

### Ignoring Cross-Repo Impact
**WRONG:** Plan feature without noting shared package changes
**CORRECT:** "Milestone 1 requires adding types to @kya-os/contracts first"

### Undocumented Assumptions
**WRONG:** Proceed assuming X without stating it
**CORRECT:** "Assumption: API supports batch requests. Confirm before Milestone 2."
