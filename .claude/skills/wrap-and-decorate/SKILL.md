---
name: wrap-and-decorate
description: Add behavior before or after existing legacy code using Wrap Method or Wrap Class (Decorator) techniques. Use when an engineer needs to extend existing behavior without modifying the original logic, such as adding logging, validation, caching, or cross-cutting concerns.
---

# Wrap Method / Wrap Class (Decorator)

Extend existing behavior by wrapping it rather than modifying it. Ideal for adding cross-cutting concerns or pre/post processing to legacy code.

## When to Use

- You need to add behavior that runs before or after existing logic.
- You want to avoid modifying the internals of legacy code.
- The new behavior is a cross-cutting concern: logging, validation, caching, metrics, authorization, error handling.

## Wrap Method

1. **Rename the existing method** to reflect that it is the core/inner behavior:
   ```
   pay() → dispatchPay()
   ```

2. **Create a new method with the original name** that:
   - Performs the new pre-processing (if any)
   - Calls the renamed original method
   - Performs the new post-processing (if any)

3. **Write tests** for the wrapper behavior. The original behavior is already characterized (or should be via `/characterization-test`).

### Example Shape

```
// Before
pay(employees) {
    // ... legacy payment logic ...
}

// After
pay(employees) {
    logPaymentRun(employees)       // new: pre-processing
    dispatchPay(employees)         // original logic, renamed
    sendPaymentConfirmation()      // new: post-processing
}
```

## Wrap Class (Decorator Pattern)

Use when the wrapping is substantial enough to warrant its own class, or when you cannot modify the original class.

1. **Create a new class** that implements the same interface as the original.
2. **Accept the original as a constructor dependency.**
3. **Delegate to the original** for core behavior.
4. **Add new behavior** before/after delegation.

### Example Shape

```
class LoggingOrderProcessor implements OrderProcessor {
    constructor(inner: OrderProcessor) { ... }

    process(order) {
        log("Processing order", order.id)
        result = inner.process(order)
        log("Order processed", result.status)
        return result
    }
}
```

## Guidelines

- The wrapper should have a single responsibility - don't pile multiple concerns into one wrapper.
- Stack multiple wrappers for multiple concerns (decorator chain).
- The original code stays untouched beyond the rename (Wrap Method) or entirely untouched (Wrap Class).
- New wrapper code must follow current design standards: dependency injection, clean interfaces, full tests.
- If the original lacks an interface/abstraction, consider using `/dependency-breaker` to extract one first.

## Output

Deliver:
- The wrapper method or class with full test coverage
- The minimal rename or integration change to the original
- Identification of other legacy code that could benefit from the same wrapping approach
