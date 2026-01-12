---
description: Initialize CLAUDE.md for a directory. Usage: /init [path]
---

# Directory Initialization

Generate a CLAUDE.md file for the specified directory.

## Input

**Path provided:** `$ARGUMENTS`

If no path provided, use current working directory.

## Workflow

### 1. Validate Path

- Confirm directory exists
- Check if CLAUDE.md already exists (offer to update or skip)

### 2. Analyze Directory

Gather empirical data:

**Files:**
- List all source files (exclude node_modules, dist, .git)
- Extract exports from each file
- Identify file purposes from exports/function names

**Patterns:**
- Detect naming conventions (services, hooks, utils)
- Find factory functions (create*, make*, build*)
- Identify shared utilities (high import count)

**Relationships:**
- What imports FROM this directory? (fan-out)
- What does this directory import? (dependencies)
- Subdirectory purposes

### 3. Generate CLAUDE.md

Use this structure:

```markdown
# [directory-name]/

[One sentence purpose - derived from exports, not guessed]

## Files

| File | What | When to read |
|------|------|--------------|
| [each file] | [from exports] | [when trigger] |

## Subdirectories

| Directory | What | When to read |
|-----------|------|--------------|
| [each subdir] | [purpose] | [when trigger] |

## Before Creating New Code

| Category | Check Location | Pattern |
|----------|----------------|---------|
| [if patterns found] | [path] | [pattern name] |
```

### 4. Present for Approval

Show generated content and ask:

```
Generated CLAUDE.md for [path]:

[content preview]

Options:
1. Create file as shown
2. Modify before creating
3. Cancel
```

### 5. Create File

Write CLAUDE.md to the specified directory.

## Examples

**Basic usage:**
```
/init src/lib/hooks
```

**Current directory:**
```
/init .
```

**With subdirectories:**
```
/init packages/core
```

## Evidence Requirements

Every entry must be derived from code:

| Field | Source |
|-------|--------|
| File "What" | Export names, class names, function signatures |
| "When to read" | What problems these exports solve |
| Subdirectory "What" | Aggregate of file purposes |
| "Before Creating" | High fan-in files (imported by many) |

**Never guess.** If purpose unclear from code, use: "Exports: [list of exports]"
