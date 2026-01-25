# Issue Title: Developer Experience (DX) & AI Acceleration Roadmap

**Description**

To improve development velocity and code quality, we are implementing a series of DX improvements and AI-specific workflows. This issue tracks the outstanding tasks discussed in our planning sessions.

## 1. Local Development Automation
- [x] **Setup Scripts**: Create cross-platform setup scripts (`scripts/setup.ps1`, `scripts/setup.sh`) to configure the environment.
- [x] **Git Hooks**: Implement `pre-commit` hook to enforce `cargo fmt`.
- [ ] **Task Runner**: Introduce `just` (or `Makefile`) to standardize commands:
    - `just verify`: Run fmt check, clippy, and tests.
    - `just coverage`: Run tarpaulin.
- [ ] **Linter Configuration**: Add `clippy.toml` to enforce project-specific lints (e.g., `clippy::perf`, `clippy::unwrap_used`).

## 2. Worktree Workflow
- [x] **Automation**: Create `molly` shell function for managing git worktrees (`molly -t feature-branch`).
- [ ] **Documentation**: Ensure team knows how to use the worktree workflow effectively.

## 3. AI Acceleration
- [ ] **Architecture Summary**: Create `docs/ARCHITECTURE_SUMMARY.md` as a high-density context file for AI agents (diagrams, key types, data flow).
- [ ] **AI Command Templates**: Add structured prompts in `.gemini/commands/`:
    - `Generate-Test.toml`: Scaffold tests using `test_utils`.
    - `Refactor-Module.toml`: Performance-focused refactoring guide.
    - `Explain-Code.toml`: Context gathering helper.
- [ ] **Rule Refinement**: Update `.gemini/GEMINI.md` and/or `.cursorrules` with specific "Negative Constraints" (what *not* to do) and preferred code patterns.

## 4. Git History Transparency
- [ ] **Attribution Standards**: Establish a standard for identifying AI-generated code.
    - **Method**: Use git trailers: `Co-authored-by: AI Assistant <ai@assistant>`.
- [ ] **Tooling**: Create a git alias (e.g., `git cai`) to automatically append this trailer to commits.

## Resources
- Plan details: `plan/dx_improvements.md`
- AI Plan details: `plan/ai_acceleration.md`
