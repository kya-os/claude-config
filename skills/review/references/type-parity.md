# Type Parity Lens

**Perspective:** "Does this comply with cross-repo contracts and type safety?"

**Output Tag:** `[TYPE_PARITY]`

## When to Apply

- Files in `packages/contracts/**`
- Files in `packages/mcp-i*/**`
- Files in `packages/agentshield*/**`
- Files matching `**/api/**/route.ts`
- Any code touching shared types or API contracts

## Checks

### Type Source (MUST)

- [ ] Types imported from `@kya-os/contracts`
- [ ] Types imported from `@kya-os/agentshield-shared`
- [ ] No local type definitions that duplicate shared packages

**Flag:** `[TYPE_PARITY] Type source: file:line defines type locally that exists in @kya-os/contracts`

### Schema Validation (MUST)

- [ ] Zod schemas exist for all API inputs
- [ ] Schemas imported from contracts package
- [ ] Runtime validation before processing

**Flag:** `[TYPE_PARITY] Schema: file:line accepts input without Zod validation`

### Naming Conventions (SHOULD)

- [ ] API fields use `snake_case`
- [ ] Internal code uses `camelCase`
- [ ] Conversion happens at API boundary

**Flag:** `[TYPE_PARITY] Naming: API at file:line uses camelCase, should be snake_case`

### Authentication Pattern (MUST)

- [ ] Uses `Authorization: Bearer ${token}` format
- [ ] Token validation before processing

**Flag:** `[TYPE_PARITY] Auth: file:line uses non-standard auth pattern`

### Backward Compatibility (MUST for existing endpoints)

- [ ] New fields are optional
- [ ] Removed fields are deprecated first
- [ ] Breaking changes documented

**Flag:** `[TYPE_PARITY] Breaking change: file:line removes required field without deprecation`

### Package Publishing (MUST before release)

- [ ] No `workspace:*` references in published packages
- [ ] Version bump appropriate for change type
- [ ] Downstream consumers identified

**Flag:** `[TYPE_PARITY] Publishing: workspace reference found in package.json`

## Cross-Repo Checklist

When touching shared types:

1. Does this type exist in `@kya-os/contracts`? If not, add it there first.
2. Does this change affect `agent-shield`, `xmcp-i`, or `know-that-ai`?
3. Is this a breaking change? If yes, major version bump required.
4. Do `create-mcpi-app` templates need updating?

## Severity Guide

| Category | Severity |
|----------|----------|
| Missing type from contracts | MUST |
| Missing Zod validation | MUST |
| Non-standard auth | MUST |
| Breaking change undocumented | MUST |
| Workspace reference in release | MUST |
| Naming convention violation | SHOULD |
