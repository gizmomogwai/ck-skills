---
name: rusty
description: Run every cargo command with CARGO_TERM_QUIET=true so its output stays short. Use before running any cargo command (build, check, test, clippy, run, fmt, doc, ...), also when cargo is one line inside a larger shell command, in a scratch project, or behind a wrapper such as rtk.
---

# Rusty

Cargo prints a lot of progress lines (`Compiling ...`, `Downloading ...`, `Finished ...`). They tell the agent nothing, but they fill the context. Quiet mode hides them. Errors, warnings and program output still show.

## Rule

Run every cargo command with `CARGO_TERM_QUIET=true`. Use `env`, so it works in every shell (bash, zsh, fish):

Bad:

```sh
cargo build
cargo test --workspace
```

Good:

```sh
env CARGO_TERM_QUIET=true cargo build
env CARGO_TERM_QUIET=true cargo test --workspace
```

## With a command wrapper

When the instructions also ask for a wrapper such as `rtk`, use both. The variable goes first:

```sh
env CARGO_TERM_QUIET=true rtk cargo build
```

The wrapper shortens cargo's output but still prints its own summary and the `Finished` line. Quiet mode removes those too.

## Scope

- This is for commands you run yourself. Do not add it to scripts, CI workflows or instructions for humans unless asked.
- If you need the hidden output to debug something (e.g. which crate is compiling), run the command once without it.
