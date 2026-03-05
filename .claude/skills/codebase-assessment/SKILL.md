---
name: codebase-assessment
description: Assess a legacy codebase to understand its current state before planning modernization. Use when first encountering a legacy codebase, when an engineer asks "where should we start?", or when you need to understand the landscape before recommending techniques.
---

# Codebase Assessment

Evaluate a legacy codebase's current state to identify what exists, where the pain is, and where to focus modernization effort.

## When to Use

- First time working with a legacy codebase.
- An engineer asks "where should we start?" or "what's the state of this code?"
- Before planning a modernization strategy.
- When you need to understand existing conventions before making changes.

## Process

### 1. Technology Landscape

Discover and document:
- **Language(s) and versions** in use.
- **Build system** and how to build/run the project.
- **Test framework(s)** and how to run tests.
- **Dependency management** and key dependencies.
- **CI/CD** pipeline if present.

### 2. Architectural Shape

Identify the current structure:
- **Module/package organization** - how is the code organized? By layer, by feature, by component, or seemingly at random?
- **Architectural style** - is there an identifiable pattern (MVC, layered, hexagonal, microservices, monolith)? Or has it drifted?
- **Boundary clarity** - are there clear separations between business logic and infrastructure, or is everything tangled?
- **Entry points** - where does execution start? What are the public APIs, CLI commands, event handlers?

### 3. Test Coverage and Quality

Assess:
- **Test existence** - are there tests? Where? What kind (unit, integration, end-to-end)?
- **Test health** - do they pass? Are they maintained? Are they fast or slow?
- **Coverage gaps** - which areas have no tests? These are the highest-risk areas for change.
- **Test patterns** - what conventions do existing tests follow? (naming, structure, assertion style)

### 4. Hotspot Analysis

Identify the areas most in need of attention:
- **Change frequency** - which files change most often? (check git history: `git log --format=format: --name-only | sort | uniq -c | sort -rn`)
- **Coupling** - which modules have the most dependencies on other modules?
- **Complexity** - which files/classes/methods are the largest or most complex?
- **Pain points** - ask the engineer: what's hardest to change? What breaks most often?

### 5. Pattern Entropy

Assess consistency:
- **Competing patterns** - are there multiple ways the same thing is done? (e.g., three different HTTP client wrappers, two DI approaches)
- **Generational layers** - can you see distinct eras of development with different styles?
- **Dead code** - are there modules, classes, or methods that appear unused?
- **Current conventions** - what patterns does the team use in the most recent code? This is likely the target style.

### 6. Dependency Health

Evaluate:
- **Outdated dependencies** - are core dependencies significantly behind current versions?
- **Abandoned dependencies** - are there dependencies that are no longer maintained?
- **Tight coupling to libraries** - is business logic entangled with specific framework or library APIs?

## Output

Deliver:
- A summary of the technology landscape
- An architectural sketch of the current structure
- A test coverage assessment with key gaps identified
- A prioritized list of hotspots and pain points
- An assessment of pattern entropy and current conventions
- Recommended starting points for modernization, with suggested techniques from the toolkit
