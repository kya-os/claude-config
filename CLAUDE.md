# KYA-OS Claude Config

LLM-assisted development guardrails. Clone from [github.com/kya-os/claude-config](https://github.com/kya-os/claude-config).

## First-Time Setup

```
Use your setup skill to initialize this repository
```

This analyzes codebase complexity and generates CLAUDE.md navigation files.

## Principles

1. **Evidence over intuition** - Require empirical proof before conclusions
2. **Decision preservation** - Capture WHY, not just WHAT
3. **Context hygiene** - Minimal, just-in-time information
4. **Review before commit** - Quality gates catch mistakes early

## Directory Structure

| Directory | Purpose | When to read |
|-----------|---------|--------------|
| `agents/` | Agent behavior definitions | Invoking specialized agents |
| `conventions/` | Standards and rules | Understanding project rules |
| `skills/` | Multi-step workflows | Complex tasks requiring structure |
| `commands/` | Slash commands | Quick operations |
| `templates/` | Reusable file templates | Creating new files |

## Commands

| Command | Purpose |
|---------|---------|
| `/init [path]` | Generate CLAUDE.md for a directory |

## Skills

| Skill | Purpose |
|-------|---------|
| `setup` | Initialize repository with CLAUDE.md hierarchy |
| `audit` | Detect stale, conflicting, or redundant documentation |
| `plan` | Create implementation plans with Decision Log |
| `review` | Code review with RULE 0/1/2 priority |

## Agents

| Agent | Use When |
|-------|----------|
| `quality-reviewer` | Review code for production risks |
| `developer` | Implement from specs |
| `debugger` | Systematic bug investigation |

## Quick Reference

### Before Writing Code
1. Check if similar code exists (DRY)
2. Read relevant CLAUDE.md files
3. For non-trivial changes: `Use your plan skill`

### During Implementation
- Follow project patterns documented in CLAUDE.md hierarchy
- No console.log (use structured logging)
- No hardcoded values that should be configurable

### Before Committing
- [ ] Ran tests
- [ ] No temporal contamination in comments
- [ ] Decision Log entries for non-obvious choices
- [ ] Updated CLAUDE.md if new patterns introduced

## Evidence Requirements

All conclusions require empirical evidence:

| Claim Type | Minimum Evidence |
|------------|------------------|
| Bug root cause | 10+ debug outputs, 3+ test inputs |
| Refactoring benefit | Before/after metrics |
| Performance claim | Benchmarks with baseline |
| "X is better than Y" | Concrete tradeoff analysis |
