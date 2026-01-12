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

| Pattern | Detection | Documentation Value |
|---------|-----------|---------------------|
| Factory functions | `create*`, `make*`, `build*` exports | HIGH - prevents duplication |
| Query/mutation hooks | `use*Query`, `use*Mutation` | HIGH - enforces consistency |
| Service classes | `*Service`, `*Manager` classes | MEDIUM - architecture clarity |
| Validation schemas | Zod/Yup schema exports | MEDIUM - reuse opportunity |
| Error handling | Custom error classes, handlers | MEDIUM - consistency |
| Constants/config | UPPER_CASE exports, config files | LOW - reference value |

**Actions:**
1. Scan high-complexity directories for patterns
2. Identify "check here first" utilities
3. Map dependencies between directories
4. Detect naming conventions

**Output:**
```
## Detected Patterns

### DRY Check Locations
| Category | Location | Pattern |
|----------|----------|---------|
| Data fetching | src/lib/hooks/queries/keys.ts | Query factory |
| Mutations | src/lib/hooks/utils/mutation-factory.ts | Mutation factory |
| Validation | src/lib/validators/schemas.ts | Zod schemas |

### Conventions Detected
- Hooks: `use[Resource]()` naming
- Services: `[name].service.ts` files
- Tests: `__tests__/[name].test.ts` structure
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

| Category | Check Location | Pattern |
|----------|----------------|---------|
| [category] | [relative path] | [pattern name] |
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
