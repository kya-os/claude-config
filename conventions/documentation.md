# Documentation Standards

Documentation serves LLM navigation and human understanding. Different files serve different purposes.

## File Hierarchy

### CLAUDE.md - Navigation Index

**Purpose:** Tell the LLM what exists and when to read it.
**Loads:** Automatically when entering directory.
**Format:** Tabular with What/When columns.

```markdown
# directory-name/

One sentence describing this directory's purpose.

## Files

| File | What | When to read |
|------|------|--------------|
| `foo.ts` | Handles X | When working on X |
| `bar.ts` | Manages Y | When Y behavior unclear |

## Subdirectories

| Directory | What | When to read |
|-----------|------|--------------|
| `utils/` | Shared utilities | Before creating new utility |
| `hooks/` | React hooks | When adding data fetching |
```

**Rules:**
- Pure navigation, no prose explanations
- "When to read" uses action verbs: "When working on...", "Before creating...", "When debugging..."
- Keep under 200 tokens (forces discipline)

### README.md - Invisible Knowledge

**Purpose:** Document what's NOT apparent from reading code.
**Loads:** Only when CLAUDE.md trigger indicates relevance.
**Content:** Architecture decisions, invariants, non-obvious constraints.

**Test:** If a developer could learn it by reading source files, it doesn't belong here.

**Belongs in README:**
- Why we chose X over Y
- Invariants that must hold
- Historical context affecting current design
- Non-obvious dependencies between components

**Does NOT belong:**
- How to use functions (that's in code)
- What files exist (that's in CLAUDE.md)
- API documentation (that's in code comments)

### Decision Log

Every non-trivial decision should be captured:

```markdown
## Decision Log

| Decision | Options Considered | Choice | Rationale |
|----------|-------------------|--------|-----------|
| Auth method | JWT, Sessions, OAuth | Sessions | Team familiar, NextAuth available |
| State management | Redux, Zustand, Context | Zustand | Simpler API, sufficient for scope |
```

**What qualifies as "non-trivial":**
- Choosing between libraries/frameworks
- Architectural patterns
- Tradeoffs affecting future work
- Anything where "why not the alternative?" is a reasonable question

## Comment Standards

### Inline Comments

**Purpose:** Explain WHY, not WHAT.

**WRONG:**
```typescript
// Increment counter
counter++;

// Check if user is admin
if (user.role === 'admin') {
```

**CORRECT:**
```typescript
// Compensate for 0-indexed array in 1-indexed API response
counter++;

// Admin bypass: skip rate limiting for internal tools
if (user.role === 'admin') {
```

### Temporal Contamination

Comments describing changes rather than current state become meaningless debt.

**FORBIDDEN patterns:**
- "Added in v1.2"
- "Changed from X to Y"
- "After refactor"
- "New implementation"
- "Fixed bug where..."
- "TODO: cleanup after migration"

**Why:** Future readers don't know when "after" is. Comments should describe the code as it IS.

**WRONG:** "Added retry logic after outage incident"
**CORRECT:** "Retry with exponential backoff handles transient network failures"

### Function Documentation

**Purpose:** WHAT it does + WHEN to use it.

```typescript
/**
 * Validates user input against schema and returns sanitized data.
 *
 * Use when: Processing untrusted input from API requests.
 * Do not use for: Internal function calls with validated data.
 *
 * @throws ValidationError if input fails schema
 */
function validateInput(input: unknown): ValidatedInput {
```

**Include:**
- One-line summary of behavior
- "Use when" trigger for LLM navigation
- Exceptions/errors thrown
- Non-obvious side effects

**Exclude:**
- Parameter descriptions obvious from types
- Return value obvious from return type
- Implementation details

## DRY Documentation

Before creating new code, document where to check for existing utilities:

```markdown
## Before Creating New Code

| Category | Check Location | Pattern |
|----------|----------------|---------|
| Data fetching | `lib/hooks/queries/` | Use existing query factory |
| Mutations | `lib/hooks/mutations/` | Use createMutation |
| Validation | `lib/validators/` | Reuse Zod schemas |
| Formatting | `lib/utils/format.ts` | Extend existing formatters |
```

This prevents duplicate implementations and maintains consistency.

## Token Budgets (Guidelines)

| File Type | Target | Rationale |
|-----------|--------|-----------|
| CLAUDE.md | ~200 tokens | Loads automatically; keep small |
| README.md | ~500 tokens | Loads on demand; can be more detailed |
| Function doc | ~50 tokens | Inline; should be scannable |
| Decision Log entry | ~30 tokens | Dense; tradeoffs only |

These are guidelines, not hard limits. Exceed when necessary, but question why.
