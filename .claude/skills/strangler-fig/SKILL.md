---
name: strangler-fig
description: Plan and execute incremental replacement of legacy modules or subsystems using the Strangler Fig pattern. Use when an engineer is ready to modernize a component by growing new code around it and progressively routing traffic/calls away from the legacy implementation.
---

# Strangler Fig

Incrementally replace legacy code by growing new implementations around it, progressively routing behavior to the new code until the legacy can be removed. Named after strangler fig trees that grow around a host tree until it is no longer needed.

## When to Use

- A module, subsystem, or service needs to be rewritten but a big-bang replacement is too risky.
- You want to modernize incrementally while keeping the system fully operational at every step.
- The legacy code has clear entry points where calls can be intercepted and redirected.
- You want the option to stop the migration at any point and still have a working system.

## Process

### Phase 1: Identify the Strangler Boundary

1. **Map the legacy component's interface.** What are all the entry points - API routes, method calls, event handlers, message consumers?

2. **Identify the interception layer.** Where can you intercept calls to route them to either old or new code?
   - API gateway or reverse proxy
   - Service facade or routing layer
   - Module-level entry points
   - Event/message router

3. **Assess shared state.** How do old and new implementations share data?
   - Shared database (pragmatic but creates coupling)
   - Event-based synchronization
   - Read from old, write to new (gradual source-of-truth migration)

### Phase 2: Slice and Prioritize

1. **Decompose into migration slices.** Each slice is a discrete piece of functionality that can be independently reimplemented and routed.

2. **Prioritize slices by:**
   - **Value**: Which slices cause the most pain or are changed most frequently?
   - **Risk**: Start with well-understood, low-coupling slices to build confidence.
   - **Independence**: Prefer slices with fewer dependencies on other legacy code.

3. **Define the contract** for each slice - what inputs, outputs, and side effects must the new implementation honor?

### Phase 3: Implement, Route, Validate (per slice)

1. **Build the new implementation** following end-state ideals:
   - Clean architecture boundaries
   - Full test coverage
   - Dependency injection
   - Observability built in

2. **Create or update the routing layer** to direct traffic for this slice:
   - Feature flags for gradual rollout
   - URL/path-based routing
   - Percentage-based traffic splitting

3. **Validate the new implementation:**
   - Run contract tests comparing old and new behavior
   - Shadow traffic: send requests to both, compare results, serve from legacy
   - Canary: route a small percentage to the new implementation
   - Monitor error rates, latency, and business metrics

4. **Cut over** when confidence is high. Remove the legacy code path for this slice.

5. **Repeat** for the next slice.

### Phase 4: Decommission

1. **Remove legacy code** for fully migrated slices.
2. **Simplify the routing layer** once all slices are migrated.
3. **Clean up shared state** mechanisms that were only needed during coexistence.

## Anti-Corruption Layer

When the legacy system has a different domain model or data structures, build an **anti-corruption layer** between old and new:

- Translates between legacy and modern domain models.
- Prevents legacy concepts from leaking into new code.
- Lives at the boundary and can be removed once the legacy is decommissioned.

## Guidelines

- **You can stop at any point.** A partially migrated system should always be fully operational.
- **Don't replicate legacy design.** The new implementation should follow current architecture, not mirror the old structure.
- **Keep the routing layer simple.** It becomes a critical path - complexity here is dangerous.
- **Monitor coexistence duration.** Long coexistence increases operational burden. Set milestones.
- **Not everything needs replacing.** Stable, well-designed legacy code can be left alone. Strangle the parts that cause pain.

## Output

Deliver:
- A migration plan with identified slices, priorities, and sequence
- The routing/interception strategy
- The data coexistence strategy
- Contract definitions for each slice
- Risk assessment and rollback approach
- A concrete first slice to implement
