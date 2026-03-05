# Working Effectively with Legacy Code

This project configures Claude Code to help engineers maintain and evolve legacy codebases using proven legacy code techniques and modern modernization patterns.

## What We Mean by Legacy Code

Legacy code is code that reflects **outdated design choices, accumulated tech debt, and pattern entropy**. It may have tests, it may even work well - but its design has drifted from current understanding, conventions, or requirements. Common symptoms:

- **Architectural drift**: The code no longer matches the team's current mental model or target architecture.
- **Pattern entropy**: Inconsistent patterns across the codebase from multiple generations of development.
- **Excessive coupling**: Changes in one area cascade unpredictably to others.
- **Opaque intent**: The code works, but *why* it works that way is unclear - design decisions are buried.
- **Resistance to change**: Even small modifications feel risky or require disproportionate effort.

## First Contact with a Legacy Codebase

Before applying any techniques, orient yourself:

1. **Discover the landscape.** What language(s), build system, test framework, and dependency management are in use?
2. **Assess what exists.** Don't assume the worst - check for existing tests, documentation, CI/CD, and architectural patterns. Use `/codebase-assessment` for a structured evaluation.
3. **Identify current conventions.** What patterns does the team follow today? What does the target architecture look like? Respect existing conventions when making changes nearby, even if they differ from the end-state ideals.
4. **Find the pain.** Where is the code most resistant to change? Where do engineers spend disproportionate effort? These are the highest-value targets for modernization.

## When NOT to Apply Legacy Techniques

Not every change to old code needs the full algorithm. Just make the change directly when:

- The code is well-tested and well-understood, and the change is straightforward.
- You are making configuration changes, updating dependencies, or fixing typos.
- The change is isolated with no coupling concerns.

The legacy code techniques are for code that is **resistant to change** - not a ceremony for every edit to old files.

## Core Principles

- **Preserve behavior first.** Verify what code does with characterization tests before changing it.
- **Small, safe steps.** Each change should be small enough to confidently say behavior is preserved.
- **New code should be clean.** Even if surrounding code is messy, new code (sprouts, wraps) must follow current design standards.
- **Lean on the compiler.** Use rename/signature changes to let compiler errors guide you to all affected locations.
- **Prefer mechanical refactorings.** Favor purely structural refactorings verifiable by the compiler or IDE.
- **Don't try to fix everything at once.** Improvement is incremental. Leave code a little better than you found it.
- **Understand before changing.** Use scratch refactoring and effect analysis to build understanding before committing to changes.
- **Converge on current patterns.** When touching legacy code, move it toward the team's current conventions and target architecture - don't just preserve the old style.

## The Legacy Code Change Algorithm

When modifying legacy code, follow this sequence:

1. **Identify change points** - Where do you need to make changes?
2. **Find test points** - Where can you write tests to cover the change? Add characterization tests if coverage is lacking.
3. **Break dependencies** - Decouple code enough to make it testable and changeable.
4. **Write tests** - Document existing behavior so you can verify changes are safe.
5. **Make changes and refactor** - With your safety net in place, modernize the code toward current patterns.

## End-State Ideals

When modernizing legacy code, converge toward these qualities. These are not prerequisites - they are the direction of travel. **Customize this section to match your team's target architecture and conventions.**

### Self-Documenting Code

- Code should communicate intent through expressive naming, not comments.
- If code needs explanation, refactor it to be clearer.

### Separation of Concerns

- Business logic should be independent of frameworks, databases, and I/O.
- Infrastructure concerns (persistence, networking, UI) should depend on business logic, not the reverse.
- Define clear boundaries between layers or modules. Existing boundary violations are candidates for modernization.
- The specific layering model (Clean Architecture, Hexagonal, etc.) should match the team's target architecture.

### Dependency Injection

- Components receive dependencies rather than creating them.
- Depend on abstractions (interfaces) not concrete implementations.
- Configure containers for different contexts (test vs. production).

### Testing Standards

- Each test should focus on one behavior.
- Test names should be descriptive sentences that communicate intent.
- Include contract tests for service interactions.
- Include architectural tests that validate boundary rules.
- Aim for high meaningful test coverage - not just coverage-seeking.

### Observability

- Structured logging with correlation IDs.
- Metrics collection for key business and performance indicators.
- Distributed tracing across service boundaries.

### Security

- Authentication and authorization logic separated from business logic.
- Audit logging for key domain events.
- Secrets loaded from environment or secret manager - never hardcoded.

### Reliability

- Explicit retry and circuit breaker strategies.
- Health-check mechanisms.
- Fall-back or degraded-service strategies.

## When Helping with Legacy Code

When an engineer asks for help modifying legacy code:

1. **Assess the situation.** Read the code. Check for tests. Identify coupling and architectural drift. For a broad evaluation, use `/codebase-assessment`.
2. **Identify seams.** Find places where behavior can be altered without editing the code at that point (object seams, link seams, preprocessing seams).
3. **Suggest the right technique** based on the situation:
   - New to this codebase? Use `/codebase-assessment`
   - Need to understand existing behavior? Use `/characterization-test`
   - Need to add new behavior cleanly? Use `/sprout-method`
   - Need to wrap or extend existing behavior? Use `/wrap-and-decorate`
   - Need to break dependencies for testing or modernization? Use `/dependency-breaker`
   - Need to understand complex code before changing it? Use `/scratch-refactor`
   - Need to understand the blast radius of a change? Use `/effect-analysis`
   - Need to find seams for safe modification? Use `/seam-finder`
   - Ready to incrementally replace a module or subsystem? Use `/strangler-fig`
   - Need to swap an internal implementation behind an abstraction? Use `/branch-by-abstraction`
   - Need the full change workflow? Use `/legacy-code-change`
4. **Converge on current patterns.** Every change is an opportunity to reduce pattern entropy and move toward the end-state ideals.

## Key Vocabulary

- **Seam**: A place where you can alter behavior without editing the code at that point.
- **Enabling point**: The place where you decide which behavior to use at a seam.
- **Characterization test**: A test that documents actual current behavior, not intended behavior.
- **Pinch point**: A narrow place where a single test can cover many code paths.
- **Interception point**: Where you can detect the effects of a change via a test.
- **Effect sketch**: A diagram of how a change propagates through the system.
- **Pattern entropy**: Inconsistency that accumulates as conventions evolve but old code doesn't.
- **Strangler fig**: Incrementally replacing legacy code by growing new code around it.
