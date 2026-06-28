---
name: configure
description: Configure a CMake project using the frozen claude preset. Use when asked to configure / set up / cmake-configure the build, or as a prerequisite before building, testing, formatting, or tidying. Defaults to claude-debug; pass "release" for claude-release.
user-invocable: true
allowed-tools:
  - Bash(cmake *)
  - Bash(ls *)
  - Read
---

# /configure

## Default — no args: run this verbatim, do not deliberate
```bash
cmake --preset claude-debug
```

That's the whole job for the common case. Only read below if `$ARGUMENTS` is non-empty or it fails.

- `$ARGUMENTS` contains `release` → use `cmake --preset claude-release` instead.
- On failure, surface the exact error verbatim — don't substitute another preset. Likely causes: the
  preset isn't defined in this tree, or the toolchain/environment it references isn't available here.
- Build tree is `build/<preset>`; `compile_commands.json` lands there.
