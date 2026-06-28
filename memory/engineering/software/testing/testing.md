# Testing

## Children

# Testing practice (framework- and language-agnostic)

Testing is a software practice, independent of any language or framework. These are the design principles; the **choice of framework, assertion API, runner, and discovery mechanism is project-specific** (see the project's own notes / the relevant language node). When tests are written in a given language, that language's coding philosophy applies to the test code in full — tests are first-class code, held to the same standard as production code.

## Principles
- **Test the observable contract, not the implementation** (*depend on contracts, not implementations* — see `software`). A test should survive a refactor that preserves behaviour and fail when behaviour changes.
- **Name each test for the expected behaviour/outcome**, as a sentence (*name by role; optimize for the reader* — see `software`) — so the failure name alone tells you what broke, without reading the body. Group by the unit under test.
- **Arrange → Act → Assert.** Set up inputs, exercise the one thing under test, then verify — kept visually separated. (Spell it out; don't abbreviate to "AAA", which collides with unrelated coding idioms.)
- **One behaviour per test.** Prefer many small, focused tests over one sprawling case; a test that checks several things hides which one failed.
- **Cover the edges explicitly** — boundary values, empty/degenerate inputs, and error/exception paths — not just the happy path. Choose non-default inputs that would catch a hard-coded or deleted code path.
- **Distinguish hard preconditions from checks:** a precondition that must hold before the test can continue should abort that test; the actual assertions should otherwise report *all* failures, not stop at the first.
- **Reduce boilerplate without hiding intent:** push input construction into factories/builders and repeated verification into shared helpers/matchers, but keep each test readable on its own.
- **Report faithfully** (the honesty rule applied to tests — see `working-style`): a failing test is a failure — never massage output to look like a pass. Make failure messages say what was expected vs. what happened.
