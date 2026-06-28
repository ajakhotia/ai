---
name: clean
description: Nuke a CMake build directory for a fresh start. Use when asked to clean / wipe / nuke / blow away the build tree (e.g. before a clean reconfigure). Defaults to claude-debug; pass "release" for claude-release.
user-invocable: true
allowed-tools:
  - Bash(rm -rf build/claude-debug)
  - Bash(rm -rf build/claude-release)
  - Bash(ls *)
  - Read
---

# /clean

## Default — no args: run this verbatim, do not deliberate
```bash
rm -rf build/claude-debug
```

That's the whole job for the common case. Only read below if `$ARGUMENTS` is non-empty.

- `$ARGUMENTS` contains `release` → use `rm -rf build/claude-release` instead.
- Only ever remove `build/claude-debug` or `build/claude-release` — never a broader path, never the
  whole `build/` tree, never anything outside it. If asked to nuke a different directory, refuse and
  say so rather than widening the target.
- `rm -rf` on a missing directory is a successful no-op; report "already clean" in that case.
- After this, the tree must be reconfigured (`/configure`) before building, testing, formatting, or
  tidying.
