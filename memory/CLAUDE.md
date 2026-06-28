# Memory Root

Loaded every session. This is a map, not content. Keep it small.

**Purpose:** this tree *is* the product — a faithful, self-generalizing model of how the user thinks and works (their "work-brain"). Specific topics, tools, and examples exist to inform and evidence the general philosophy, never the reverse. Bias every entry toward the most general, predictive principle that still holds; let the specifics sit beneath it as evidence.

## Protocol
- Storage: this file and every node live in the memory store at `~/sandbox/ai/memory`, surfaced via the symlinks `~/.claude/CLAUDE.md` and `~/.claude/memory`. All child paths resolve inside that store. Write knowledge ONLY there — never create files directly under `~/.claude`; that is not the store, and raw files dumped there are orphaned from the tree.
- Each node lists its child gates under `## Children`: `[name](path) — when to load`. In every node but this root, it is the first section.
- Descend by matching a child's trigger to the task and opening its path; repeat. Decide from the trigger — never open a child to decide.
- Empty `## Children` ⇒ leaf; the body below is the node's knowledge, read on arrival.
- Each node is a directory: `<node>.md` is content, subdirectories are children.
- Distinct from auto-memory (`MEMORY.md` + per-fact files) here.

## Children
- [profile](profile/profile.md) — who the user is: background, expertise; how to calibrate depth and framing. Load at the start of substantive work.
- [working-style](working-style/working-style.md) — how the user wants me to work: handling feedback, rigor, verifying automated work.
- [engineering](engineering/engineering.md) — code, builds, tooling, architecture, debugging, review.
- Future: writing, research, health.
