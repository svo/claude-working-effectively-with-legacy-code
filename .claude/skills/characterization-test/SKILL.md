---
name: characterization-test
description: Write characterization tests that document the actual current behavior of legacy code before modifying it. Use when an engineer needs to understand what existing code does, lock down behavior before refactoring, or create a safety net for legacy code changes.
---

# Characterization Test

Write tests that document what the code *actually does* - not what it *should* do. These tests create a safety net that detects unintended behavior changes during refactoring or modernization.

## Process

1. **Read the code under examination carefully.** Trace the logic, identify inputs, outputs, and side effects. Do not assume you know what it does - read it.

2. **Write tests based on your understanding of the code's actual behavior.** For each behavior you observe in the code, write a test with an assertion capturing that behavior.

3. **Run the tests to verify your understanding matches reality.** Fix any assertions where the code behaves differently than you expected - the code is the source of truth, not your reading of it.

4. **Probe edge cases.** Expand coverage to boundaries:
   - What happens with null/nil/empty inputs?
   - What happens at boundary values?
   - What happens with unexpected types?
   - What side effects occur (files written, state mutated, exceptions thrown)?

6. **Document surprising behavior in the test name.** Use descriptive names:
   ```
   test_should_return_negative_one_when_input_is_empty
   test_should_silently_truncate_when_name_exceeds_fifty_characters
   test_should_modify_global_state_when_called_with_zero
   ```

## Guidelines

- **Do not fix bugs you discover.** Characterization tests lock down *current* behavior, including bugs. Fixing bugs is a separate step that comes after the safety net is in place.
- **Focus on the change area.** You don't need to characterize the entire codebase - focus on code near your planned changes.
- **Look for pinch points.** Find narrow places where a single test can cover many code paths through the system.
- **Test at interception points.** Choose test points where you can observe the effects of the code you plan to change.
- **One assertion per test.** Each test should verify one specific behavior.
- **If the code is hard to test, note the dependency.** This feeds into `/dependency-breaker` for the next step.

## Output

Deliver:
- A set of characterization tests with descriptive names
- A summary of discovered behaviors, especially surprising ones
- A list of dependencies that block further testing (candidates for `/dependency-breaker`)
