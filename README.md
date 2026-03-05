# Working Effectively with Legacy Code - Claude Code Configuration

A starting-point [Claude Code](https://docs.anthropic.com/en/docs/claude-code) configuration that equips engineers with proven techniques for maintaining and evolving legacy codebases. Language-agnostic and framework-agnostic - drop the .claude/ directory into any legacy project and start working.

## What This Is

A .claude/ directory containing:

- `CLAUDE.md` - Instructions that orient Claude Code toward safe, incremental legacy code modernization.
- skills - Invocable workflows for specific legacy code situations, from initial assessment through to full system replacement.

## Quick Start

Copy the .claude/ directory into your legacy project:

Then start Claude Code in your project. The configuration is active immediately.

## Usage with Rovo Dev CLI

[Rovo Dev CLI](https://support.atlassian.com/rovo/docs/use-rovo-dev-cli/) uses a different configuration structure to Claude Code, but the same instructions and skills can be used with a small amount of translation.

### Configuration mapping

|Claude Code |Rovo Dev CLI |
|-------------------------------------------|------------------------------------------|
|`.claude/CLAUDE.md` |`.agent.md` in the project root |
|`.claude/commands/*.md` (slash commands) |Saved prompts (via `/prompts`) |
|`~/.claude/CLAUDE.md` (global instructions)|`~/.rovodev/.agent.md` (global agent file)|

### Steps

1. Copy the project instructions into .agent.md at the root of your legacy project:

   cp .claude/CLAUDE.md .agent.md

1. Recreate the skills as saved prompts — the contents of each .claude/commands/*.md file can be saved as prompts in Rovo Dev. Add them interactively using /prompts once inside a session.

1. Run Rovo Dev CLI from your project root:

   acli rovodev run

Rovo Dev will automatically read .agent.md and apply the project instructions to the session. The .claude/ directory will be ignored.

> Note: You can keep both .claude/CLAUDE.md and .agent.md in the same project to support both tools side by side. Add .agent.local.md to .gitignore if you want personal overrides without affecting the shared configuration.

## Skills

|Skill |Purpose |
|------------------------|--------------------------------------------------------------------------|
|`/codebase-assessment` |Assess a legacy codebase's current state - the entry point for the toolkit|
|`/legacy-code-change` |The full Legacy Code Change Algorithm - orchestrates all other skills |
|`/characterization-test`|Document actual current behavior before making changes |
|`/seam-finder` |Find safe points to intercept behavior for testing or modification |
|`/dependency-breaker` |Break coupling to make code testable and changeable |
|`/sprout-method` |Add new behavior cleanly alongside legacy code |
|`/wrap-and-decorate` |Extend existing behavior via wrapping (Decorator pattern) |
|`/scratch-refactor` |Explore and understand complex code without committing changes |
|`/effect-analysis` |Map the blast radius of a proposed change |
|`/strangler-fig` |Incrementally replace modules or subsystems at system boundaries |
|`/branch-by-abstraction`|Incrementally replace implementations behind an abstraction layer |

## What We Mean by Legacy Code

This configuration treats legacy code as code reflecting outdated design choices, accumulated technical debt, and pattern entropy - not simply "code without tests." Legacy code may be well-tested and working, but its design has drifted from current understanding, conventions, or requirements.

## Customization

The CLAUDE.md includes an End-State Ideals section describing qualities to converge toward when modernizing (separation of concerns, dependency injection, testing standards, observability, etc.). This section is explicitly marked for customization - adapt it to match your team's target architecture and conventions.

## Principles

- Preserve behavior first - verify before changing.
- Small, safe steps - each change provably preserves behavior.
- New code should be clean - sprouts and wraps follow current standards.
- Don't fix everything at once - incremental improvement, not big-bang rewrites.
- Converge on current patterns - reduce pattern entropy with every change.
- Know when NOT to apply - well-tested, well-understood code doesn't need the full ceremony.
