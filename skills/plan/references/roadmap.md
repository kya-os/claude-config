# Roadmap Context

> **Last Updated:** [DATE - UPDATE THIS]
> **Quarter:** Q1 2026

## Current OKRs

### O1: Ship Agent Shield v1.1
- **KR1:** Detection + blocking middleware live (Target: Jan 31)
- **KR2:** 5 beta customers using blocking (Target: Feb 15)
- **KR3:** <100ms p95 latency on detection (Target: Feb 28)

### O2: MCP-I Ecosystem Adoption
- **KR1:** 10 third-party MCP servers using MCP-I (Target: Mar 15)
- **KR2:** create-mcpi-app scaffolds all supported platforms (Target: Feb 28)
- **KR3:** Documentation coverage >90% (Target: Mar 31)

### O3: Bouncer Dashboard Polish
- **KR1:** Monitor dashboards with real data (Target: Jan 31)
- **KR2:** Tier/billing visibility complete (Target: Feb 15)
- **KR3:** <3 click onboarding flow (Target: Feb 28)

## Active Initiatives

| Initiative | Owner | Status | Dependencies |
|------------|-------|--------|--------------|
| Middleware Detection | Dylan | In Progress | xmcp-i blocking support |
| Tier Display | [Name] | In Progress | Billing API |
| Monitor Dashboards | [Name] | Completing | Analytics pipeline |

## What's IN Scope This Quarter

- Agent Shield middleware (detection + blocking)
- Bouncer dashboard data visualization
- MCP-I framework stabilization
- Tier/billing visibility (display only, not management)
- Documentation improvements

## What's OUT of Scope This Quarter

- Custom IDP support (Q2)
- Multi-tenant billing management (Q2)
- Agent marketplace (Q3+)
- Self-service tier upgrades (Q2)
- Mobile dashboard (not planned)

## Key Dependencies

```
Agent Shield Blocking
    └── requires: xmcp-i blocking support
    └── requires: Detection confidence thresholds

Tier Display
    └── requires: Billing API (external team)
    └── blocks: Warning states

Onboarding Redesign
    └── requires: Detection shipped
```

## Cross-Repo Coordination

| Change Type | Coordination Required |
|-------------|----------------------|
| New shared type | Add to @kya-os/contracts FIRST |
| API endpoint change | Update contracts + consumers together |
| Breaking change | Major version bump, migration guide |
| New package feature | Update create-mcpi-app templates |

---

**MAINTENANCE:** Update this file at the start of each quarter and when initiatives change.
