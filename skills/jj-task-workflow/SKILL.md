---
name: jj-task-workflow
description: >
  Use when starting any new repository task. Create a fresh empty Jujutsu
  change and describe it before editing; keep each task in its own change.
---

# Jujutsu task workflow

1. Inspect `jj status`. Preserve existing work in its current change. For a
   Git-only repository, initialize Jujutsu with `jj git init`.
2. Run `jj new`, if you are not on an empty jj commit, then immediately run 
   `jj describe --message` with a concise
   Conventional Commits description of the task's intent. Dont include attribution.
3. Confirm `jj status` shows an empty, described working-copy change before
   editing files. Make the task's changes in that change and refine its description
   if the scope changes.
4. At completion, inspect `jj diff` and `jj status`. Leave the described change
   for review unless the user requests further history operations or publication.

Use Jujutsu for history operations. Start the next independent task with another
empty, described change rather than adding it to the previous task.
