# prime/

Session priming skill - loads context and validates roadmap health.

## Files

| File | What | When to read |
|------|------|--------------|
| `SKILL.md` | Priming workflow and validation rules | Running `/prime` |

## References

| File | What |
|------|------|
| `references/codebase-facts.md` | Key patterns, commands, pitfalls |

## Also Loads

This skill also reads from `skills/plan/references/`:
- `roadmap.md` - Current OKRs and initiatives
- `strategy.md` - Product direction

## Usage

```
/prime              # Full prime with validation
/prime --quick      # Skip validation, just load context
```
