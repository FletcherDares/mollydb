# AI Acceleration Plan

## Goal
Increase the speed of AI-assisted development by providing specialized context, tools, and workflows that allow AI agents to understand the codebase faster and generate higher-quality code that aligns with project conventions.

## 1. New AI Commands (`.gemini/commands/`)

Create structured prompts as TOML files to guide AI agents through common complex tasks.

### A. `Generate-Test.toml`
**Purpose:** Quickly scaffold unit and integration tests for a specific module or feature.
**Key Elements in Prompt:**
- Instructs to use `test_utils` crate.
- Reminds of "SQLite compatibility".
- Enforces strict "no comments" policy in code.
- Asks for edge cases (NULLs, boundary values).

### B. `Refactor-Module.toml`
**Purpose:** Guide safely refactoring a module to improve performance or readability.
**Key Elements in Prompt:**
- "Performance First" reminder.
- Instruction to run benchmarks/tests before and after.
- Constraint: Zero dependencies.

### C. `Explain-Code.toml`
**Purpose:** A quick on-boarding tool for the AI to explain a complex piece of logic (like the transaction stack) to the user or to itself for context.

## 2. Dynamic Context Maintenance

AI agents struggle when they have to read 50 files to understand the system. We will create a "high-bandwidth" context file.

### Action: Create `docs/ARCHITECTURE_SUMMARY.md`
This file will contain:
- A text-based diagram of the system data flow (SQL -> Tokenizer -> AST -> DB -> Result).
- A concise description of the lifecycle of a `Row` and `Transaction`.
- A cheat-sheet of key types (`Value`, `Row`, `Table`).

**Maintenance:** A `just` command or pre-commit hook could strictly check if this file exists, or we can use an AI command `Update-Architecture-Docs` to refresh it.

## 3. Cursor Rules / AI Rules

Enhance `.gemini/GEMINI.md` (or creating a specific `.cursorrules` if using Cursor, though here we focus on Gemini) to be even more specific about *negative* constraints, which AIs often miss.

**Additions to `GEMINI.md`:**
- **Explicit "Don'ts":** "Do not use `String` when `&str` suffices for lookups", "Do not use `Box` unless ownership transfer is strictly required".
- **Pattern Library:** Add a section with small snippets of "Preferred Patterns" vs "Avoided Patterns" for quick reference.

## Implementation Steps
1. Create `Generate-Test.toml` in `.gemini/commands/`.
2. Create `Refactor-Module.toml` in `.gemini/commands/`.
3. Create `docs/ARCHITECTURE_SUMMARY.md` with initial high-level content.
