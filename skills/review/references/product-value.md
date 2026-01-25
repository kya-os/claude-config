# Product Value Lens

**Perspective:** "Does this solve real user problems and align with product goals?"

**Output Tag:** `[PRODUCT_VALUE]`

## When to Apply

- Planning new features
- Reviewing PRDs/TDDs
- Features touching user-facing functionality

## Checks

### User Story Clarity (SHOULD)

- [ ] Clear "As a [user], I want [goal], so that [benefit]"
- [ ] Problem being solved is explicit
- [ ] Success criteria defined

**Flag:** `[PRODUCT_VALUE] User story: No clear user or problem statement`

### Roadmap Alignment (MUST)

- [ ] Fits a current quarter OKR
- [ ] Within scope of active initiative
- [ ] Not explicitly out-of-scope

**Flag:** `[PRODUCT_VALUE] Roadmap: Feature not aligned with current goals`

### Scope Assessment (SHOULD)

- [ ] Minimum viable scope identified
- [ ] Nice-to-haves separated from must-haves
- [ ] No feature creep beyond original request

**Flag:** `[PRODUCT_VALUE] Scope creep: [feature] expands beyond original ask`

### Edge Cases (SHOULD)

- [ ] Error states considered
- [ ] Empty states considered
- [ ] Loading states considered

**Flag:** `[PRODUCT_VALUE] Edge case: No handling for [scenario]`

## KYA-OS Specific Checks

### Target User Match

- [ ] Agent Shield: Website owners wanting AI visibility/control
- [ ] Bouncer: API owners wanting agent authorization
- [ ] MCP-I: Developers adding identity to MCP servers

**Flag:** `[PRODUCT_VALUE] Target mismatch: Feature serves [X] but product targets [Y]`

### Value Proposition Alignment

- [ ] Agent Shield: "Know when AI is accessing your site"
- [ ] Bouncer: "Only verified agents get through"
- [ ] MCP-I: "Add agent auth in minutes"

**Flag:** `[PRODUCT_VALUE] Value drift: Feature doesn't support core value prop`

## Severity Guide

| Category | Severity |
|----------|----------|
| Not on roadmap | MUST |
| Contradicts strategy | MUST |
| Target user mismatch | MUST |
| No user story | SHOULD |
| Scope creep | SHOULD |
