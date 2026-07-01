## Children

# User-facing documentation (READMEs, tutorials, onboarding guides)

The reader is an outsider deciding whether to adopt, then trying to get running — skimming, low-attention, no context and no special access. The job is to **convince and orient fast, then make the steps actually work.**

- **Lead with the payload, in attention order, because the reader quits early.** One sentence on what this is *for* → the single strongest differentiator, emphasized in proportion to its importance → a concrete, *familiar, runnable* example with a visual → only then the build/use boilerplate a skimmer skips. The reader must "get it" before the parts their eyes glaze over. (Pick the example by the teaching-artifact rule — see `software`.)
- **Ruthlessly short.** Attention is scarce; every section earns its place. Cut to the shortest that still convinces and instructs.
- **Anchor the human reader with visuals.** A color-coded diagram and emoji section-glyphs aid navigation and retention — use them, balanced against brevity.
- **Assume no privileged access.** Steps must run for a brand-new outsider: prefer credential-free forms (HTTPS remotes over SSH), name every prerequisite, and order them so each is satisfied before it is needed.
- **Audit from the target reader's perspective.** Periodically read it as the external evaluator and find the holes that erode trust or block adoption — unclear license, no maturity/status signal, platform/prereqs buried, no "how do I verify it worked" — then plug them.
- **Setup instructions are load-bearing and rot; verify by execution** (see `working-style`): every command, path, and pinned version must actually work in a clean environment, exercised by a representative naive consumer against the exact version under review — not your own expert reading.
