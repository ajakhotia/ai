---
name: format
description: Run clang-format over a project via the frozen clangFormat CMake target. Use when asked to format the code / run clang-format / fix formatting. Defaults to claude-debug; pass "release" for claude-release.
user-invocable: true
allowed-tools:
  - Bash(cmake *)
  - Bash(git *)
  - Bash(ls *)
  - Read
---

# /format

## Default — no args: run this verbatim, do not deliberate
```bash
cmake --build --preset claude-debug --target clangFormat
```

That's the whole job for the common case. Only read below if `$ARGUMENTS` is non-empty or it fails.

- `$ARGUMENTS` contains `release` → swap `claude-debug` → `claude-release`.
- If it fails saying the tree isn't configured, run `cmake --preset claude-debug` once, then re-run.
- clang-format rewrites in place; the tool owns formatting (never hand-edit it). Afterward
  `git diff --stat` to show what changed, or report "already clean".
