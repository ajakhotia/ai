---
name: tidy
description: Run clang-tidy over a project via the frozen clangTidy CMake target, then act on its findings. Use when asked to run clang-tidy / lint / tidy the code. Defaults to claude-debug; pass "release" for claude-release.
user-invocable: true
allowed-tools:
  - Bash(cmake *)
  - Bash(ls *)
  - Bash(git *)
  - Read
  - Edit
  - Grep
---

# /tidy

## Default — no args: run this verbatim, do not deliberate
```bash
cmake --build --preset claude-debug --target clangTidy
```

That's the whole job for the common case. Only read below if `$ARGUMENTS` is non-empty or it fails.

- `$ARGUMENTS` contains `release` → swap `claude-debug` → `claude-release`.
- If it fails saying the tree isn't configured, run `cmake --preset claude-debug` once, then re-run.
- Respond to findings: when clang-tidy names a fix, apply **exactly** that fix — never `// NOLINT` to
  keep a less idiomatic shape. Suppress only the genuinely unavoidable, and only with existing
  precedent in the codebase. Re-run after fixing to confirm a clean pass.
