---
name: review
description: Lens-based review for code, plans, and PRs with multi-perspective analysis
---

# Review Skill

Structured review that applies multiple perspectives (lenses) to catch issues that single-viewpoint review misses. Consolidates findings and surfaces conflicts for human decision.

## When to Use

- Before merging significant changes
- After completing implementation milestones
- When asked to review code quality
- When reviewing plans/TDDs before implementation
- Before refactoring (understand current issues first)

## Lens System

### Available Lenses

| Lens | File | Use When |
|------|------|----------|
| Code Quality | `references/code-quality.md` | Any code review |
| Type Parity | `references/type-parity.md` | API/protocol code |
| Product Value | `references/product-value.md` | Plans, features |
| System Design | `references/system-design.md` | Architecture decisions |
| DevX | `references/devx.md` | Developer-facing changes |
| UX | `references/ux.md` | Frontend/UI changes |

### Lens Selection Matrix

| Review Type | Load These Lenses |
|-------------|-------------------|
| **Quick** (trivial changes) | code-quality only |
| **Standard** (most PRs) | code-quality, type-parity, devx |
| Plan/TDD | product-value, system-design, devx |
| Plan with API | + type-parity |
| Plan with UI | + ux |
| PR (backend) | code-quality, devx |
| PR (API) | code-quality, type-parity, devx |
| PR (frontend) | code-quality, ux, devx |
| PR (contracts) | code-quality, type-parity, system-design |
| PR (full-stack) | All applicable |

## Workflow

### Phase 1: Context Discovery

Before examining code:

1. **Determine review type** - Plan, PR, code? Trivial or significant?
2. **Select lenses** - Use matrix above
3. **Read lens files** - Load from `references/`
4. **Find project standards** - Check CLAUDE.md in relevant directories
5. **Understand scope** - What changed? What should it accomplish?

### Phase 2: RULE 0 Scan (Production Reliability)

**Highest priority. Block on any finding.**

Apply BEFORE any lens. These are universal:

- Security vulnerabilities (injection, auth bypass, data exposure)
- Data loss risks (unhandled errors that lose data silently)
- Unvalidated assumptions (code assumes conditions without checks)
- Resource leaks (connections, handles, memory not released)

**Method:**
1. Trace error paths: "What happens when X fails?"
2. Trace data flow: "Where does untrusted input go?"
3. Use OPEN questions: "What value does X have here?" (not "Is X safe?")

### Phase 3: Lens Application

For each selected lens:

1. **Read the lens file** from `references/`
2. **Apply lens checks** to the code/plan
3. **Tag findings** with lens name: `[LENS_NAME: finding]`
4. **Note severity**: MUST, SHOULD, COULD

### Phase 4: Conflict Detection

After applying all lenses, identify conflicts:

| Lens A Says | Lens B Says | Conflict Type |
|-------------|-------------|---------------|
| Product: Add this feature | System Design: Too complex | Scope vs Complexity |
| DevX: Simpler API | Type Parity: Must match spec | Convenience vs Compliance |

**Document conflicts explicitly.** Don't resolve silently - human decides.

### Phase 5: Consolidation

Combine all findings into single output.

## Output Format

```markdown
## Review: [What was reviewed]

### Verdict: PASS | PASS_WITH_CONCERNS | NEEDS_CHANGES

### Lenses Applied
- [List lenses used and why]

### Standards Referenced
- [CLAUDE.md files read]

---

## RULE 0 Findings (Blockers)

### [CATEGORY]: Title
- **Location:** file:line
- **Issue:** What is wrong
- **Evidence:** Concrete observation
- **Fix:** Specific action

---

## Lens Findings

### [CODE_QUALITY]: Title
- **Severity:** MUST | SHOULD | COULD
- **Location:** file:line
- **Issue:** What the lens detected
- **Fix:** How to address

---

## Conflicts Detected

| Lens A | Lens B | Conflict | Recommendation |
|--------|--------|----------|----------------|
| [Lens] | [Lens] | [Description] | [Suggested resolution] |

**Human Decision Required:** [Yes/No]

---

## Summary

[One paragraph: overall assessment, key concerns, recommendation]

### Blockers (MUST fix)
- [ ] [item]

### Recommendations (SHOULD fix)
- [ ] [item]

### Suggestions (COULD improve)
- [ ] [item]
```

## Evidence Requirements

Every finding must have:

1. **Location** - Exact file and line
2. **Observation** - What you actually saw
3. **Consequence** - What happens if unfixed
4. **Fix** - Specific, actionable

## Anti-Patterns

### Missing Lens Tags
**WRONG:** "This violates DRY"
**CORRECT:** "[CODE_QUALITY] DRY violation - same validation logic in X, Y, Z"

### Silent Conflict Resolution
**WRONG:** Decide which lens "wins" without noting
**CORRECT:** Document conflict, provide recommendation, flag for human
