# C++

## Children

# C++ style, idioms & patterns

These apply to **all** C++ equally — public API, implementation, and **test code** alike. (Testing as a practice is the sibling `testing` node; the *framework* is project-specific. Doc-comment conventions are the sibling `documentation` node.) Nothing here is test-specific: idioms like value-init and IIFE are C++ practices wherever C++ is written.

## The clang toolchain (C++ instance of *mechanics to tools* — see `software`)
- clang-format + clang-tidy enforce all formatting, identifier naming, and a broad lint surface as build errors. Don't memorize the knobs — write the idioms below so there's little to fix, and act on the diagnostics. When clang-tidy names a fix (e.g. prefer-member-initializer), apply **exactly** that fix; reach for `// NOLINT(<check>)` only when genuinely unavoidable, with precedent.
- External-authority tie-breaker when idioms conflict: C++ Core Guidelines > Meyers (*Effective Modern C++*) > Sutter.

## Naming
- Types / structs / enums / type-aliases / template parameters → `PascalCase`. Functions / methods / variables / parameters → `camelCase`. Data members (public or private) → `m` + `PascalCase` (`mValue`). Named compile-time constants → `k` + `PascalCase` (`kMaxCount`). Namespaces → `lower_case`.
- **Name by role, not type** — no noise words or type suffixes (the most-missed rule; see `software` for the general naming principle).
- Template parameters are descriptive nouns — never `T`/`U`, never `FooT`. When a parameter is re-exported as a member `using` alias, the parameter takes a trailing `_` and the alias drops it (`template<typename ValueType_> ... using ValueType = ...;`). A never-aliased parameter may take a plain name.
- Acronyms are cased as words (`Se3`, `HttpClient`).

## const-correctness & attributes
- Locals are `const` by default; mutable only when genuinely mutated. Read-only scalar params are `const` even by value.
- `[[nodiscard]]` on essentially every value-returning accessor / pure function. `noexcept` wherever it holds (including conditional `noexcept(...)`).
- Accessors come in const / non-const overload pairs; the read path is `noexcept` and returns by `const&` or value. Use `constexpr`/`consteval` wherever compile-time evaluation is possible; `= default`-ed `operator==` on value types.
- **`const` data members are a manual judgment** (the relevant lint is disabled): a `const` member kills copy/move-assignment, so reserve it for genuine immutability and leave value types assignable.
- **Pointer members don't propagate `const`** — `const` on `this` freezes the handle, not the pointee, so a `const` method can still mutate through a raw pointer member (a silent hole in logical const). Fix with a const-propagating pointer wrapper, or a private accessor pair (`T& foo()` / `const T& foo() const`) used everywhere instead of the raw member. Prefer a **value member** when ownership allows (it propagates `const` for free); reach for a pointer only when forced (polymorphism, shared/weak ownership).

## RAII & ownership (lean in hard)
- Constructors do the real work; destructors release symmetrically. Don't split a class's own construction into post-construction `init()`/`load()` + `shutdown()` where a ctor/dtor will do — the reader's first guess is "read the constructor"; reward it. (Free factory functions are still fine for *value production* / named-constructor patterns.)
- Ownership lives in the type: `unique_ptr` for sole ownership, `shared_ptr` for shared lifetime, raw `T&`/`T*` only as a **non-owning borrow** (documented "borrowed, not owned; must outlive ..."). Create with `make_unique`/`make_shared` — never raw `new`/`delete`. Manual element lifetime uses `construct_at`/`destroy_at`.
- A scope-guard RAII type (entry action in ctor, exit action in dtor) handles acquire/release pairs; mark such types `[[nodiscard]]` when discarding an instance is a bug.
- **Typed handle, not `void*`/`shared_ptr<void>`, when the type is known**; erase a type only on a genuine type-erasure boundary, never to dodge an include cycle. (Derive-don't-store generally → `software`.)

## Initialization
- Members are **always** set in the member-init list, never assigned in the ctor body. Base classes first, then members in declaration order.
- **Brace-init by default** (`mFoo{value}`). Use **parens** specifically to avoid a greedy `initializer_list` ctor (some JSON / matrix types), or when forwarding into a storage member.
- Default member initializers for trivially-defaulted state, braced, with explicit literal suffixes (`bool mReady{false};`, `std::uint64_t mId{0ULL};`). Single-argument and forwarding ctors are `explicit`. Use delegating ctors to funnel overloads into one.
- **Value-init idiom (Almost Always Auto):** prefer `auto x = T{...};` over `T x;`; an empty object is `auto x = T{};`. Loop counters get an explicit braced type (`for(auto i = std::size_t{0}; ...)`), never a bare `0`. Digit separators in numeric literals (`1'000'000`); designated initializers for aggregates.

## Class layout
- Order **by visibility**: `public:` → `protected:` → `private:`. **Within each section: members first, then methods** — i.e. aliases/nested types → data members (`const`/`constexpr` first, then mutable) → methods.
- The **rule of five is always spelled out** (`= delete` for resource owners, `= default` for value types), as a contiguous group; move ops and overriding dtors are `noexcept`. Member declaration order is deliberate (it drives construction/destruction order) — comment it when load-bearing.
- `struct` = passive aggregate / value type with public data (still `m`-prefixed members); `class` = anything with invariants or behavior. Definitions default to the `.cpp` (keep a body inline in the header only with reason — templates, compile-time-visible `constexpr`/`consteval`, header-only); in the `.cpp`, define methods in the same order they're declared.

## Function signatures
- **Leading return types** by default. Use a **trailing return type only when it earns its place**: lambdas, deduction guides, and out-of-line definitions whose return type is a class-scope name (a nested type or base-class type) — there a trailing return resolves through class scope, so the name stays unqualified. More generally, **don't qualify a name reachable unqualified**.
- **Choose the parameter form that states the truth about ownership:** cheap → by value; read-only big → `const&` (strings → `string_view`); kept copy / consumed move-only sink → **by value then `std::move`** (preferred over `T&&` for sinks); in-out → `&`; concrete known-type sink → `T&&` + `std::move` (move the parameter itself); relay of unknown category → forwarding ref + `std::forward`; in-place build → `Args&&...` + `std::forward`. Return by value. Default arguments for optional behavior knobs.

## Idioms
- **Errors are exceptions**, funneled through one throwing helper that prefixes source location; catch once at the top level on `const std::exception&`. No error-code returns or out-params for errors.
- Spelled-out logical keywords: `not`, `and`, `or` — never `!`, `&&`, `||`. `std::optional` for "maybe a value", never sentinel pointers/out-params.
- Constrain templates with **concepts + `requires`**; guard type relationships with `static_assert(cond, "msg")`. CRTP for static-polymorphism interfaces. `enum class` with explicit underlying type; consume via a local `using`; remap in a `switch` whose `default:` **throws** (never silent fallthrough/blind cast).
- `auto` pervasively for locals (braced-type init supplies the type). Range-based `for` with `const auto&` for objects, `const auto` for trivial scalars. Prefer ranges algorithms and **associative-container access by intent** (`contains`/`at`/`emplace`/`try_emplace`) over `find` unless you need the iterator.
- A value produced in a restricted scope but used after it → compute it in an **IIFE `[&]{ ... }()` bound to a `const`**, not declare-then-assign (beware: an early-return or assert *macro* inside the IIFE returns from the lambda, not the enclosing function). `static_cast<void>(...)` to explicitly discard a `[[nodiscard]]`. Write deduction guides where deduction is non-obvious.

## Headers
- `#pragma once`, never include guards. Includes: own/same-module headers in `"quotes"` first, then a **single alphabetically-sorted block** of `<angle-bracket>` includes (std + third-party + cross-module together). Cross-module includes use the full path. No forward declarations — full includes.
- **Never a namespace-/file-scope alias in a header** (`using`/`typedef` at namespace scope) — it injects a name into every including TU and usually leaks an implementation type. Member aliases inside a class are fine; a local `using` in a function body or `.cpp` is fine. Default to spelling the type out at use sites; reach for even a member alias only when the type recurs many times. If an include cycle blocks a shared type, spell it out in both places or restructure — never header-alias (or fall back to `void`) to dodge the cycle.
