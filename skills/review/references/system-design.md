# System Design Lens

**Perspective:** "Is this architecturally sound and maintainable?"

**Output Tag:** `[SYSTEM_DESIGN]`

## Checks

### Existing Patterns (MUST)

- [ ] Follows established codebase patterns
- [ ] Uses existing utilities/helpers
- [ ] No reinventing existing solutions

**Flag:** `[SYSTEM_DESIGN] Pattern: file creates new pattern when [existing] already handles this`

### YAGNI (SHOULD)

- [ ] No premature abstraction
- [ ] No unused flexibility
- [ ] Complexity justified by current requirements

**Flag:** `[SYSTEM_DESIGN] YAGNI: [abstraction] added for hypothetical future need`

### Separation of Concerns (SHOULD)

- [ ] Clear boundaries between modules
- [ ] No business logic in UI components
- [ ] Each layer has single responsibility

**Flag:** `[SYSTEM_DESIGN] Separation: file:line mixes [concern A] with [concern B]`

### Cross-Repo Impact (MUST for shared packages)

- [ ] Changes to contracts package coordinated
- [ ] Breaking changes have migration path
- [ ] Downstream consumers identified

**Flag:** `[SYSTEM_DESIGN] Cross-repo: change to [package] affects [consumers] without coordination`

### Testability (SHOULD)

- [ ] Dependencies injectable
- [ ] Side effects isolated
- [ ] External services mockable

**Flag:** `[SYSTEM_DESIGN] Testability: file:line has untestable dependency`

## KYA-OS Architecture Patterns

### Query Key Factory (agent-shield)

- [ ] Uses centralized query key factory
- [ ] Mutation uses proper invalidation
- [ ] Cache strategy matches data freshness needs

**Flag:** `[SYSTEM_DESIGN] Query pattern: file:line creates ad-hoc query key instead of using factory`

### Service Layer (know-that-ai)

- [ ] Business logic in services, not routes
- [ ] Drizzle queries in data layer
- [ ] Clean separation of concerns

**Flag:** `[SYSTEM_DESIGN] Service layer: file:line has DB query in route handler`

### Provider Pattern (xmcp-i)

- [ ] Platform-agnostic core logic
- [ ] Adapters for Node.js/Cloudflare/Vercel
- [ ] No platform-specific code in core

**Flag:** `[SYSTEM_DESIGN] Provider: file:line has platform-specific code in core package`

## Severity Guide

| Category | Severity |
|----------|----------|
| Ignores existing pattern | MUST |
| Cross-repo breaking change | MUST |
| Missing query factory | SHOULD |
| YAGNI violation | SHOULD |
| Testability issue | SHOULD |
