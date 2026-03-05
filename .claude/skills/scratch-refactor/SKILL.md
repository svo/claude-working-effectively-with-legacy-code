---
name: scratch-refactor
description: Perform scratch refactoring to understand complex legacy code without committing changes. Use when an engineer needs to understand what legacy code does before making real modifications, or when the code is too complex to reason about by reading alone.
---

# Scratch Refactoring

Refactor freely to understand the code - then throw it all away. This is an exploration technique, not a code change technique.

## When to Use

- The code is too complex to understand by reading alone.
- You need to find the boundaries, responsibilities, and hidden abstractions in a large class or method.
- You want to build a mental model before planning real changes.
- You need to identify what tests to write before modifying the code.

## Process

1. **Create an explicit scratch context.** Make it clear that these changes are exploratory and will not be kept:
   - Use a worktree (`/worktree`) for isolated exploration that is easy to discard.
   - Work on a branch you will discard.
   - Or simply note that all changes will be reverted.

2. **Refactor aggressively to understand.** There are no rules - this is exploration:
   - Rename variables and methods to what you think they mean.
   - Extract methods to isolate responsibilities.
   - Inline things to see the full picture.
   - Move code around to see what groups together.
   - Add temporary comments if it helps your understanding.

3. **Look for hidden structure:**
   - **Feature sketches**: Group related instance variables and methods - these may be hidden classes.
   - **Responsibility clusters**: Which methods always change together? They may belong in their own module.
   - **Temporal coupling**: What must happen in sequence? This reveals workflow or pipeline structure.
   - **Data affinity**: Which variables are always used together? They may be a missing data structure.

4. **Record your findings** before discarding the scratch work:
   - A description of the code's actual responsibilities.
   - Hidden abstractions or classes you discovered.
   - Dependencies that would need breaking.
   - Suggested decomposition strategy.
   - Candidate areas for characterization tests.

5. **Discard all scratch changes.** Revert everything. The value is the understanding, not the code.

## Guidelines

- This is judgment-free exploration. Ugly intermediate states are expected.
- Do not try to preserve behavior during scratch refactoring - you are exploring, not shipping.
- Time-box the exploration. If you haven't built understanding in a reasonable time, step back and try a different entry point.
- The output should inform what technique to use next: `/characterization-test`, `/dependency-breaker`, `/sprout-method`, `/strangler-fig`, etc.

## Output

Deliver:
- A summary of what the code actually does (its responsibilities)
- Hidden abstractions or concepts discovered
- A dependency map of what makes the code hard to change
- Recommended next steps and which techniques to apply
- All scratch changes reverted - no code modifications remain
