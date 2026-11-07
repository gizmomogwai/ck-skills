---
name: dlang
description: >
  Use when writing D code, setting up a D project, or changing its build and
  tests. Use dub.sdl, an LDC Nix development shell, unit-threaded, explicit
  output paths, and D-safe identifiers.
---

# D project workflow

## Package recipe and compiler

- Use `dub.sdl` for every package, including example and main subpackages.
  Preserve dependency and configuration semantics when translating a JSON recipe.
  `dub.selections.json` is still the dependency lock file; keep it as JSON.
- Set up `flake.nix` with a default development shell containing `pkgs.ldc`
  and `pkgs.dub`. Reuse existing flake structure and inputs. Include native
  build tools and libraries only when the project needs them, and advertise
  only supported platforms.
- Run builds and tests with `--compiler=ldc2`. Use `nix develop --command`
  when the compiler and dependencies come from the development shell.

## Library, examples, and main program

When there is reusable logic or multiple entry points, make the root DUB
package a library. Keep shared code under `source/`; example programs and
an optional main program depend on that library rather than copy its code.

- Declare example subpackages with `subPackage "examples/<name>"`.
- Put an optional main program in `main/` with its own `dub.sdl`, declared
  with `subPackage "main"`.
- Give each executable its own `mainSourceFile`. Keep executable entry points
  outside the library's source paths.
- For a small standalone tool with no useful library boundary, keep one
  executable package. Library/example scaffolding is conditional, not mandatory.

Set `targetPath` and `targetName` explicitly. Paths are relative to the
package's recipe, not the repository root:

| Recipe | `targetPath` |
|---|---|
| Root standalone executable | `out/main/<program>` |
| `main/dub.sdl` | `../out/main/<program>` |
| `examples/<name>/dub.sdl` | `../../out/examples/<name>` |
| Root library | `out/lib/<package>` |
| Root test configuration | `out/tests/<package>` |

Keep generated runners and other generated artifacts under `out/` as well.
Ignore `/out/` in version control. Update callers, scripts, and documentation
when an output path changes.

## Formatting
Fully {} every statement (also single expressions of an if/while block).
Use UFCS when possible.
Format code with dub run dfmt -- -i source (no options).

## Tests: unit-threaded

- Use [unit-threaded](https://github.com/atilaneves/unit-threaded) for testing.
  Add a bounded released dependency in the `unittest` configuration, keeping
  it out of normal library and application builds.
- Prefer an explicit test runner using `mixin runTestsMain!(...)` with the
  modules containing tests. Add new test modules to that list. Use a generated
  runner only when maintaining the list is impractical.
- Configure `unittest` as an executable with `mainSourceFile` pointing to
  that runner and `targetPath` under `out/tests/`. Define the normal build
  configuration first, so a regular build does not select the test runner.
  Exclude any application `main` from the test build.
- Keep test-only imports, assertions, and UDAs in test modules or behind a
  package-specific test version / `Have_unit_threaded`. A consumer building
  with unittests must not unexpectedly need the library's test dependency.
- Name tests with string UDAs; use the framework's assertions for
  useful failures. Run the smallest relevant test selection.
- Tests run concurrently by default. Use `--single` for process-global state
  or main-thread-only APIs. `@Serial` only serializes tests within a module;
  it does not replace single-thread mode for those constraints.

```sh
nix develop --command dub build --compiler=ldc2
nix develop --command dub test --compiler=ldc2
nix develop --command dub test --compiler=ldc2 -- --single
nix develop --command dub run --compiler=ldc2 :demo
```

The last command assumes an example subpackage named `demo`; use the actual
subpackage name. Verify that the runner discovers and executes the intended
tests, and that executable artifacts land at the configured output paths.

## Identifier checks

Before naming modules, fields, parameters, functions, or generated bindings,
consult [reserved-keywords.md](reserved-keywords.md). Common traps include
`body`, `out`, `scope`, `version`, `debug`, `alias`, `module`, `package`,
`function`, `delegate`, `ref`, and `with`.

Choose a descriptive alternative, such as `result` instead of `out`.
If an API must retain a keyword-like name, append one underscore (`out_`);
do not change capitalization to evade the keyword. Declare no custom
identifiers beginning with `__`; compiler-provided names may still be used.

## Sources

- [DUB build settings](https://dub.pm/dub-reference/build_settings/)
- [DUB configurations](https://dub.pm/dub-reference/configurations/)
- [DUB subpackages](https://dub.pm/dub-reference/subpackages/)
- [unit-threaded runner options](https://github.com/atilaneves/unit-threaded/blob/master/subpackages/runner/source/unit_threaded/runner/options.d)
