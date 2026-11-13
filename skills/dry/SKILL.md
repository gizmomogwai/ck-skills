---
name: dry
description: Keep every piece of knowledge in exactly one place and let everything else read from it. Use whenever writing or editing code, configuration, documentation, instructions or skills, and before adding a function, type, constant, setting, file or section that may already exist.
---

# DRY (don't repeat yourself)

Every piece of knowledge has one authoritative place. A change to that knowledge is then one edit, and two copies can never disagree.

## Rules

1. **Search before you add.** Look for a function, type, constant, setting, documentation section or skill that already holds the concept. Search for the concept, not for the name you would give it. Reuse or extend what you find.
2. **One source, many readers.** When two places need the same thing, both read it from one source: a shared file, a constant, a function, a generated output. Never copy it and keep the copies in sync by hand.
3. **Refer, do not restate.** Documentation, comments and instructions point to the source. They do not repeat its content.
4. **Merge the duplication you touch.** When your change needs the same edit in two places, merge the two places first. If that is outside the task, make both edits and tell the user about the duplication.

Bad: the same instructions kept as two files, one per tool.

```nix
home.file.".claude/CLAUDE.md".source = ./claude/CLAUDE.md;
home.file.".copilot/copilot-instructions.md".source = ./copilot/copilot-instructions.md;
```

Good: one file, linked twice.

```nix
home.file.".claude/CLAUDE.md".source = ./agents/instructions.md;
home.file.".copilot/copilot-instructions.md".source = ./agents/instructions.md;
```

## What is not duplication

- **Alike by accident.** Two pieces of code that look the same but change for different reasons hold different knowledge. Keep them apart.
- **A reader that does not exist yet.** Do not build a shared abstraction for a possible second user. Merge when the second real user arrives.
