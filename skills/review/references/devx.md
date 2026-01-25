# DevX Lens

**Perspective:** "Will future developers hate this?"

**Output Tag:** `[DEVX]`

## Checks

### Error Messages (MUST)

- [ ] Errors say what went wrong
- [ ] Errors say how to fix it
- [ ] Errors include context (file, line, values)

**Flag:** `[DEVX] Error message: file:line shows "[error]" - not actionable`

### Documentation (SHOULD)

- [ ] Complex logic has comments
- [ ] Public APIs have JSDoc/TSDoc
- [ ] Non-obvious decisions have `// REASON:` comments

**Flag:** `[DEVX] Documentation: [function] lacks documentation for non-obvious behavior`

### Breaking Changes (MUST)

- [ ] Breaking changes documented
- [ ] Migration path provided
- [ ] Deprecation warnings before removal

**Flag:** `[DEVX] Breaking: [change] breaks existing usage without migration guide`

### Consistency (SHOULD)

- [ ] Follows existing patterns in codebase
- [ ] API shape matches similar APIs
- [ ] No surprising behavior

**Flag:** `[DEVX] Consistency: [pattern] differs from established [existing pattern]`

### Configuration (SHOULD)

- [ ] Sensible defaults
- [ ] No hardcoded values that should be config
- [ ] Environment-specific values in env vars

**Flag:** `[DEVX] Config: file:line hardcodes [value] that should be configurable`

## KYA-OS Specific Patterns

### Package Development

- [ ] README with usage examples
- [ ] TypeScript types exported
- [ ] Changelog maintained

**Flag:** `[DEVX] Package: [package] missing README/types/changelog`

### create-mcpi-app Templates

- [ ] Templates work out of the box
- [ ] All platforms have equivalent features
- [ ] Clear comments explaining structure

**Flag:** `[DEVX] Template: [template] fails on fresh install`

### CLI Tools

- [ ] Help text for all commands
- [ ] Consistent flag naming (--verbose, --dry-run)
- [ ] Exit codes meaningful (0 success, 1 error)

**Flag:** `[DEVX] CLI: [command] missing help text or uses non-standard flags`

### Debug Experience

- [ ] Errors traceable to source
- [ ] Structured logging with context
- [ ] No silent failures

**Flag:** `[DEVX] Debug: file:line error loses context/stack trace`

## Severity Guide

| Category | Severity |
|----------|----------|
| Breaking without migration | MUST |
| Cryptic error messages | MUST |
| Template fails on install | MUST |
| Missing docs for public API | SHOULD |
| Inconsistent with patterns | SHOULD |
