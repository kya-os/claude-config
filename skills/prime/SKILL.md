---
name: prime
description: Prime agent context with roadmap, strategy, and codebase patterns. Run at session start.
---

# Prime Skill

Loads business context and validates freshness before starting work. Run this at the beginning of each session to ensure you have accurate, up-to-date context.

## When to Use

- **Start of every session** - Before any meaningful work
- **After `/clear`** - When context was reset
- **Before planning** - Ensures roadmap awareness
- **When switching contexts** - Moving between repos/features

## Workflow

### Phase 1: Load Business Context

Read and internalize:

1. **`references/roadmap.md`** (via plan skill references)
   - Current quarter OKRs
   - Active initiatives and owners
   - What's in/out of scope

2. **`references/strategy.md`** (via plan skill references)
   - Product mission and principles
   - Target users per product
   - Technical principles

3. **`references/codebase-facts.md`** (this skill's references)
   - Key patterns and conventions
   - Common pitfalls to avoid
   - Useful commands and shortcuts

### Phase 2: Validate Roadmap Health

Check for staleness signals:

| Signal | Severity | Action |
|--------|----------|--------|
| Initiative `Complete` without `Completion PR` | Warning | Flag for cleanup |
| Initiative past `Target` date, still `In Progress` | Warning | Ask if status changed |
| All initiatives `Complete` | Error | Quarter needs refresh |
| `Last Updated` > 30 days | Warning | Confirm still accurate |
| Initiative in both "In Scope" and "Out of Scope" | Error | Contradiction - clarify |
| Active initiative with no `Owner` | Warning | Assign owner |

### Phase 3: Validate Strategy Consistency

Check for contradictions:

- Does any active initiative contradict "What We Don't Do"?
- Are target users consistent across products?
- Do technical principles conflict with active work?

### Phase 4: Load Codebase Context

For the current working directory:

1. Read root `CLAUDE.md` if present
2. Note key patterns and conventions
3. Identify "Before Creating" checklists
4. Note any repo-specific guardrails

### Phase 5: Output Primed Status

```markdown
## Session Primed

**Quarter:** Q[X] 2026
**Last Roadmap Update:** [date]

### Current Focus
- **O1:** [Primary objective]
- **Active Initiative:** [Most relevant to this session]

### Roadmap Health
- [ ] All initiatives have owners
- [ ] No past-due items without updates
- [ ] No completed items pending cleanup
- [x] Strategy and roadmap aligned

### Warnings (if any)
- [Warning 1]
- [Warning 2]

### Codebase Context
- **Repo:** [current repo]
- **Key Patterns:** [from CLAUDE.md]
- **Watch Out For:** [common pitfalls]

### Ready to Work
[Confirm ready or list blockers]
```

## Handling Validation Failures

### Stale Roadmap

If roadmap appears stale:

```
⚠️ Roadmap may be outdated:
- "Middleware Detection" target was Jan 31, still showing "In Progress"
- Last updated: [date]

Before proceeding:
1. Is this initiative complete? Update status and add Completion PR
2. Is target date extended? Update target
3. Is this blocked? Add note explaining blocker

Would you like to update the roadmap now, or proceed with current context?
```

### Contradictions Found

If contradictions exist:

```
⚠️ Contradiction detected:
- Strategy says "No consumer-facing products"
- But initiative "Mobile Dashboard" is active

This needs resolution before planning work in this area.
Options:
1. Remove initiative (out of scope)
2. Update strategy (scope changed)
3. Clarify distinction (not actually consumer-facing)

Which applies?
```

## Quick Prime (Skip Validation)

For quick context loading without full validation:

```
/prime --quick
```

Loads context but skips health checks. Use when you're confident roadmap is current.

## Integration with Other Skills

- **Plan skill** calls prime automatically if not already primed
- **Review skill** references primed context for product-value lens
- **Audit skill** can run prime validation as part of documentation audit

## Anti-Patterns

### Skipping Prime
**WRONG:** Jump into coding without context
**CORRECT:** `/prime` at session start, then work

### Ignoring Warnings
**WRONG:** See stale roadmap warning, proceed anyway
**CORRECT:** Address warnings or explicitly acknowledge ("proceeding despite stale roadmap because X")

### Priming Without Reading
**WRONG:** Run `/prime`, ignore output, start working
**CORRECT:** Read the primed status, note current focus and warnings
