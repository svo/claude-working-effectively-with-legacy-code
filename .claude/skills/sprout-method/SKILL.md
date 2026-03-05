---
name: sprout-method
description: Add new behavior to legacy code using the Sprout Method or Sprout Class technique. Use when an engineer needs to add functionality without further entangling existing legacy code, keeping new code clean and tested while minimizing changes to the original.
---

# Sprout Method / Sprout Class

Add new behavior by growing clean, tested code alongside legacy code rather than weaving new logic into existing tangles.

## When to Use

- You need to add new functionality to an existing method or class.
- The existing code is difficult to test or understand.
- You want new code to follow current design standards regardless of the surrounding code quality.

## Sprout Method

1. **Identify where the new behavior is needed** in the existing method.

2. **Write the new logic as a separate method.**
   - Give it a clear, intention-revealing name.
   - Design it with clean inputs and outputs - avoid relying on the internal state of the legacy code.

3. **Write tests for the new method in isolation.**
   - The new method should be fully testable on its own.
   - One assertion per test, descriptive test names.

4. **Call the new method** from the original method at the appropriate point.

5. **Keep the original method unchanged** beyond adding the call. Do not refactor the legacy method in this step.

### Example Shape

```
// Before: all logic tangled in one method
processOrder(order) {
    // ... 80 lines of legacy logic ...
}

// After: new behavior sprouted into a clean method
processOrder(order) {
    // ... existing legacy logic ...
    applyLoyaltyDiscount(order)  // new sprouted call
    // ... remaining legacy logic ...
}

applyLoyaltyDiscount(order) {
    // Clean, tested, follows current patterns
}
```

## Sprout Class

Use when the new behavior requires its own dependencies or when the existing class is too entangled to instantiate in tests.

1. **Create a new class** for the new functionality.
2. **Design it with dependency injection** - receive collaborators via constructor.
3. **Test it independently** with full coverage.
4. **Use it from the original class** with minimal changes.

## Guidelines

- New code must follow end-state ideals: clean architecture boundaries, dependency injection, self-documenting naming.
- The sprout is born clean even if it lives next to messy code.
- This is a stepping stone - the legacy code around it can be modernized later.
- If you cannot call the sprout without modifying the legacy method's dependencies, use `/dependency-breaker` first.

## Output

Deliver:
- The new sprouted method or class with full test coverage
- The minimal change to the original code to integrate the sprout
- Notes on further modernization opportunities discovered during the work
