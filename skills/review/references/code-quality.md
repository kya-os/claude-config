# Code Quality Lens

**Perspective:** "Does this code meet quality standards?"

**Output Tag:** `[CODE_QUALITY]`

## Checks

### Type Safety (MUST)

- [ ] No `any` types without `// REASON:` comment
- [ ] No type assertions (`as`) without justification
- [ ] Function parameters and returns typed

**Flag:** `[CODE_QUALITY] Type safety: any type at file:line without justification`

### Function Size (SHOULD)

- [ ] Functions < 50 lines
- [ ] Functions have single responsibility
- [ ] Can describe function without "and"

**Flag:** `[CODE_QUALITY] GOD_FUNCTION: file:fn is N lines with X responsibilities`

### File Size (SHOULD)

- [ ] Files < 300 lines
- [ ] Files have cohesive purpose

**Flag:** `[CODE_QUALITY] GOD_OBJECT: file is N lines, consider splitting`

### DRY Principle (SHOULD)

- [ ] No copy-paste code blocks
- [ ] Similar logic extracted to utilities
- [ ] Same bug wouldn't need fixing in multiple places

**Flag:** `[CODE_QUALITY] DUPLICATE_LOGIC: same pattern in X, Y, Z - extract to utility`

### SOLID Principles (SHOULD)

- [ ] Single Responsibility - one reason to change
- [ ] Open/Closed - extend without modifying
- [ ] Dependency Inversion - depend on abstractions

**Flag:** `[CODE_QUALITY] SOLID violation: [principle] at file:line`

### Error Handling (MUST)

- [ ] Errors not swallowed silently
- [ ] Error context preserved (cause chaining)
- [ ] Consistent error patterns

**Flag:** `[CODE_QUALITY] Silent failure: file:line catches error and returns false`

### Logging (SHOULD)

- [ ] No console.log (use structured logging)
- [ ] Appropriate log levels

**Flag:** `[CODE_QUALITY] Logging: console.log at file:line - use logger service`

## Severity Guide

| Category | Severity |
|----------|----------|
| Type safety violation | MUST |
| Silent error swallowing | MUST |
| GOD_FUNCTION/OBJECT | SHOULD |
| DRY violation | SHOULD |
| Console.log usage | SHOULD |
