## Children
- [inCode](inCode/inCode.md) — documenting code for its maintainer/caller: doc comments, API/interface docs, docstrings.
- [userFacing](userFacing/userFacing.md) — docs that onboard or convince an outside reader: READMEs, tutorials, guides.

# Documentation

Principles common to all documentation; the children instantiate them for code readers vs. outside readers.

- **Optimize for the reader and document to the contract** (see `software`): say what a thing is *for* and how to use it; the reader should not need the body. Put each point where its reader first looks.
- **Introduce before use; define every term on first use, never in terms of itself** — order the material so each concept is grounded the first time it appears, and coin domain terms inline. Use **one name per concept**, matching the code, consistently throughout (single source of truth for vocabulary).
- **Say each fact once** (single source of truth applied to prose — see `software`): never restate what the name, signature, or an earlier sentence already said.
- **Write for the actual audience's level**, in plain words and short sentences, no filler or hedging — calibrate to who reads this artifact, from a maintainer to a brand-new evaluator.
