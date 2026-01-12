---
name: audit
description: Detect outdated, conflicting, or redundant documentation. Enforces DRY/SOLID for docs.
---

# Audit Skill

Validates CLAUDE.md hierarchy for accuracy, consistency, and redundancy. Detects documentation drift from code reality.

## When to Use

- **Periodic maintenance:** Monthly or after major refactors
- **Before releases:** Ensure docs match shipped code
- **After setup:** Verify generated CLAUDE.md files
- **When confused:** If Claude gives wrong guidance, docs may be stale

## Invocation

**Full audit:**
```
Use your audit skill to check documentation accuracy
```

**Specific directory:**
```
Use your audit skill on src/lib/hooks
```

**Fix mode (interactive):**
```
Use your audit skill to fix documentation issues
```

---

## Audit Categories

### 1. Staleness Detection

**Goal:** Find CLAUDE.md content that no longer matches code.

**Checks:**

| Check | Method | Severity |
|-------|--------|----------|
| Missing files | File referenced in CLAUDE.md doesn't exist | HIGH |
| Missing exports | Documented function/class not exported | HIGH |
| New files undocumented | Files exist but not in CLAUDE.md | MEDIUM |
| Renamed files | Old name in docs, new name in filesystem | HIGH |
| Moved files | Path in docs doesn't match actual location | HIGH |

**Detection:**
```
For each file reference in CLAUDE.md:
  1. Check if file exists at stated path
  2. If file has exports, verify documented exports exist
  3. Flag mismatches

For each file in directory:
  1. Check if file is documented in CLAUDE.md
  2. Flag undocumented files (except generated/config)
```

### 2. Conflict Detection

**Goal:** Find contradictory documentation across files.

**Checks:**

| Check | Method | Severity |
|-------|--------|----------|
| Duplicate guidance | Same pattern documented differently in two places | HIGH |
| Contradictory triggers | "When to read" conflicts between files | MEDIUM |
| Inconsistent naming | Same concept called different names | MEDIUM |
| Circular references | A says "see B", B says "see A" | LOW |

**Detection:**
```
Build pattern registry from all CLAUDE.md files:
  - Pattern name → [locations where documented]
  - "Check here first" → [locations pointing there]

Flag:
  - Same pattern name, different descriptions
  - Same location, different pattern names
  - Contradictory "when to read" for same directory
```

### 3. Redundancy Detection (DRY for Docs)

**Goal:** Find documentation that repeats or could be consolidated.

**Checks:**

| Check | Method | Severity |
|-------|--------|----------|
| Duplicate content | Same text in multiple CLAUDE.md files | MEDIUM |
| Over-documented | CLAUDE.md for trivial directory (<3 files, no patterns) | LOW |
| Redundant with code | Docs restate what code comments say | LOW |
| Copy-paste patterns | Identical "Before Creating" sections | MEDIUM |

**Detection:**
```
For each CLAUDE.md:
  1. Hash content sections
  2. Compare against other CLAUDE.md files
  3. Flag >70% similarity

For each directory with CLAUDE.md:
  1. Count files and exports
  2. If <3 files and no detected patterns, flag as over-documented
```

### 4. Completeness Check (SOLID for Docs)

**Goal:** Ensure each CLAUDE.md has single responsibility and complete coverage.

**Checks:**

| Check | Method | Severity |
|-------|--------|----------|
| Missing "Before Creating" | High-complexity dir without DRY guidance | HIGH |
| No "when to read" triggers | Files listed without actionable triggers | MEDIUM |
| Mixed concerns | CLAUDE.md covers unrelated directories | MEDIUM |
| Orphan directories | Subdirectory not indexed by parent | MEDIUM |

**Detection:**
```
For each CLAUDE.md:
  1. Check all subdirectories are indexed
  2. Check all file entries have "when to read"
  3. If complexity score >50, require "Before Creating" section
```

---

## Workflow

### Phase 1: Scan

**Actions:**
1. Find all CLAUDE.md files in repository
2. Parse each into structured data (files, subdirs, patterns)
3. Build cross-reference map

**Output:**
```
## Documentation Inventory

- CLAUDE.md files found: [count]
- Directories covered: [count]
- Patterns documented: [count]
- "Before Creating" sections: [count]
```

### Phase 2: Validate

**Actions:**
1. Run staleness checks (file existence, export verification)
2. Run conflict checks (cross-reference patterns)
3. Run redundancy checks (similarity analysis)
4. Run completeness checks (coverage analysis)

**Output:**
```
## Audit Findings

### HIGH Severity
| Issue | Location | Details |
|-------|----------|---------|
| MISSING_FILE | src/lib/CLAUDE.md:5 | References `utils.ts` but file is `utils/index.ts` |
| STALE_EXPORT | src/hooks/CLAUDE.md:8 | Documents `useAuth` but export is `useAuthentication` |

### MEDIUM Severity
| Issue | Location | Details |
|-------|----------|---------|
| UNDOCUMENTED_FILE | src/lib/hooks/ | `useNewFeature.ts` not in CLAUDE.md |
| DUPLICATE_PATTERN | src/CLAUDE.md, lib/CLAUDE.md | Both document "query factory" differently |

### LOW Severity
| Issue | Location | Details |
|-------|----------|---------|
| OVER_DOCUMENTED | src/types/ | 2 files, no patterns, CLAUDE.md unnecessary |
```

### Phase 3: Report

**Summary with actionable next steps:**

```
## Audit Summary

| Category | HIGH | MEDIUM | LOW |
|----------|------|--------|-----|
| Staleness | 2 | 3 | 0 |
| Conflicts | 1 | 1 | 0 |
| Redundancy | 0 | 2 | 1 |
| Completeness | 1 | 2 | 0 |

## Recommended Actions

1. **Fix immediately (HIGH):**
   - Update src/lib/CLAUDE.md: utils.ts → utils/index.ts
   - Update src/hooks/CLAUDE.md: useAuth → useAuthentication
   - Add "Before Creating" to packages/core/CLAUDE.md

2. **Fix soon (MEDIUM):**
   - Add useNewFeature.ts to src/lib/hooks/CLAUDE.md
   - Consolidate query factory docs (keep in src/CLAUDE.md, reference from lib/)

3. **Consider (LOW):**
   - Remove src/types/CLAUDE.md (low value)
```

### Phase 4: Fix (Interactive)

If invoked with fix mode:

```
Use your audit skill to fix documentation issues
```

**For each HIGH/MEDIUM issue:**
1. Show current state
2. Show proposed fix
3. Ask for approval
4. Apply fix or skip

---

## Evidence Requirements

All findings must be empirically verified:

| Finding Type | Required Evidence |
|--------------|-------------------|
| Missing file | `ls` or glob confirms file doesn't exist |
| Stale export | grep/read confirms export name mismatch |
| Duplicate content | Actual text comparison with similarity % |
| Undocumented file | File exists AND not in CLAUDE.md |

**Never flag based on inference.** Only flag what can be verified.

---

## Integration with Setup

After running setup skill:
```
Use your audit skill to verify generated documentation
```

This validates that setup produced accurate CLAUDE.md files.

---

## Anti-Patterns

### False Positives

**WRONG:** Flag every file not in CLAUDE.md
**CORRECT:** Only flag source files; ignore generated, config, test files

### Over-Correction

**WRONG:** Suggest removing all "redundant" docs
**CORRECT:** Consolidate to single source of truth, reference from others

### Style Nitpicking

**WRONG:** Flag inconsistent markdown formatting
**CORRECT:** Focus on semantic accuracy, not presentation
