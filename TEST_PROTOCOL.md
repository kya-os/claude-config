# A/B Test Protocol: Claude Config Effectiveness

Empirical test to measure improvement from kya-os/claude-config installation.

## Test Setup

### Environment A: Baseline (No Config)
```bash
# Fresh clone without .claude
git clone https://github.com/kya-os/agent-shield.git /tmp/test-baseline
cd /tmp/test-baseline
rm -rf .claude  # Ensure no config
```

### Environment B: With Config
```bash
# Fresh clone with claude-config
git clone https://github.com/kya-os/agent-shield.git /tmp/test-with-config
cd /tmp/test-with-config
git clone https://github.com/kya-os/claude-config.git .claude
# Then run: Use your setup skill to initialize this repository
```

### Test Execution
- Use **fresh Claude Code sessions** for each test (clear context)
- Record **time to first correct answer**
- Record **number of tool calls** (file reads, searches)
- Record **accuracy of answer** against known-correct answer

---

## Test Questions

### Question 1: DRY Enforcement (Session ID Generation)

**Ask both environments:**
```
I need to add session tracking to a new API endpoint.
How should I generate session IDs to be consistent with the rest of the codebase?
```

**Known-Correct Answer:**
There are THREE different session ID implementations:
1. `EdgeSessionTracker` (Next.js): `crypto.randomUUID()` - globally unique
2. `ExpressSessionTracker`: `${Date.now()}-${Math.random().toString(36).substr(2, 9)}` - timestamp-based
3. `SessionManager.generateSessionId()`: Time-window based with 5-minute bucketing

The correct answer depends on context:
- For middleware: Use `SessionManager` (enhanced middleware pattern)
- For raw tracking: Use existing tracker for your platform
- **Do NOT create a new implementation**

**Grading:**
| Grade | Criteria |
|-------|----------|
| A | Identifies all 3 implementations, explains differences, recommends reuse |
| B | Identifies 2 implementations, recommends reuse |
| C | Identifies 1 implementation, recommends reuse |
| D | Suggests creating new implementation |
| F | Creates new implementation without checking existing code |

**Metrics to Record:**
- [ ] Time to answer (seconds)
- [ ] Number of file reads
- [ ] Number of grep/glob searches
- [ ] Mentioned existing implementations? (Y/N)
- [ ] Recommended reuse? (Y/N)

---

### Question 2: Pattern Understanding (Confidence Scale)

**Ask both environments:**
```
I'm adding a new detection method that returns a confidence score.
What scale should I use (0-1 or 0-100) and why?
```

**Known-Correct Answer:**
The codebase has a complex confidence scale situation:
- **Config uses 0-100**: `confidenceThreshold: 70` in agent-detector.ts:30
- **WASM interface uses 0-1**: Internal WASM module expectation
- **EdgeAgentDetector returns 0-100**: But gets converted to 0-1 for WASM interface
- **Conversion happens twice**: loader-factory.ts:68 and :185

New detection methods should:
1. Return 0-100 scale (matches config and final output)
2. If integrating with WASM loader, conversion is handled automatically
3. Check `loader-factory.ts` lines 64-67 for the documented conversion logic

**Grading:**
| Grade | Criteria |
|-------|----------|
| A | Identifies both scales, explains conversion chain, cites file:line |
| B | Identifies both scales, explains one conversion |
| C | Identifies one scale with reasoning |
| D | Guesses without checking code |
| F | Gives wrong answer confidently |

**Metrics to Record:**
- [ ] Time to answer (seconds)
- [ ] Read loader-factory.ts? (Y/N)
- [ ] Read agent-detector.ts? (Y/N)
- [ ] Mentioned dual conversion? (Y/N)
- [ ] Cited specific line numbers? (Y/N)

---

### Question 3: Error Handling Consistency

**Ask both environments:**
```
I'm implementing a new storage adapter. Looking at RedisStorageAdapter
and MemoryStorageAdapter, should I add try-catch around JSON.parse operations?
```

**Known-Correct Answer:**
**YES, but with awareness of the inconsistency:**
- `RedisStorageAdapter`: Catches all JSON parse errors, logs warning, returns gracefully
- `MemoryStorageAdapter`: Has NO error handling - exceptions propagate

This is an **existing inconsistency** in the codebase. For a new adapter:
1. Follow `RedisStorageAdapter` pattern (graceful degradation)
2. Consider proposing a fix for `MemoryStorageAdapter`
3. The pattern `typeof data === 'string' ? JSON.parse(data) : data` appears 7+ times without centralization

**Grading:**
| Grade | Criteria |
|-------|----------|
| A | Identifies inconsistency, recommends Redis pattern, suggests refactoring |
| B | Identifies inconsistency, recommends one pattern |
| C | Recommends try-catch without noting inconsistency |
| D | Says "follow existing pattern" without identifying which |
| F | Says no try-catch needed |

**Metrics to Record:**
- [ ] Time to answer (seconds)
- [ ] Read redis-adapter.ts? (Y/N)
- [ ] Read memory-adapter.ts? (Y/N)
- [ ] Identified inconsistency? (Y/N)
- [ ] Suggested centralization? (Y/N)

---

### Question 4: Architectural Understanding (EventEmitter vs Storage)

**Ask both environments:**
```
I need to track when agents are detected for analytics.
Should I use the EventEmitter or the StorageAdapter? What's the difference?
```

**Known-Correct Answer:**
The system has TWO parallel tracking mechanisms that are **not synchronized**:

1. **EventEmitter** (`agentshield-core/src/events/emitter.ts`):
   - In-process event broadcasting
   - For real-time handlers/callbacks
   - Events can fire before storage completes

2. **StorageAdapter** (`agentshield-nextjs/src/storage/`):
   - Persistent storage (Redis/Memory)
   - For historical queries and analytics
   - Can have events that emitter never fired

**For analytics, use StorageAdapter** - it's persistent. But be aware:
- No transaction semantics between them
- They can get out of sync
- If you need both real-time and persistence, you must handle both explicitly

**Grading:**
| Grade | Criteria |
|-------|----------|
| A | Explains both mechanisms, identifies sync issue, recommends correctly |
| B | Explains both mechanisms, recommends correctly |
| C | Identifies one mechanism correctly |
| D | Confuses the two |
| F | Invents a mechanism that doesn't exist |

**Metrics to Record:**
- [ ] Time to answer (seconds)
- [ ] Found emitter.ts? (Y/N)
- [ ] Found storage types? (Y/N)
- [ ] Mentioned sync issue? (Y/N)
- [ ] Correct recommendation? (Y/N)

---

### Question 5: Implementation Task (Most Critical)

**Ask both environments:**
```
Add a utility function to format detection confidence as a percentage string
(e.g., 0.85 → "85%"). Where should this go and how should it be implemented?
```

**Known-Correct Answer:**
Before creating new code, check existing utilities:
- `apps/web/lib/utils/format-numbers.ts` has `formatPercentage()`
- This ALREADY EXISTS - do not duplicate

If the function truly doesn't exist for your use case:
1. Add to `packages/agentshield-shared/src/utils/` (shared across packages)
2. Export from package index
3. Follow existing naming conventions

**Grading:**
| Grade | Criteria |
|-------|----------|
| A | Finds existing formatPercentage(), recommends reuse |
| B | Checks for existing utilities first, then proposes location |
| C | Proposes reasonable location without checking existing |
| D | Creates duplicate in wrong location |
| F | Creates inline implementation without any consideration |

**Metrics to Record:**
- [ ] Time to answer (seconds)
- [ ] Searched for existing utilities? (Y/N)
- [ ] Found formatPercentage? (Y/N)
- [ ] Recommended reuse? (Y/N)
- [ ] If new: proposed shared location? (Y/N)

---

## Scoring Summary

### Per-Question Scoring
| Grade | Points |
|-------|--------|
| A | 4 |
| B | 3 |
| C | 2 |
| D | 1 |
| F | 0 |

### Aggregate Metrics

**Understanding Score** (Questions 1-4):
- Max: 16 points
- Measures: Pattern recognition, architecture understanding

**DRY Enforcement Score** (Question 5):
- Max: 4 points
- Measures: Checks before creating

**Efficiency Score**:
- Total tool calls across all questions
- Lower is better (found info faster)

**Time Score**:
- Total time across all questions
- Lower is better

---

## Expected Results

### Baseline (No Config)
- Understanding: 6-10 points (explores randomly, misses patterns)
- DRY: 1-2 points (likely creates new code)
- Tool calls: 30-50 (lots of searching)
- Time: 10-15 minutes

### With Config (After Setup)
- Understanding: 12-16 points (CLAUDE.md guides to right files)
- DRY: 3-4 points (checks existing utilities first)
- Tool calls: 15-25 (targeted reads)
- Time: 5-8 minutes

---

## Recording Template

```markdown
## Test Results: [Date]

### Environment: [Baseline / With Config]

#### Question 1: Session ID
- Time: ___ seconds
- File reads: ___
- Searches: ___
- Grade: ___
- Notes:

#### Question 2: Confidence Scale
- Time: ___ seconds
- File reads: ___
- Searches: ___
- Grade: ___
- Notes:

#### Question 3: Error Handling
- Time: ___ seconds
- File reads: ___
- Searches: ___
- Grade: ___
- Notes:

#### Question 4: EventEmitter vs Storage
- Time: ___ seconds
- File reads: ___
- Searches: ___
- Grade: ___
- Notes:

#### Question 5: Format Utility
- Time: ___ seconds
- File reads: ___
- Searches: ___
- Grade: ___
- Notes:

### Summary
- Understanding Score: ___/16
- DRY Score: ___/4
- Total Tool Calls: ___
- Total Time: ___ minutes
```

---

## Running the Test

1. **Prepare environments** (baseline and with-config)
2. **Start fresh Claude Code session** for each environment
3. **Ask Question 1**, record all metrics
4. **Clear context** (`/clear`)
5. **Repeat for Questions 2-5**
6. **Compare aggregate scores**

The improvement should be measurable and empirical.
