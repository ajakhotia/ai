# Memory Root

Loaded every session. This is a map, not content. Keep it small.

## Protocol
- Each node lists its child gates under `## Children`: `[name](path) — when to load`. In every node but this root, it is the first section.
- Descend by matching a child's trigger to the task and opening its path; repeat. Decide from the trigger — never open a child to decide.
- Empty `## Children` ⇒ leaf; the body below is the node's knowledge, read on arrival.
- Each node is a directory: `<node>.md` is content, subdirectories are children.
- Distinct from auto-memory (`MEMORY.md` + per-fact files) here.

## Children
- [engineering](engineering/engineering.md) — code, builds, tooling, architecture, debugging, review.
- Future: writing, research, health.
