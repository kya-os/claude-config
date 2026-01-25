# Product Strategy Context

> **Last Updated:** [DATE - UPDATE THIS]

## Mission

**KYA-OS:** Enable trusted AI agent interactions through identity, reputation, and access control.

## Product Suite

### Agent Shield
**Purpose:** Detect and protect against unauthorized AI agent access.
**Target User:** Website owners who want visibility/control over AI traffic.
**Value Prop:** "Know when AI is accessing your site. Control what it can do."

### Agent Bouncer
**Purpose:** Gate access to resources based on agent identity and reputation.
**Target User:** API/service owners who want to authorize legitimate agents.
**Value Prop:** "Only verified, reputable agents get through."

### MCP-I Framework (@kya-os/*)
**Purpose:** Protocol and tooling for agent identity and delegation.
**Target User:** MCP server developers who want to add identity.
**Value Prop:** "Add agent authentication to your MCP server in minutes."

## Strategic Principles

### 1. Developer Experience First
- Onboarding < 5 minutes for basic setup
- Clear error messages with actionable fixes
- Comprehensive docs with working examples

### 2. Start Simple, Add Complexity
- Pixel (visibility) → Middleware (control) → Full Bouncer (auth)
- Free tier for small usage → Paid for scale

### 3. Protocol Over Product
- MCP-I as open standard, not proprietary lock-in
- Shared packages (@kya-os/*) enable ecosystem

### 4. Trust Through Transparency
- Show users exactly what agents are doing
- Clear audit trail of agent actions

## Technical Principles

### Type Safety
- All shared types in @kya-os/contracts
- Zod schemas for runtime validation
- No `any` types without explicit justification

### API Design
- RESTful endpoints for dashboard/external
- MCP protocol for agent communication
- Consistent error format across all APIs

### Cross-Repo Consistency
- Shared packages are source of truth
- Breaking changes require coordination
- Version bumps follow semver strictly

## What We Don't Do

- **Consumer-facing products** - We're B2B/developer focused
- **General bot detection** - We focus specifically on AI agents
- **Human identity** - We're agent identity, not user auth
- **Content moderation** - We control access, not content

---

**MAINTENANCE:** Update when strategy changes. Review quarterly.
