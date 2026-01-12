---
name: review
description: Comprehensive code review using RULE 0/1/2 hierarchy
---

# Review Skill

Structured code review that catches production risks, conformance violations, and structural issues in priority order.

## When to Use

- Before merging significant changes
- After completing implementation milestones
- When asked to review code quality
- Before refactoring (understand current issues first)

## Workflow

### Phase 1: Context Discovery

Before examining code:

1. **Find project standards**
   - Read CLAUDE.md in relevant directories
   - Note any documented patterns or conventions
   - Identify "Before creating new code, check..." sections

2. **Understand scope**
   - What is being reviewed? (PR, file, directory)
   - What changed? (if reviewing diff)
   - What should it accomplish?

### Phase 2: RULE 0 Scan (Production Reliability)

Highest priority. Block on any finding.

**Categories:**
- Security vulnerabilities (injection, auth bypass, data exposure)
- Data loss risks (unhandled errors that lose data silently)
- Unvalidated assumptions (code assumes conditions without checks)
- Resource leaks (connections, handles, memory not released)

**Method:**
1. Trace error paths: "What happens when X fails?"
2. Trace data flow: "Where does untrusted input go?"
3. Use OPEN questions: "What value does X have here?" (not "Is X safe?")

### Phase 3: RULE 1 Scan (Project Conformance)

**Requires:** Project documentation must specify the standard.

**Check against documented patterns:**
- Does code use established utilities? (query factories, mutation helpers)
- Does code follow error handling pattern?
- Does code use correct logging mechanism?

**Do NOT flag** patterns not documented. Style preferences are not conformance violations.

### Phase 4: RULE 2 Scan (Structural Quality)

Apply only after RULE 0 and RULE 1.

**Categories:**
- GOD_FUNCTION: >50 lines or >3 responsibilities
- DUPLICATE_LOGIC: Same logic in multiple places
- INCONSISTENT_ERROR_HANDLING: Mixed patterns
- DEAD_CODE: Unreachable or unused

**Skip if** project documentation permits the pattern.

## Evidence Requirements

Every finding must have:

1. **Location:** Exact file and line
2. **Observation:** What you saw (not interpreted)
3. **Consequence:** What happens if unfixed
4. **Fix:** Specific action (implementable without additional context)

**WRONG:**
```
Issue: Error handling could be improved
```

**CORRECT:**
```
Issue: user-service.ts:142 - database error returns false, caller logs "saved" but data lost
Consequence: Silent data loss, no recovery possible
Fix: Throw UserPersistenceError with original exception context
```

## Output Format

```markdown
## Review: [What was reviewed]

### Verdict: [PASS | PASS_WITH_CONCERNS | NEEDS_CHANGES]

### Standards Applied
- [List CLAUDE.md files read]
- [Or "No project docs found. RULE 0 and RULE 2 only."]

### Findings

#### [CATEGORY SEVERITY]: Title
- **Rule:** 0 | 1 | 2
- **Location:** file:line
- **Issue:** What is wrong
- **Evidence:** Concrete observation
- **Fix:** Specific action

[Repeat for each finding, ordered by severity]

### Considered But Not Flagged
- [Pattern examined but determined non-issue with rationale]

### Summary
[One paragraph: overall assessment and key concerns]
```

## Quality Checks

Before submitting review:

- [ ] Every RULE 0 finding has specific failure scenario
- [ ] Every RULE 1 finding cites specific project standard
- [ ] Every RULE 2 finding verified project docs don't permit it
- [ ] Every finding has location + evidence + fix
- [ ] No style preferences flagged as issues
- [ ] Used OPEN verification questions (not yes/no)

## Anti-Patterns

### Vague Findings
**WRONG:** "There might be issues with error handling"
**CORRECT:** "file:line - exception caught and ignored, caller receives success"

### Style as Quality
**WRONG:** "Should use arrow functions instead of function declarations"
**CORRECT:** Only flag if project documentation specifies this convention

### Missing Evidence
**WRONG:** "This could cause performance issues"
**CORRECT:** "This O(n²) loop at file:line processes [N] items, measured at [X]ms"

### Flagging Known Risks
If reviewing a plan with "Known Risks" section, those risks are **out of scope**. Do not re-flag acknowledged risks.
