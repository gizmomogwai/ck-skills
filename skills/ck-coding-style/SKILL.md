---
name: ck-coding-style
description: Spell out every name in code (`destination`, not `dst`) and use types that carry their unit (`Duration`, not a bare integer). Use whenever writing or editing code in any language, including sketches, examples and snippets inside a chat answer, and also while a brevity mode such as ponytail or caveman is active.
---

# Coding style

Code is read far more often than written. A reader must never have to expand an abbreviation or guess a unit.

## Rules

**Unabbreviated names.** Spell out every name you choose: variables, parameters, fields, functions, types, constants.

Bad:

```rust
struct Packer { z: Compress, out: Vec<u8>, max: usize }
fn feed(&mut self, pkt: &[u8]) -> bool
```

Good:

```rust
struct Packer { compressor: Compress, output: Vec<u8>, maximum_size: usize }
fn feed(&mut self, packet: &[u8]) -> bool
```

More: `destination` not `dst`, `configuration` not `cfg`, `index` not `idx`, `return_code` not `rc`.

Names defined by an external API (`avail_in` of zlib) stay as they are.

**Rich types for units.** Use a type that encodes the unit, not a bare number. Prefer standard-library types (`std::time::Duration` in Rust, `time.Duration` in Go, `datetime.timedelta` in Python). Never pass a raw integer where the caller must guess whether it means milliseconds, seconds or minutes.

## Brevity modes

Ponytail, caveman and similar modes cut the amount of code and prose. They do not cut names: fewer statements, never shorter names.

## Scope

- Every piece of code you write: files, scratch scripts, one-off checks, snippets inside a chat answer.
- Command-line options are covered by the skill `no-short-options-in-instructions-and-scripts`.
