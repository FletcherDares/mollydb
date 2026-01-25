# DX Improvements Plan

## Goal
Improve the Developer Experience (DX) for the MollyCache project by automating routine tasks and enforcing standards locally, reducing the feedback loop time compared to waiting for CI.

## 1. Task Runner (`just`)

Introduce `just` (a modern alternative to `make`) to standardize common development workflows.

### Proposed `Justfile`:
```makefile
# List available commands
default:
    @just --list

# Format code
fmt:
    cargo fmt --all

# Run clippy
lint:
    cargo clippy -- -D warnings

# Run all tests
test:
    cargo test

# Run code coverage (requires tarpaulin)
coverage:
    cargo tarpaulin --ignore-tests

# Full verification (format check, lint, test) - Good for pre-commit
verify:
    cargo fmt --all -- --check
    cargo clippy -- -D warnings
    cargo test
```

## 2. Git Hooks

Implement a `pre-commit` hook to catch issues before they are pushed. This ensures the "clean code" philosophy is maintained at the source.

### Proposed `.git/hooks/pre-commit` (or managed via a script in `scripts/setup-hooks.sh`):
```bash
#!/bin/sh
# Ensure code is formatted
cargo fmt --all -- --check
if [ $? -ne 0 ]; then
    echo "Code formatting failed. Run 'cargo fmt' or 'just fmt' first."
    exit 1
fi

# Ensure code compiles and lints pass
cargo clippy -- -D warnings
if [ $? -ne 0 ]; then
    echo "Clippy check failed."
    exit 1
fi
```

## 3. Clippy Configuration

Add a `clippy.toml` or configure `Cargo.toml` to enforce specific lints that align with the project's philosophy (e.g., performance checks, idiomatic Rust).

### Proposed Lints:
- `clippy::pedantic` (selectively enabled)
- `clippy::perf` (highly relevant for in-memory DB)
- `clippy::unwrap_used` (to encourage proper error handling, though `expect` might be allowed in tests)

## Implementation Steps
1. Create `Justfile` in root.
2. Create `scripts/setup-hooks.sh` to install git hooks.
3. Update `README.md` to mention `just` and the setup script.
