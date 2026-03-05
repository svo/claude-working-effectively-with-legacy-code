---
name: effect-analysis
description: Analyze the blast radius of a proposed change to legacy code. Use when an engineer needs to understand what will be affected by a change, find the right test points, or assess the risk of a modification before making it.
---

# Effect Analysis

Map how a proposed change propagates through the codebase to understand its blast radius and identify the right places to test.

## When to Use

- Before making a change to legacy code, to understand what might break.
- When deciding where to write tests to cover a planned modification.
- When assessing the risk of a change to determine how cautious to be.
- When looking for pinch points where a few tests can cover many effects.

## Process

1. **Identify the change point.** What specific code will be modified?

2. **Trace forward effects.** From the change point, follow the data and control flow:
   - What methods use the return value of this method?
   - What methods read the state this method modifies?
   - What methods call those methods? (Continue recursively.)
   - What external systems are affected (databases, files, APIs, UI)?

3. **Trace backward effects.** What feeds into the change point:
   - What callers pass data to this code?
   - What state must be set up before this code runs?
   - What assumptions do callers make about this code's behavior?

4. **Draw an effect sketch.** Create a visual map showing:
   - The change point (center)
   - Direct effects (first ring)
   - Indirect effects (outer rings)
   - External system boundaries

   Use ASCII or structured text:
   ```
   [changePoint]
     → [directEffect1] → [indirectEffect1a]
                        → [indirectEffect1b] → [externalSystem]
     → [directEffect2]
     ← [caller1]
     ← [caller2] ← [upstreamCaller]
   ```

5. **Identify interception points.** Find the best places to write tests:
   - Close to the change point for focused verification.
   - At pinch points for broad coverage with few tests.
   - At system boundaries for integration confidence.

6. **Assess risk.** Based on the effect sketch:
   - How many code paths are affected?
   - Are external systems in the blast radius?
   - Are there effects that cross architectural boundaries?
   - How much of the affected code has existing tests?

## Guidelines

- Be thorough but pragmatic - trace effects until they reach a boundary (tested code, stable interface, or external system).
- Pay special attention to **mutable shared state** - it creates non-obvious effect paths.
- Distinguish between effects that change behavior and effects that are purely structural.
- Use the effect sketch to justify your testing strategy to the engineer.

## Output

Deliver:
- An effect sketch showing the blast radius of the proposed change
- Identified interception points and pinch points for testing
- A risk assessment (low/medium/high) with justification
- Recommended testing strategy before making the change
- Any dependencies that need breaking to make testing feasible
