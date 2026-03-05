---
name: branch-by-abstraction
description: Incrementally replace an internal implementation by introducing an abstraction layer, building a new implementation behind it, switching over, and removing the abstraction. Use when an engineer needs to swap a library, framework, or internal module without a big-bang replacement.
---

# Branch by Abstraction

Replace an internal implementation incrementally by introducing an abstraction boundary, building the new implementation behind it, and switching over - all while the system remains working at every step.

This is the in-codebase equivalent of the Strangler Fig pattern. Where strangler fig works at system/service boundaries with routing layers, branch by abstraction works at code boundaries with interfaces.

## When to Use

- Replacing a library or framework (e.g., swapping an ORM, HTTP client, or logging framework).
- Replacing an internal module with a new implementation.
- Migrating from one pattern to another across the codebase (e.g., callbacks to async/await).
- Any situation where you need to swap an implementation but cannot do it atomically.

## Process

### Step 1: Identify the Flawed Abstraction (or Missing One)

- What concrete implementation do you want to replace?
- Where is it used? Map all call sites.
- Is there already an abstraction (interface, abstract class, protocol) between the consumers and the implementation? If so, you may be able to skip step 2.

### Step 2: Introduce the Abstraction

1. **Create an interface** that represents the capability the implementation provides, not the implementation's API. Design the abstraction around *what consumers need*, not what the current implementation offers.

2. **Make the existing implementation conform** to the new interface. This should be a mechanical refactoring - behavior does not change.

3. **Update all consumers** to depend on the abstraction rather than the concrete implementation. Again, purely mechanical - no behavior change.

4. **Verify everything still works.** Run existing tests. This step must be a zero-behavior-change refactoring.

### Step 3: Build the New Implementation

1. **Create the new implementation** of the abstraction, following end-state ideals:
   - Clean design, full test coverage, dependency injection.
   - Do not replicate quirks of the old implementation unless they are contractually required.

2. **Test the new implementation** against the same interface contract as the old one. Contract tests that run against both implementations are ideal.

### Step 4: Switch Over

Choose a migration strategy:

- **All-at-once**: Swap the binding in your DI container or factory. Simple when the abstraction is clean and contract tests pass.
- **Gradual**: Use a feature flag or configuration to route some consumers to the new implementation while others use the old. Useful for high-risk replacements.
- **Consumer-by-consumer**: Update individual call sites one at a time. Useful when the abstraction isn't perfectly uniform across consumers.

### Step 5: Clean Up

1. **Remove the old implementation** once all consumers have migrated.
2. **Evaluate the abstraction**: Is it still needed? If the new implementation is the only one and there's no foreseeable need for substitution, the abstraction may be unnecessary complexity. Consider inlining it. If it serves as a useful architectural boundary or test seam, keep it.

## Guidelines

- **Every step must leave the system working.** If any step breaks the build or tests, the step is too large - break it down further.
- **The abstraction should reflect the consumer's needs**, not the implementation's API. This is your chance to design a better interface.
- **Contract tests are essential.** They verify both implementations honor the same behavior and make the switch-over safe.
- **Don't gold-plate the abstraction.** It exists to enable the migration. If it outlives its usefulness, remove it.
- If consumers are too tightly coupled to the old implementation to introduce an abstraction cleanly, use `/dependency-breaker` first.

## Output

Deliver:
- The abstraction (interface/protocol) designed around consumer needs
- The new implementation with full test coverage
- Contract tests that verify both old and new implementations
- A migration plan for switching consumers over
- Identification of the cleanup step (remove old implementation, evaluate abstraction)
