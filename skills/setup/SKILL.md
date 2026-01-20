---
name: setup
description: Initialize repository with CLAUDE.md hierarchy. Analyzes codebase complexity, identifies patterns, and generates navigation files.
---

# Setup Skill

Bootstraps a repository with CLAUDE.md navigation files. Analyzes codebase structure, identifies high-complexity areas, detects existing patterns, and generates context files for LLM navigation.

## When to Use

- **New project:** First time setting up kya-os/claude-config
- **Existing project:** Adding CLAUDE.md hierarchy to established codebase
- **Refresh:** Updating stale CLAUDE.md files after major changes
- **Single directory:** Targeted setup for specific path

## Invocation

**Full repository setup:**
```
Use your setup skill to initialize this repository
```

**Dry run (analyze only):**
```
Use your setup skill to analyze this repository (dry run)
```

**Single directory:**
```
Use your setup skill on src/lib/hooks
```

**Refresh existing:**
```
Use your setup skill to refresh CLAUDE.md files
```

---

## Workflow

### Phase 1: Discovery

**Goal:** Understand repository structure and identify setup targets.

**Actions:**
1. Detect project type (monorepo, single package, application)
2. Identify package manager (npm, pnpm, yarn, cargo, go.mod)
3. Find existing documentation (README.md, CLAUDE.md, docs/)
4. Map directory structure (max depth 4)

**Output:**
```
## Repository Profile

- **Type:** [monorepo | single-package | application]
- **Language:** [primary language]
- **Package Manager:** [detected]
- **Existing Docs:** [count] README.md, [count] CLAUDE.md
```

### Phase 2: Complexity Analysis

**Goal:** Identify directories that need CLAUDE.md files based on complexity.

**Complexity Signals:**

| Signal | Weight | Detection Method |
|--------|--------|------------------|
| File count | HIGH | >10 files in directory |
| Export count | HIGH | >5 exports from index file |
| Dependency fan-in | HIGH | Many files import from here |
| Nesting depth | MEDIUM | >2 levels of subdirectories |
| Pattern presence | MEDIUM | Factory, service, hook patterns |
| Test coverage | LOW | Has __tests__ or *.test.* files |

**Actions:**
1. Score each directory (0-100)
2. Identify top 10-15 highest complexity directories
3. Detect existing patterns and utilities
4. Find "DRY check" locations (factories, shared utilities)

**Output:**
```
## Complexity Map

| Directory | Score | Reason | Priority |
|-----------|-------|--------|----------|
| src/lib/hooks | 85 | 23 files, query factory, high fan-in | HIGH |
| src/services | 72 | 15 files, service pattern | HIGH |
| packages/core | 68 | 12 exports, used by 8 packages | HIGH |
| src/utils | 45 | 18 files, low coupling | MEDIUM |
```

### Phase 3: Pattern Detection

**Goal:** Identify reusable patterns that should be documented in CLAUDE.md.

**Pattern Types:**

| Pattern | Detection | Grep Command | Documentation Value |
|---------|-----------|--------------|---------------------|
| Factory functions | `create*`, `make*`, `build*` exports | `rg "export (function\|const) (create\|make\|build)"` | HIGH - prevents duplication |
| Query/mutation hooks | `use*Query`, `use*Mutation` | `rg "export function use\w+(Query\|Mutation)"` | HIGH - enforces consistency |
| Service classes | `*Service`, `*Manager` classes | `rg "export class \w+(Service\|Manager)"` | MEDIUM - architecture clarity |
| Validation schemas | Zod/Yup schema exports | `rg "export const \w+Schema"` | MEDIUM - reuse opportunity |
| Error handling | Custom error classes, handlers | `rg "export class \w+Error"` | MEDIUM - consistency |
| Utility functions | Format, parse, convert utils | `rg "export function (format\|parse\|convert)"` | HIGH - common DRY violations |

**Actions:**
1. Scan high-complexity directories for patterns
2. Identify "check here first" utilities
3. **Generate grep commands for each pattern category**
4. Map dependencies between directories
5. Detect naming conventions
6. **Identify directories to exclude from exploration**

**Output:**
```
## Detected Patterns

### DRY Check Locations
| Category | Location | Grep Command |
|----------|----------|--------------|
| Data fetching | src/lib/hooks/queries/ | `rg "export function use\w+Query"` |
| Mutations | src/lib/hooks/utils/ | `rg "createMutation"` |
| Formatters | src/lib/utils/ | `rg "export function format"` |

### Quick Search Commands
```bash
# Find existing hooks
rg "export function use" src/lib/hooks/

# Find existing utilities
rg "export function" src/lib/utils/

# Find service patterns
rg "export class.*Service" src/services/
```

### Conventions Detected
- Hooks: `use[Resource]()` naming
- Services: `[name].service.ts` files
- Tests: `__tests__/[name].test.ts` structure

### Exclude from Exploration
- `node_modules/` - External dependencies
- `dist/`, `build/`, `.next/` - Build artifacts
- `.turbo/`, `.cache/` - Cache directories
- `coverage/` - Test coverage
```

### Phase 4: Setup Map Generation

**Goal:** Create actionable plan for CLAUDE.md generation.

**Actions:**
1. Compile discovery + complexity + patterns into setup map
2. Prioritize directories (HIGH > MEDIUM > LOW)
3. Identify dependencies between CLAUDE.md files
4. Generate preview of what will be created

**Output:**
```
## Setup Map

### Proposed CLAUDE.md Files

| Path | Priority | Content Focus |
|------|----------|---------------|
| ./CLAUDE.md | REQUIRED | Project overview, key directories |
| packages/CLAUDE.md | HIGH | Package index, dependency order |
| src/lib/hooks/CLAUDE.md | HIGH | Query/mutation patterns, DRY refs |
| src/services/CLAUDE.md | HIGH | Service layer patterns |
| src/utils/CLAUDE.md | MEDIUM | Utility index |

### Generation Order
1. Root CLAUDE.md (depends on nothing)
2. packages/CLAUDE.md (depends on root)
3. src/lib/hooks/CLAUDE.md (depends on root)
...

Proceed with generation? [Awaiting user confirmation]
```

### Phase 5: User Confirmation

**Goal:** Get explicit approval before generating files.

**Present to user:**
- List of files to be created
- Directories that will be skipped (and why)
- Any existing CLAUDE.md files that will be updated

**Wait for:**
- Approval to proceed
- Modifications to the plan
- Request for dry-run only

### Phase 6: Generation

**Goal:** Create CLAUDE.md files according to approved plan.

**For each approved directory:**

1. Read all files in directory
2. Identify exports and public interfaces
3. Detect "when to read" triggers
4. Find relationships to other directories
5. Generate CLAUDE.md following template

**CLAUDE.md Template:**
```markdown
# [directory-name]/

[One sentence purpose derived from code analysis]

## Files

| File | What | When to read |
|------|------|--------------|
| [file.ts] | [purpose from exports/code] | [trigger condition] |

## Subdirectories

| Directory | What | When to read |
|-----------|------|--------------|
| [subdir/] | [purpose] | [trigger] |

## Before Creating New Code

| Category | Check Location | Grep Command |
|----------|----------------|--------------|
| [category] | [relative path] | `rg "[pattern]" [path]` |

## Quick Search Commands

[Generate 2-4 grep commands specific to this directory's patterns]

## Do NOT Explore

[List directories irrelevant to this context - build artifacts, deps, etc.]
```

### Phase 7: Verification

**Goal:** Ensure generated files are accurate and useful.

**Actions:**
1. Verify all file references exist
2. Check for circular dependencies in triggers
3. Validate "Before Creating" paths are correct
4. Report any issues found

**Output:**
```
## Setup Complete

### Created
- [count] CLAUDE.md files
- [count] directories documented

### Verification
- File references: [PASS/FAIL]
- Path accuracy: [PASS/FAIL]
- Pattern detection: [count] patterns documented

### Next Steps
1. Review generated CLAUDE.md files
2. Add project-specific patterns to "Before Creating" sections
3. Run `/init [path]` for any missed directories
```

---

## Modes

### Initialize (Default)

Full repository setup from scratch.

```
Use your setup skill to initialize this repository
```

### Analyze (Dry Run)

Generate setup map without creating files.

```
Use your setup skill to analyze this repository (dry run)
```

### Refresh

Update existing CLAUDE.md files based on current codebase.

```
Use your setup skill to refresh CLAUDE.md files
```

### Single Directory

Setup specific directory only.

```
Use your setup skill on src/lib/hooks
```

---

## Complexity Scoring Algorithm

```
Score = (
  (file_count > 10 ? 25 : file_count * 2.5) +
  (export_count > 5 ? 25 : export_count * 5) +
  (fan_in_count > 3 ? 20 : fan_in_count * 6) +
  (has_subdirs ? 15 : 0) +
  (has_patterns ? 10 : 0) +
  (has_tests ? 5 : 0)
)

Priority:
  HIGH: score >= 60
  MEDIUM: score >= 30
  LOW: score < 30
```

---

## Evidence Requirements

All generated content must be empirically verifiable:

| Content | Evidence Source |
|---------|-----------------|
| File purpose | Export names, function signatures |
| "When to read" | Import graph analysis |
| Patterns | Function naming, file naming conventions |
| DRY locations | High fan-in analysis |

**Do NOT generate:**
- Speculative purposes ("might be used for...")
- Undiscoverable patterns
- References to non-existent files

---

## Anti-Patterns

### Over-Documentation

**WRONG:** Create CLAUDE.md for every directory
**CORRECT:** Only document directories with complexity score >= 30

### Speculative Content

**WRONG:** "This file probably handles authentication"
**CORRECT:** "Exports: `authenticateUser()`, `validateToken()`, `refreshSession()`"

### Static Triggers

**WRONG:** "Read when working on auth"
**CORRECT:** "Read when implementing user session management"

### Missing DRY References

**WRONG:** Document structure without utility references
**CORRECT:** Include "Before Creating New Code" with specific paths

### Missing Grep Hints

**WRONG:** "Check utils/ for formatters"
**CORRECT:** `rg "export function format" lib/utils/`

---

## Token Optimization

Generated CLAUDE.md files should optimize for minimal token usage:

### Lean Content (~200 lines max)
- Tables over prose
- Grep commands over descriptions
- "Do NOT Explore" sections to prevent unnecessary reads

### Grep Hints
Every "Before Creating" section should include runnable grep commands:

```markdown
| Category | Check Location | Grep Command |
|----------|----------------|--------------|
| Formatters | `lib/utils/` | `rg "export function format"` |
| Hooks | `lib/hooks/` | `rg "export function use"` |
```

### Forbidden Directories
Include "Do NOT Explore" section in root CLAUDE.md:

```markdown
## Do NOT Explore

These directories waste context tokens:

| Directory | Reason |
|-----------|--------|
| `node_modules/` | External dependencies |
| `dist/` `build/` `.next/` | Build artifacts |
| `.turbo/` `.cache/` | Cache directories |
| `coverage/` | Test coverage reports |
| `*.min.js` `*.bundle.js` | Bundled files |
```

### When Grep Is Not Enough

For very large codebases (>500 files), consider recommending MCP semantic search:

```markdown
## Large Codebase Note

This codebase may benefit from semantic search via MCP.
See: https://zilliz.com/blog/why-im-against-claude-codes-grep-only-retrieval
```

### Session Hygiene Reminder

Include in root CLAUDE.md:

```markdown
## Session Tips

- Use `/clear` between unrelated tasks
- Use `/compact` for long debugging sessions
- One objective per session for best results
```
