# KYA-OS Claude Config

LLM-assisted development guardrails for the KYA-OS ecosystem.

## Always-On Guardrails

These rules apply to ALL code changes. No exceptions without explicit `// REASON:` comments.

### Code Quality

| Rule | Standard | Violation |
|------|----------|-----------|
| No `any` types | Every `any` needs `// REASON:` comment | Silent type erosion |
| Function size | < 50 lines | GOD_FUNCTION |
| File size | < 300 lines | GOD_OBJECT |
| No duplication | 3+ similar blocks = extract | DRY violation |
| Single responsibility | One reason to change per unit | SOLID violation |
| No console.log | Use structured logging | Debug leak |

### Type Parity (KYA-Specific)

When touching API code or types:
- All shared types MUST come from `@kya-os/contracts` or `@kya-os/agentshield-shared`
- Never duplicate type definitions across repos
- If type doesn't exist in shared package, add it there first
- Check: Does this change require updating the shared package?

### Package Awareness (KYA-Specific)

When changing code in `packages/*`:
- Flag if change requires version bump (patch/minor/major)
- Flag if change is breaking (requires major bump)
- Flag downstream packages that import this
- Flag if templates in `create-mcpi-app` need update

### Cross-Repo Parity

When touching API endpoints:
- Request/response types must exist in `@kya-os/contracts`
- Field naming: snake_case for API, camelCase for internal
- Auth pattern: Bearer token, consistent across endpoints
- Zod schemas required for runtime validation

## Agents (Isolated Context)

Use agents when task benefits from fresh, isolated context:

| Agent | Use When |
|-------|----------|
| `debugger` | Complex bug investigation requiring clean slate |
| `developer` | Implementation from detailed spec |

## Skills (Main Context)

Use skills when task needs full conversation context:

| Skill | Use When |
|-------|----------|
| `plan` | Planning features (loads roadmap/strategy context) |
| `review` | Reviewing code/plans (applies multi-lens analysis) |
| `audit` | Detecting stale/conflicting documentation |
| `setup` | Initializing repository with CLAUDE.md hierarchy |

## Review Protocol

Before approving any plan, TDD, or PR:

1. **Check guardrails** (automatic - rules above)
2. **Load relevant skill** (`plan` for planning, `review` for review)
3. **Apply skill's workflow**
4. **Surface conflicts** for human decision

## Evidence Requirements

All conclusions require empirical evidence:

| Claim Type | Minimum Evidence |
|------------|------------------|
| Bug root cause | Specific file:line, reproduction steps |
| Refactoring benefit | Before/after metrics |
| Performance claim | Benchmarks with baseline |
| "X is better than Y" | Concrete tradeoff analysis |

## Session Optimization

| Command | When to Use | Benefit |
|---------|-------------|---------|
| `/clear` | Between unrelated tasks | Fresh context |
| `/compact` | Long sessions | Summarize and continue |

## Do NOT Explore

Skip these directories - they waste context:

- `node_modules/`, `dist/`, `build/`, `.next/`
- `.turbo/`, `.cache/`, `coverage/`
- `*.min.js`, `*.bundle.js`
