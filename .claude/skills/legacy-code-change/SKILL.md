---
name: legacy-code-change
description: Execute the full Legacy Code Change Algorithm to safely modify legacy code. Use when an engineer needs end-to-end guidance for making a change to legacy code, from identifying change points through to delivering tested, modernized code.
---

# Legacy Code Change Algorithm

The complete workflow for safely modifying legacy code. This orchestrates the other skills into a coherent end-to-end process.

## Process

### Step 1: Identify Change Points

- What specific behavior needs to change or be added?
- Which files, classes, and methods are involved?
- Read the code thoroughly. Use `/scratch-refactor` if the code is too complex to understand by reading.

### Step 2: Analyze the Blast Radius

- Use `/effect-analysis` to map how the change propagates.
- Identify what could break and where tests are needed.
- Determine the risk level (low/medium/high).

### Step 3: Find Seams and Test Points

- Use `/seam-finder` to identify where behavior can be intercepted for testing.
- Identify pinch points where a few tests provide broad coverage.
- If no suitable seams exist, plan dependency breaking.

### Step 4: Break Dependencies (if needed)

- Use `/dependency-breaker` to decouple code enough to test it.
- Apply the simplest technique that unblocks testing.
- Prefer techniques that also move toward clean architecture.

### Step 5: Write Characterization Tests

- Use `/characterization-test` to document current behavior.
- Cover the change point and its immediate effects.
- Focus on the behavior that must be preserved.

### Step 6: Make the Change

Choose the appropriate technique based on the type of change:

| Change Type | Technique |
|---|---|
| Add new behavior | `/sprout-method` |
| Extend existing behavior (before/after) | `/wrap-and-decorate` |
| Replace a module or subsystem | `/strangler-fig` |
| Modify existing behavior | Direct modification (with characterization tests as safety net) |

### Step 7: Verify and Converge

- Run all characterization tests to confirm behavior is preserved.
- Run new tests to confirm the change works as intended.
- Review the change for opportunities to reduce pattern entropy:
  - Does the new code follow current conventions?
  - Can nearby code be nudged toward the target architecture?
  - Are there quick wins for improving naming, structure, or boundaries?
- Do not over-reach - incremental improvement, not wholesale refactoring.

## Decision Guide

```
Need to change legacy code?
│
├─ Don't understand the code?
│  └─ /scratch-refactor → then return here
│
├─ Don't know what will break?
│  └─ /effect-analysis → then continue
│
├─ Can't test the code?
│  ├─ Can't find a seam? → /seam-finder
│  └─ Found seam but blocked? → /dependency-breaker
│
├─ No tests covering the change area?
│  └─ /characterization-test → then continue
│
├─ Ready to make the change?
│  ├─ Adding new behavior → /sprout-method
│  ├─ Extending behavior → /wrap-and-decorate
│  └─ Replacing a component → /strangler-fig
│
└─ Change complete → verify, converge, done
```

## Output

Deliver:
- The completed change with all tests passing
- Summary of techniques applied and why
- Characterization tests added as a lasting safety net
- Notes on further modernization opportunities for future work
