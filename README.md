# KYA-OS Claude Config

Opinionated LLM-assisted development guardrails. Clone into any repository's `.claude/` directory.

## Philosophy

LLM-assisted code rots faster than hand-written code. Technical debt accumulates because:
1. The LLM doesn't know what it doesn't know
2. You're moving too fast to notice
3. Decisions aren't captured, so context is lost
4. "Feels right" replaces "I have evidence"

This config treats that as an engineering problem, not a tooling problem.

## Quick Start

### Installation

```bash
# Clone into your project's .claude directory
git clone https://github.com/kya-os/claude-config.git .claude

# Or add to existing .claude
git clone https://github.com/kya-os/claude-config.git /tmp/kya-claude
cp -r /tmp/kya-claude/* .claude/
rm -rf /tmp/kya-claude
```

### First-Time Setup

After installation, start Claude Code and run:

```
Use your setup skill to initialize this repository
```

This will:
1. Analyze your codebase structure and complexity
2. Identify key directories needing CLAUDE.md files
3. Detect existing patterns and utilities
4. Generate a setup map for your approval
5. Create CLAUDE.md files for approved directories

### Per-Directory Setup

For targeted setup of a specific directory:

```
/init src/lib/hooks
```

Or interactively:

```
Use your setup skill on src/lib/hooks
```

---

## Core Principles

### 1. Evidence Over Intuition

Every claim requires empirical proof. No "seems like" or "feels right."

| Claim | Required Evidence |
|-------|-------------------|
| Bug root cause | 10+ debug outputs, 3+ test inputs |
| "X is better than Y" | Concrete tradeoff analysis |
| Performance improvement | Before/after benchmarks |
| Refactoring benefit | Measurable metrics |

### 2. Decision Preservation

Capture WHY decisions were made, not just WHAT was built.

Every non-trivial decision needs:
- Options considered
- Choice made
- Rationale (why this, why not alternatives)

### 3. Context Hygiene

Minimal, just-in-time information. CLAUDE.md files are navigation indexes, not documentation dumps.

### 4. Review Before Commit

Quality gates catch mistakes before they compound:
- RULE 0: Production reliability (MUST fix)
- RULE 1: Project conformance (SHOULD fix)
- RULE 2: Structural quality (COULD fix)

---

## Installation Options

### Option 1: New Project (Recommended)

```bash
cd your-project
git clone https://github.com/kya-os/claude-config.git .claude
```

Then in Claude Code:
```
Use your setup skill to initialize this repository
```

### Option 2: Existing .claude Directory

```bash
cd your-project

# Backup existing config
mv .claude .claude-backup

# Clone fresh
git clone https://github.com/kya-os/claude-config.git .claude

# Merge your existing skills/commands
cp -r .claude-backup/skills/* .claude/skills/ 2>/dev/null
cp -r .claude-backup/commands/* .claude/commands/ 2>/dev/null
cp .claude-backup/settings.local.json .claude/ 2>/dev/null
```

### Option 3: Global Configuration

```bash
# For all projects (Claude Code reads ~/.claude/)
git clone https://github.com/kya-os/claude-config.git ~/.claude
```

### Option 4: As Git Submodule

```bash
git submodule add https://github.com/kya-os/claude-config.git .claude
git commit -m "Add kya-os claude-config"
```

---

## Directory Structure

```
.claude/
├── CLAUDE.md              # Root navigation index
├── agents/                # Agent behavior definitions
│   ├── quality-reviewer.md   # RULE 0/1/2 review hierarchy
│   ├── developer.md          # Implementation with DRY checks
│   └── debugger.md           # Evidence-based debugging
├── conventions/           # Standards and rules
│   ├── evidence.md           # Empirical proof requirements
│   ├── documentation.md      # CLAUDE.md/README.md standards
│   └── structural.md         # Code quality patterns
├── skills/               # Multi-step workflows
│   ├── setup/               # Repository initialization
│   ├── plan/                # Implementation planning
│   └── review/              # Code review workflow
├── commands/             # Slash commands
│   └── init.md              # Directory initialization
└── templates/            # Reusable file templates
```

---

## Usage

### Setup & Initialization

**Full repository setup:**
```
Use your setup skill to initialize this repository
```

**Single directory setup:**
```
/init src/lib/hooks
```

**Analyze without generating:**
```
Use your setup skill to analyze this repository (dry run)
```

### Planning Complex Features

```
Use your plan skill to create an implementation plan for [feature]
```

Creates structured plan with:
- Decision Log (WHY decisions were made)
- Known Risks with mitigations
- Milestones with dependencies
- Testing strategy

### Code Review

```
Use your review skill on [file/PR/directory]
```

Reviews against:
- RULE 0: Production reliability (MUST)
- RULE 1: Project conformance (SHOULD)
- RULE 2: Structural quality (COULD)

### Debugging

```
Use your debugger agent to investigate [bug description]
```

Systematic evidence gathering with:
- Minimum 10 debug outputs
- 3+ test inputs
- Open verification questions
- Clean codebase on exit

---

## What Gets Generated

### Root CLAUDE.md

Created at project root with:
- Project overview
- Tech stack summary
- Key directory index
- "Before creating new code" checklist

### Directory CLAUDE.md Files

Created in high-complexity directories with:
- Purpose of directory
- File index with "when to read" triggers
- Subdirectory index
- Existing patterns to reuse

### Example Output

After running setup on a TypeScript monorepo:

```
your-project/
├── CLAUDE.md                    # Project overview
├── packages/
│   ├── CLAUDE.md               # Package index, dependency order
│   ├── core/CLAUDE.md          # Core patterns, exports
│   └── api/CLAUDE.md           # API conventions, endpoints
├── apps/
│   └── web/
│       ├── CLAUDE.md           # App structure
│       └── lib/
│           ├── hooks/CLAUDE.md    # Query/mutation patterns
│           └── services/CLAUDE.md # Service layer patterns
└── .claude/                    # This config
```

---

## Customization

### Add Project-Specific Patterns

After setup, enhance generated CLAUDE.md files:

```markdown
# src/hooks/

React hooks for data fetching.

## Before Creating New Code

| Category | Check Location | Pattern |
|----------|----------------|---------|
| Queries | `queries/keys.ts` | Use query factory |
| Mutations | `utils/mutation-factory.ts` | Use createMutation |
```

### Add Domain-Specific Skills

Create new skills in `skills/[skill-name]/`:

```
skills/
└── your-skill/
    ├── CLAUDE.md      # Navigation index
    └── SKILL.md       # Workflow definition
```

### Extend Conventions

Add project-specific rules to `conventions/`:

```markdown
# conventions/your-project.md

## API Response Format
All endpoints return { data, error, meta } structure.

## Error Codes
Use codes from lib/errors/codes.ts, do not create new ones.
```

---

## Maintenance

### Keeping CLAUDE.md Files Updated

After significant changes:
```
Use your setup skill to refresh CLAUDE.md files
```

### Periodic Audit

Check for stale, conflicting, or redundant documentation:
```
Use your audit skill to check documentation accuracy
```

The audit skill detects:
- **Staleness:** Files referenced in CLAUDE.md that no longer exist
- **Conflicts:** Same pattern documented differently in multiple places
- **Redundancy:** Duplicate content that violates DRY
- **Incompleteness:** High-complexity directories missing "Before Creating" sections

Fix issues interactively:
```
Use your audit skill to fix documentation issues
```

### Adding New Directories

When you add new feature directories:
```
/init src/new-feature
```

---

## What This Prevents

- Publishing without testing
- Losing context on why decisions were made
- Duplicate implementations (DRY enforcement)
- Silent data loss from swallowed errors
- Style preferences treated as quality issues
- Conclusions without evidence
- Context loss between sessions

## What This Enables

- Automatic codebase context for LLMs
- Structured planning before implementation
- Evidence-based debugging
- Consistent code review
- Preserved decision rationale
- Clear quality priorities (RULE 0 > 1 > 2)
- Scalable documentation that stays current

---

## Updating

```bash
cd .claude
git pull origin main
```

Or if using submodule:
```bash
git submodule update --remote .claude
```

---

## License

MIT - Use, modify, and distribute freely.

## Credits

Built for [KYA-OS](https://github.com/kya-os) projects.
Inspired by [solatis/claude-config](https://github.com/solatis/claude-config).
