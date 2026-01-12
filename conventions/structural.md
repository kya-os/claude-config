# Structural Quality Standards

Code structure patterns that affect maintainability. These are RULE 2 concerns - apply only after RULE 0 (production reliability) and RULE 1 (project conformance) are satisfied.

## Severity Definitions

| Severity | Meaning | Action |
|----------|---------|--------|
| MUST | Unrecoverable risk, knowledge loss | Block until fixed |
| SHOULD | Maintainability debt | Fix before merge |
| COULD | Improvement opportunity | Fix if convenient |

## Structural Categories

### GOD_FUNCTION (SHOULD)

**Definition:** Function with >50 lines or >3 distinct responsibilities.

**Detection:**
- Multiple levels of abstraction in one function
- Multiple "sections" separated by comments
- Difficult to name without "and"

**Fix:** Extract into focused functions, each with single responsibility.

**Exception:** Procedural scripts, configuration, test setup may legitimately be long.

### GOD_OBJECT (SHOULD)

**Definition:** Class/module with >500 lines or >7 public methods.

**Detection:**
- Multiple unrelated concerns
- Methods that don't use most class state
- Difficult to describe purpose in one sentence

**Fix:** Split by responsibility. Each class should have one reason to change.

### DUPLICATE_LOGIC (SHOULD)

**Definition:** Same logic in multiple places (not just similar code).

**Detection:**
- Copy-paste with minor modifications
- Multiple functions doing essentially the same thing
- Same bug would need fixing in multiple places

**Fix:** Extract shared logic. Create utility functions or base classes.

**Note:** Similar-looking code that serves different purposes is NOT duplication.

### INCONSISTENT_ERROR_HANDLING (SHOULD)

**Definition:** Different error patterns in the same codebase.

**Examples:**
- Some functions throw, others return null
- Some use error codes, others use exceptions
- Some log errors, others swallow them

**Fix:** Establish and document error handling pattern. Apply consistently.

### DEAD_CODE (COULD)

**Definition:** Code that cannot be reached or is never used.

**Detection:**
- Unreachable branches
- Unused exports
- Commented-out code left for "reference"

**Fix:** Delete it. Version control preserves history.

### CONVENTION_VIOLATION (SHOULD)

**Definition:** Code that violates documented project patterns.

**Requires:** Project documentation must specify the convention.

**Examples:**
- Using console.log when logger service exists
- Hardcoding query keys when factory exists
- Creating new utility when existing one covers the case

**Fix:** Follow documented pattern or update documentation if pattern should change.

## What NOT to Flag

### Style Preferences

**NOT a finding:**
- Using for-loop instead of .map()
- Using ternary instead of if/else
- Variable naming that follows language conventions

**Why:** These are style preferences, not structural quality issues.

### Equivalent Implementations

**NOT a finding:**
- `dict(zip(keys, values))` vs dict comprehension
- `Array.from()` vs spread operator
- Promise.all vs for-await-of (when equivalent)

**Why:** Functionally equivalent. No maintainability difference.

### Permitted Patterns

If project documentation explicitly permits a pattern, do NOT flag it as structural violation.

**Example:** If README says "notification handlers may be monolithic for traceability", do not flag 80-line notification handler as GOD_FUNCTION.

## Application Order

1. **Check RULE 0** - Is there a production reliability risk? (MUST severity)
2. **Check RULE 1** - Does project documentation permit this? (Read CLAUDE.md)
3. **Check RULE 2** - Does this match a structural category? (SHOULD/COULD)

Never flag RULE 2 if RULE 0 problem exists in same code path.
Never flag RULE 2 if project documentation permits the pattern.

## Evidence Requirement

Every structural finding must have:

1. **Specific location** - File and line number
2. **Concrete observation** - What you saw (not interpreted)
3. **Measurable impact** - Why this hurts maintainability

**WRONG:** "This code is hard to read"
**CORRECT:** "user-service.ts:50-180 is 130 lines with 4 distinct responsibilities: validation (50-70), fetching (71-100), transformation (101-140), persistence (141-180). Each should be separate function."
