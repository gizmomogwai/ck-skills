---
name: no-short-options-in-instructions-and-scripts
description: Use long command-line options (--recursive, not -r) in scripts and in written instructions, and comment every short option that cannot be avoided. Use whenever writing or editing a shell script, Makefile, CI workflow, mise/just task, Dockerfile, README, doc page, runbook, or any command meant for someone else to read or run.
---

# No short options in instructions and scripts

Scripts and instructions are read far more often than written. `-rf` makes the reader look up the man page. `--recursive --force` does not.

## Rule

Write the long form of every option in:

- scripts: shell, Makefile, CI workflows, mise/just tasks, Dockerfiles, git hooks
- instructions: READMEs, docs, runbooks, comments, commit or PR text with commands, commands handed to the user

Bad:

```sh
rm -rf build
git commit -m "fix"
curl -sSL -o out.tar.gz "$url"
```

Good:

```sh
set -o errexit -o nounset -o pipefail  # not: set -euo pipefail
rm --recursive --force build
git commit --message "fix"
curl --silent --show-error --location --output out.tar.gz "$url"
```

## When a short option is unavoidable, comment it

Some tools have no long form, or the script must run where long options are missing (BSD/macOS coreutils, busybox, POSIX `sh` builtins). Keep the short option. Put a comment on the same line, or the line above, saying what each flag does and why it is short.

```sh
# -p: no error if it exists, create parents. Short: BSD mkdir on macOS has no --parents.
mkdir -p "$dir"

tar -xzf "$archive"  # x extract, z gunzip, f file. Short: busybox tar has no long options.
```

## Scope

- Interactive commands you run yourself in a terminal session are exempt. The rule covers text that persists or that someone else reads or runs.
- Leave untouched lines in existing files alone. Fix the lines you change.
- Single-letter subcommand-style arguments that are not options (e.g. `chmod u+x`) are not affected.
