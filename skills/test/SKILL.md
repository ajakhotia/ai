---
name: test
description: Build and run a CMake/CTest project's tests against the frozen claude tree. Use when asked to run tests / ctest / the test suite. Defaults to claude-debug; pass "release" for claude-release.
user-invocable: true
allowed-tools:
  - Bash(ctest *)
  - Bash(cmake *)
  - Bash(ls *)
  - Read
---

# /test

## Default — no args: run this verbatim, do not deliberate
```bash
cmake --build --preset claude-debug && ctest --test-dir build/claude-debug --output-on-failure
```

That's the whole job for the common case. Only read below if `$ARGUMENTS` is non-empty or it fails.

- `$ARGUMENTS` contains `release` → swap `claude-debug` → `claude-release` in both commands.
- Any other token → append `-R <token>` to ctest (test-name regex filter).
- If it fails saying the tree isn't configured, run `cmake --preset claude-debug` once, then re-run.
- Report pass/fail counts and the full output of any failing test — never call a failure a pass.
