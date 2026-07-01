## Children

# In-code documentation

The in-code instance of the parent's documentation principles (and *document to the interface* — see `software`): document the **interface and how to use it**, never the implementation — a reader should tell whether this is the API they need, and how to call it, without opening the body.

## What gets a comment
- Document every declaration in the header — public and **private** alike. For a private member the audience is the maintainer: describe the contract (what it does, what it guarantees, what it throws, who calls it), still not the body.
- Skip the comment when the declaration is self-evident: a trivial alias (`using size_type = std::size_t`), an obvious getter. A comment that only echoes the code is worse than none.

## The entry, top to bottom
- **Summary (`@brief`)** — one sentence naming precisely what it is or does: the caller's-eye view for a function, the kind-of-thing-and-its-defining-behavior for a type. This is the reader's mental model, so stay on the entity: describe what is declared, not its collaborators (point to related or consumer APIs with `@see` instead), and never let a member's summary echo the enclosing type's. (Coin each domain term on first use — see parent.)
- **Example** — a tiny worked case, only when the signature alone leaves usage *or behavior* unclear: a typical call, or an input→output pair when the semantics are easier shown than told (a concrete example often teaches faster than a paragraph — reach for it before piling on prose). Format it exactly as the project's formatter would: read the config (e.g. `.clang-format`: column limit, bracket-breaking, bin-packing) and apply it; do not hand-wrap a line that fits.
- **Constraints & gotchas** — preconditions, ownership/lifetime, thread-safety, units, valid ranges, side effects, invalidation, what throws.

## Voice
- Write for a smart beginner: if a newcomer would stumble, simplify. (Plain words, short sentences, no hedging — see parent.)
- Full grammatical prose, concise and imperative — never telegraphic. Keep articles (a/an/the), and avoid noun stacks that read as imperative verbs ("Fill fraction" → "the fraction filled").

## Doxygen mechanics
- `@brief` for the summary; add `@param` / `@tparam` / `@return` / `@throws` only where they say something the signature does not.
- Attach a parameter's constraint to its own `@param` line.
- One space between a tag's name and its text — never aligned columns: `@param name text`, not `@param name    text`. Wrapped continuation lines sit at the base `///` indent, not aligned under the text.
- Combine references — `@see a, b` — and put a blank comment line between consecutive tags.
- Separate consecutive documented members with a blank line (between one member's declaration and the next member's doc comment), so each member reads as its own unit.

## Shape (template)
```
/// @brief <what this is or does, in plain words>.
///
/// Example:                 // only when usage is not obvious from the signature
///
///     <one minimal, typical call>
///
/// @tparam <T> <what to set it to / what it constrains>
///
/// @param <name> <only what the signature does not say; constraint attached here>
///
/// @return <meaning, units, or sentinel — only when non-obvious>
///
/// @see <relatedApi, otherApi>
```
