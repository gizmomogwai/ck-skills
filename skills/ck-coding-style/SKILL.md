---
name: ck-coding-style
description: >
  Readability-first coding style: long-form flags in scripts, unabbreviated
  variable names, and rich types for units. Code must be readable without
  decoding terse abbreviations or short flags.
triggers:
  - writing shell scripts
  - adding CLI commands to scripts, justfiles, or Makefiles
  - any flag or option passed to a shell command in a script
  - writing any code
---

## Core rules

**Long-form flags.** Always use long flags in scripts and files.
Short flags (`-e`, `-v`, `-r`) require mental lookup; long forms
(`--errexit`, `--verbose`, `--recursive`) are self-documenting.

**Unabbreviated names.** Variable and function names must be fully spelled
out. A reader must never have to mentally expand an abbreviation. `destination`
not `dst`, `configuration` not `cfg`, `index` not `idx`.

**Rich types for units.** Use a type that encodes the unit rather than a bare
number. Prefer standard-library types when available (e.g. `std::time::Duration`
in Rust, `time.Duration` in Go, `datetime.timedelta` in Python). Never pass a
raw integer where the caller must guess whether it means milliseconds, seconds,
or minutes.

## When long form is unavailable

Some flags have no long equivalent. `set -e` has `set -o errexit`, but not
every tool follows GNU conventions. In that case:

1. Check `--help` or `man` first.
2. If truly no long form exists: keep the short flag and add an inline comment.

```bash
sort -k2 -n  # --key=2 --numeric-sort (sort does not support long forms on this system)
```
