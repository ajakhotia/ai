# CMake

## Children

# CMake style, project composition & workflow

Modern, target-centric CMake throughout.

## Project composition — CMake instantiation (concept in `engineering`, submodule mechanics in `software`)
- Submodules are declared **flat under the root's `external/`** and pulled into the build with explicit `add_subdirectory()`.
- `if(PROJECT_IS_TOP_LEVEL)` is the concrete *is-top-level* gate: infra `include()`s, the `clangFormat`/`clangTidy` targets, container-image targets, and `add_subdirectory()` of `external`/infra/`docker` live **only** inside that block, so a project never overreaches when built as a submodule.

## Authoring style
- **Commands lowercase; keyword arguments and operators UPPERCASE** (`find_package(Foo REQUIRED)`, `if((TARGET A) AND (TARGET B))`). 2-space indent, no tabs.
- Wrapper/function calls lay arguments out **one keyword per line, then each value on its own indented line**; the closing `)` is appended to the last argument (no dangling `)` on its own line).
- Section/multi-line commentary uses bracket comments `#[[ ... ]]`; inline rationale uses `#`; reusable helper functions get `##`-prefixed doc blocks. `message(STATUS ...)` announces structural actions.

## Targets — modern, target-scoped
- Define libraries/executables through **project wrapper functions** (an exported-library / exported-executable helper), not raw `add_library`/`add_executable`; raw `add_executable` appears only for test binaries (named `<unit>Test`).
- Target base names are unprefixed and match the module dir; consumed via a `<namespace>::` ALIAS. Library TYPE is left unset so `BUILD_SHARED_LIBS` controls it.
- **Strictly target-scoped commands with explicit visibility** (`target_include_directories/link_libraries/compile_features/...` with `PUBLIC`/`PRIVATE`/`INTERFACE`) — never global `include_directories`/`link_libraries`/`add_compile_options`. Standardize the C++ standard via `target_compile_features(... PUBLIC cxx_std_NN)`. Warning flags are always `PRIVATE`, via per-compiler generator expressions, one compiler per line. Public include dirs use the build/install dualism (`$<BUILD_INTERFACE:...>` + `$<INSTALL_INTERFACE:include>`).
- **No file globbing** — sources and public headers are listed explicitly, one per line. Subdirectories added with explicit `add_subdirectory(<name>)`.

## Helper functions
- Reusable helpers live in a shared infra/commons module and are pulled in via `include()` only at top level. Names are `lower_snake_case` verbs; private helpers get an `_impl` suffix.
- Keyword-driven via the standard `cmake_parse_arguments` boilerplate (`OPTIONS_ARGUMENTS` / `SINGLE_VALUE_ARGUMENTS` / `MULTI_VALUE_ARGUMENTS` then `cmake_parse_arguments("<PREFIX>_PARAM" ...)`). Prefix = function initials + `_PARAM`. Validate required args (fatal) and warn on soft-missing ones. Function-local temporaries are `UPPER_SNAKE` with the function-initials prefix. Prefer target properties over cache/global variables.

## Dependencies, install & export
- `find_package(... REQUIRED)` is the norm; config-mode with explicit `COMPONENTS` where applicable. **Dependencies are found in the leaf module that uses them**, not centrally.
- Each wrapper does its own `install(TARGETS ... EXPORT <set>)` into one shared export set. Top level installs the export set with a `<Project>::` namespace, generates a ConfigVersion (`COMPATIBILITY SameMajorVersion`), and `configure_package_config_file` + a manual `install(FILES ...)` of the generated config. The installed `Config.cmake.in` is `@PACKAGE_INIT@` → `find_dependency(...)` → include the targets file → `check_required_components(<Project>)`.
- `cmake_minimum_required` is pinned; `project()` declares an explicit version and `LANGUAGES`.

## Testing registration
- `include(CTest)` only at top level, guarded by test-framework target availability; each module guards `add_subdirectory(test)` behind `BUILD_TESTING`. Standard tests use the test framework's discovery mechanism; fall back to a single `add_test(...)` + `set_tests_properties(... ENVIRONMENT_MODIFICATION ...)` only when discovery can't run the binary (e.g. runtime library paths), with a comment explaining why.

## Workflow
- Modern CMake with presets. Each tree carries a committed `CMakePresets.json` and/or a user-only `CMakeUserPresets.json` (never committed). Configure / build / test / format / tidy run through **frozen, fixed-name presets** (a debug and a release variant) and **fixed-name `clangFormat` / `clangTidy` targets** — wired up as global skills, so they're invoked the same way in every project.
