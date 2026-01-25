# Codebase Facts

> **Last Updated:** [DATE - UPDATE THIS]

Quick reference for working effectively in the KYA-OS ecosystem.

## Repository Map

| Repo | Purpose | Key Entry Points |
|------|---------|------------------|
| `agent-shield` | AI detection platform | `apps/web`, `packages/agentshield-*` |
| `xmcp-i` | MCP identity framework | `packages/contracts`, `packages/mcp-i-*` |
| `know-that-ai` | Agent registry | `src/app`, `src/lib` |
| `reputation-engine` | Rust reputation scoring | `crates/*`, `services/*` |

## Shared Packages (Source of Truth)

| Package | Contains | Import From |
|---------|----------|-------------|
| `@kya-os/contracts` | All shared types, Zod schemas | xmcp-i repo |
| `@kya-os/agentshield-shared` | Agent Shield types, constants | agent-shield repo |

**Rule:** Types MUST exist in shared package before use in consuming repos.

## Common Patterns

### Query Keys (agent-shield)

```typescript
// Always use the factory
import { queryKeys } from '@/lib/queries/keys'

// Good
queryKey: queryKeys.sessions.list(projectId)

// Bad - ad-hoc keys
queryKey: ['sessions', projectId]
```

### Service Layer (know-that-ai)

```typescript
// Business logic in services, not routes
// Good: src/lib/services/agent.service.ts
// Bad: Logic directly in app/api/agents/route.ts
```

### Error Handling

```typescript
// Always preserve context
throw new AppError('Failed to fetch', { cause: originalError })

// Never swallow silently
catch (e) { return false } // BAD
```

## Common Pitfalls

| Pitfall | Impact | Prevention |
|---------|--------|------------|
| Defining types locally | Type drift across repos | Check contracts first |
| `workspace:*` in published packages | Publish fails | Pre-publish validation |
| `console.log` in production | Debug noise | Use structured logger |
| Missing Zod validation on API | Runtime type errors | Always validate inputs |
| Hardcoded URLs/keys | Environment issues | Use env vars |

## Useful Commands

| Command | Purpose |
|---------|---------|
| `pnpm build` | Build all packages (turbo) |
| `pnpm test` | Run tests |
| `pnpm lint` | Lint all packages |
| `pnpm api:validate-parity` | Check API contract consistency (xmcp-i) |

## Testing Patterns

| Repo | Framework | Notes |
|------|-----------|-------|
| agent-shield | Vitest | Turbo task-based |
| know-that-ai | Jest + Playwright | Max 4 DB workers |
| xmcp-i | Vitest | API parity tests |
| reputation-engine | Rust `#[test]` | SQLx offline mode |

## Environment Setup

```bash
# All repos use pnpm
pnpm install

# Build before testing
pnpm build

# Most repos have turbo
pnpm turbo run test
```

## Key Files to Know

| File | Purpose |
|------|---------|
| `packages/contracts/src/index.ts` | All shared type exports |
| `apps/web/src/lib/queries/keys.ts` | Query key factory (agent-shield) |
| `src/lib/errors.ts` | Error classes (know-that-ai) |
| `turbo.json` | Build pipeline config |

---

**MAINTENANCE:** Update when significant patterns change or new repos are added.
