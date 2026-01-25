# KYA-OS Claude Config

LLM-assisted development guardrails for the KYA-OS ecosystem.

## Installation

### New Project

```bash
git clone https://github.com/kya-os/claude-config.git .claude
```

### Existing .claude Directory

```bash
# Backup existing config
mv .claude .claude-backup

# Clone fresh
git clone https://github.com/kya-os/claude-config.git .claude

# Merge your existing skills/commands
cp -r .claude-backup/skills/* .claude/skills/ 2>/dev/null
cp -r .claude-backup/commands/* .claude/commands/ 2>/dev/null
cp .claude-backup/settings.local.json .claude/ 2>/dev/null
```

### Global (All Projects)

```bash
git clone https://github.com/kya-os/claude-config.git ~/.claude
```

### Git Submodule

```bash
git submodule add https://github.com/kya-os/claude-config.git .claude
```

## First-Time Setup

After installation, in Claude Code:

```
Use your setup skill to initialize this repository
```

This analyzes your codebase and generates CLAUDE.md navigation files.

## What's Included

### Always-On Guardrails

Rules that apply to every interaction (in root CLAUDE.md):
- No `any` types without justification
- Type parity with `@kya-os/contracts`
- Package awareness for version bumps
- Cross-repo coordination rules

### Skills

| Skill | Command | Purpose |
|-------|---------|---------|
| `prime` | `/prime` | **Run first** - loads context, validates roadmap freshness |
| `plan` | `Use your plan skill` | Create implementation plans with roadmap context |
| `review` | `Use your review skill` | Multi-lens code review (6 perspectives) |
| `setup` | `Use your setup skill` | Initialize repository with CLAUDE.md hierarchy |
| `audit` | `Use your audit skill` | Detect stale/conflicting documentation |

### Agents

| Agent | Purpose |
|-------|---------|
| `debugger` | Systematic bug investigation with clean slate |
| `developer` | Implementation from detailed specs |

### Commands

| Command | Purpose |
|---------|---------|
| `/init [path]` | Generate CLAUDE.md for a specific directory |

## Key Features

### Lens-Based Code Review

The review skill applies multiple perspectives:

| Lens | Catches |
|------|---------|
| Code Quality | Type safety, DRY, SOLID violations |
| Type Parity | Missing contracts, schema validation |
| Product Value | Roadmap alignment, scope creep |
| System Design | Architecture patterns, cross-repo impact |
| UX | Loading/error/empty states, accessibility |
| DevX | Error messages, breaking changes, docs |

### Roadmap-Aware Planning

The plan skill loads business context before planning:
- `references/roadmap.md` - Current OKRs and initiatives
- `references/strategy.md` - Product direction and scope

Plans flag misalignment with quarterly goals before technical design.

### Cross-Repo Coordination

KYA-specific rules for the ecosystem:
- Types from `@kya-os/contracts` or `@kya-os/agentshield-shared`
- Version bump awareness for packages
- Template updates for `create-mcpi-app`

## Customization

### Update Roadmap/Strategy

Edit these files to match your current priorities:
- `skills/plan/references/roadmap.md`
- `skills/plan/references/strategy.md`

### Add Custom Lenses

Create new lens files in `skills/review/references/`:

```markdown
# Custom Lens

**Perspective:** "What this lens checks"
**Output Tag:** `[CUSTOM]`

## Checks
- [ ] Check 1
- [ ] Check 2
```

Then reference it in `skills/review/SKILL.md`.

## Updating

```bash
cd .claude && git pull origin main
```

## License

MIT

## Credits

Built for [KYA-OS](https://github.com/kya-os) projects.
