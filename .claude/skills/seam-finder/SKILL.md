---
name: seam-finder
description: Find seams in legacy code where behavior can be altered without editing the code at that point. Use when an engineer needs to identify safe modification points, test insertion points, or places to intercept behavior for testing or modernization.
---

# Seam Finder

A seam is a place where you can alter behavior without editing the code at that point. Every seam has an enabling point - the place where you choose which behavior to use. Finding seams is the key to making legacy code testable and changeable.

## When to Use

- You need to get legacy code under test but cannot modify it directly.
- You want to substitute dependencies without changing the code that uses them.
- You need to identify safe points for intercepting behavior during modernization.

## Seam Types

### Object Seams

**What:** Use polymorphism (interfaces, subclassing, duck typing) to substitute behavior.
**Enabling point:** Where the object is instantiated or injected.

Look for:
- Virtual/overridable methods that can be replaced in a test subclass.
- Constructor parameters where a different implementation could be passed.
- Interface types where a test double could be substituted.
- Method parameters typed to abstractions rather than concretes.

### Link Seams

**What:** Substitute behavior by changing what gets linked, loaded, or resolved at build/deploy time.
**Enabling point:** Build configuration, classpath, module resolution, or import mechanism.

Look for:
- Module imports that could be replaced with test modules.
- Classpath ordering that determines which implementation is loaded.
- Build configurations that swap implementations.
- Dynamic module loading or plugin systems.

### Preprocessing Seams

**What:** Use preprocessor directives or compile-time mechanisms to swap behavior.
**Enabling point:** Macro definitions, conditional compilation flags.

Look for:
- `#ifdef` / `#define` blocks (C/C++)
- Compile-time feature flags
- Code generation templates

## Process

1. **Read the code around the change point.** Identify the dependencies that make it hard to test or modify.

2. **For each dependency, search for seams:**
   - Can you substitute the dependency through an object seam (polymorphism, injection)?
   - Can you substitute it through a link seam (different module, classpath)?
   - Can you substitute it through preprocessing (compile-time swap)?

3. **Evaluate each seam:**
   - How easy is it to use? (Object seams are usually easiest.)
   - Does using it require changes elsewhere? (Prefer seams with minimal ripple.)
   - Does it move the code toward the end-state architecture? (Prefer seams that introduce clean abstractions.)

4. **Identify the enabling point** for each seam and confirm you can control it in tests.

5. **Recommend the best seam** for the engineer's situation.

## Guidelines

- Object seams are the most common and usually the best choice in object-oriented code.
- If no seam exists, you need to create one - this is where `/dependency-breaker` techniques apply.
- A good seam is one where the enabling point is easy to control and the substitution is safe.
- Prefer seams that also serve as architectural boundaries - they do double duty.

## Output

Deliver:
- A list of identified seams with their types and enabling points
- A recommendation for which seam to use and why
- If no suitable seam exists, the recommended `/dependency-breaker` technique to create one
