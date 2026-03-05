---
name: dependency-breaker
description: Break dependencies in legacy code to make it testable and changeable. Use when code cannot be tested or modified because of tight coupling to databases, file systems, network services, singletons, static methods, or other hard dependencies.
---

# Dependency Breaker

Break the dependencies that make legacy code resistant to testing and change. This is the prerequisite for most other legacy code techniques.

## When to Use

- You cannot instantiate a class in a test harness because its constructor does too much.
- A method cannot run in tests because it reaches out to databases, file systems, or network services.
- Code depends on singletons or global state that cannot be substituted.
- Static method calls prevent substitution of behavior.
- A method's dependencies are hidden inside its implementation rather than explicit in its signature.

## Techniques

### Extract and Override

Take a dependency-creating call, extract it into its own overridable method, then substitute it in tests via subclassing.

1. Identify the problematic call (database query, file read, API call).
2. Extract it into a virtual/overridable method with a clear name.
3. In tests, create a subclass that overrides this method to return test data.

**Variants:**
- **Extract and Override Call** - for individual method calls
- **Extract and Override Factory Method** - for object creation in constructors
- **Extract and Override Getter** - for lazy initialization

### Parameterize

Make hidden dependencies explicit by passing them in.

- **Parameterize Method** - pass the dependency as a method parameter instead of creating it internally.
- **Parameterize Constructor** - accept collaborators via constructor (dependency injection).

### Extract Interface

Create an abstraction from an existing concrete class so you can substitute fakes in tests.

1. Identify the concrete class that is causing the dependency.
2. Extract an interface containing the methods your code actually uses.
3. Make the concrete class implement the interface.
4. Depend on the interface rather than the concrete class.

### Other Techniques

- **Adapt Parameter** - wrap a third-party type you cannot change behind your own interface.
- **Break Out Method Object** - move a large complex method into its own class to test it independently.
- **Introduce Instance Delegator** - replace a static method with an instance method that delegates to the static, making it overridable.
- **Introduce Static Setter** - for singletons, add a setter so tests can substitute a fake instance.
- **Encapsulate Global References** - wrap global variables/functions in a class to make them substitutable.
- **Skin and Wrap the API** - create a thin wrapper around a third-party API to decouple from it.
- **Push Down Dependency** - move a dependency into a subclass so the parent can be tested without it.
- **Pull Up Feature** - move code into a superclass to decouple from subclass dependencies.

## Process

1. **Identify the blocking dependency.** What prevents this code from being tested?
2. **Choose the simplest technique** that breaks the dependency. Prefer parameterization and interface extraction over more complex approaches.
3. **Apply the technique using mechanical refactoring.** Small steps, lean on the compiler.
4. **Verify the code is now testable.** Write a test that instantiates/calls the code in isolation.
5. **Move toward the end-state.** Where possible, choose the technique that also moves the code toward clean architecture (e.g., extract interface aligns with depending on abstractions).

## Guidelines

- Prefer techniques that make dependencies explicit (parameterization, constructor injection) over those that hide the seam (subclass and override).
- The goal is testability now and clean architecture eventually - each dependency break is a step on that path.
- Don't break all dependencies at once. Focus on the ones blocking your current change.
- Use `/seam-finder` to identify where dependencies can be intercepted.

## Output

Deliver:
- The specific dependency identified and the technique applied
- The refactored code with the dependency broken
- Tests demonstrating the code can now be tested in isolation
- Notes on remaining dependencies that could be addressed in future work
