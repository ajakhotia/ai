---
name: generalize-memory
description: Retrospect the just-finished conversation for durable learnings and assimilate the ones that generalize, then critically re-read the whole curated memory tree and escalate specifics into general, predictive principles — leaving memory cleaner, sharper, more concept-driven, and able to decide unseen cases. Use at the end of a conversation or effort to kick off the full introspect→assimilate→improve cycle, when asked to generalize / consolidate / tidy memory, after folding in a batch of new rules, or periodically.
user-invocable: true
allowed-tools:
  - Read
  - Edit
  - Write
  - Bash(ls *)
  - Bash(find *)
  - Bash(grep *)
  - Bash(cat *)
  - Bash(rm *)
  - Bash(rmdir *)
  - Bash(mkdir *)
---

# /generalize-memory

**Goal.** Run the full cycle of **introspection → assimilation → improvement** in one pass: mine the conversation just completed for durable learnings, fold the ones that generalize into memory, then converge the curated tree toward a **general philosophy of how the user works**. The tree *is* the product — a model of their work-brain; specific topics, tools, and examples exist to *inform and evidence* the generic structure, never to be the primary content. Each run leaves memory cleaner, sharper, more concept-driven, and **predictive** (principles that decide *unseen* cases) rather than **reactive** (a log of past corrections). Idempotent: a run that surfaces nothing new and over already-general memory should change little.

## The tree
The curated map lives at `~/sandbox/ai/memory` (root `CLAUDE.md`). Each node is a directory: `<node>.md` is content, subdirectories are children; every non-root node opens with `## Children` gates (`[name](path) — when to load`). Per-project auto-memory (a project's `MEMORY.md` + per-fact files) is separate; this skill curates the tree, and routes only genuinely project-specific facts to that project's auto-memory.

## Phase 1 — Retrospect & assimilate (the conversation just completed)
Run this first. If there is no substantive effort to mine (the skill was called cold), skip to Phase 2.
1. **Introspect.** Scan the conversation for signal: corrections the user gave, preferences they revealed, where I stumbled and the general reason why, an approach they confirmed as right. Express each as a *candidate principle*, not as the incident that produced it.
2. **Gate hard.** Keep a candidate only if it will make me better at *future, unseen* tasks. Being true, or a faithful record of what just happened, is **not** the bar. Drop problem-specific mechanics, one-off details, and baseline knowledge I already apply — recording those is clobber that dilutes everything else. (This is the `working-style` recording gate; when in doubt, leave it in the conversation, not the tree.)
3. **Assimilate the survivors.** Read the node(s) that would own each survivor and check current coverage first. Place it at the lowest node that covers it; **fold it into an existing bullet rather than adding a new one** wherever possible, and never duplicate what is already stated or merely implied — tighten the existing wording instead. A genuinely project-specific fact goes to that project's auto-memory (gated the same way), not the curated tree.

## Phase 2 — Converge the tree
1. **Read the whole tree** — every node, root to leaves. Hold the full picture at once; you are looking *across* nodes, not within one.
2. **Find recurring shape.** For each concrete rule ask: *what general principle is this an instance of?* and *does the same idea recur elsewhere under a different surface (another language, tool, framework, domain)?* Cluster the instances.
3. **Escalate to the lowest common ancestor.** State the principle — generically — at the lowest node that covers all its instances (two C++ rules + a CMake rule → the `software` node; something spanning engineering and writing → `working-style` or root). Strip tool/framework names from the principle itself.
4. **Demote specifics to evidence.** In each leaf, keep the concrete instantiation but trim the rationale now carried by the ancestor (you pass through the ancestor to reach the leaf, so the hierarchy does the work). A leaf rule should read as "here is how the general principle lands in this language/tool," often with a `(see <ancestor>)` pointer.
5. **Make it predictive.** Rewrite reactive single-case rules as principles that would already decide a *new* case (e.g. "use this linter's named fix" → "lean on tools for mechanics; apply their prescribed fix rather than fighting them"). A rule that genuinely can't generalize stays specific — that's fine.
6. **Consolidate & place deliberately.** Deduplicate; merge overlapping content; split a node only when a sub-topic loads independently. Keep each node small (map, not content). Create a new ancestor node only when several instances share a home that doesn't yet exist; update the parent's `## Children` gate when you do.

## Hard constraints
- **Ruthless economy.** Every word serves the objective or gets cut — filler, hedging, restatement, throat-clearing. A rule reduces to the words that decide a case; nothing else survives. Trim wording, never a real practice — concision is not dropped coverage.
- **Lossless reshaping.** Escalation moves and generalizes a rule; it never silently drops a real practice. When unsure whether something is still covered, keep it.
- **Concept over tool.** A general statement names no specific tool / framework / library / repo; those appear only as examples in leaves.
- **Public-safe.** This store is public — no project names, repo names, absolute paths, or verbatim private excerpts anywhere.
- **Protocol intact.** `## Children` stays the first section of every non-root node; the root stays small; the purpose statement and protocol at the root are not diluted.

## Finish
- Verify: every `## Children` link resolves to a real path (no dead gates), and a leakage scan finds no tool/project-specific terms in nodes meant to be general.
- Report, in this order: what I harvested from the conversation and where it landed (and what I gated out and why), then what I escalated and to where, what I merged, and what I deliberately left specific.
