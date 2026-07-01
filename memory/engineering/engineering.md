# Engineering

## Children
- [software](software/software.md) — source code, builds, software tooling.

# Cross-cutting engineering principles

These hold for any engineering project, whatever the domain; the software node below instantiates them.

- **Compose any project from reusable components; the whole assembles, each component adapts to its role.** Build a project as a *root* that pulls in the components it needs — shared infrastructure and sibling projects — included **flat and non-recursively** (one copy of each; you never recurse into a component's own components). The root owns all integration: it selects the shared pieces, points at the right configurations/standards, and sets up the project-level tooling and deliverables. The decisive idea: the same component plays **two roles and behaves differently in each** — as the *root* it does all the setup; as an *included component* it stays dormant and defers to its consumer's root, so it never overreaches (gate setup on an *is-this-the-top* condition). Centralize the common infrastructure — toolchains/standards, scripts, shared parts and configs — in one shared component so every project reuses it and an improvement made anywhere propagates by **updating a single version pointer**; the leverage compounds with the number of consumers, which is the whole point at scale. This is single-source-of-truth and explicit ownership lifted to the composition level, and it is **domain-independent** — a mechanical or electrical assembly composes the same way (shared parts library, standards, a BOM), not just software.
