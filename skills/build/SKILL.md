---
name: build
description: Build a CMake project using the frozen claude build preset. Use when asked to build / compile the project (optionally a specific target). Defaults to claude-debug; pass "release" for claude-release.
user-invocable: true
allowed-tools:
  - Bash(cmake *)
  - Bash(ls *)
  - Read
---

# /build

## Default — no args: run this verbatim, do not deliberate
```bash
cmake --build --preset claude-debug
```

That's the whole job for the common case. Only read below if `$ARGUMENTS` is non-empty or it fails.

- `$ARGUMENTS` contains `release` → swap `claude-debug` → `claude-release`.
- Any other token is a target → append `--target <token>`.
- If it fails saying the tree isn't configured, run `cmake --preset claude-debug` once, then re-run.
- Treat any compiler warning as a failure — report diagnostics faithfully and fix the cause, never
  suppress.
