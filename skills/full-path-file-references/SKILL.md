---
name: full-path-file-references
description: Always reference files with their full absolute path in the workspace. Use whenever a reply, review finding, summary or subagent prompt mentions a file, e.g. "piece.toml" or "config.rs:1191".
---

# Full-path file references

Every file reference in user-visible output uses the full absolute path, never a bare filename or a repo-relative path.

- Bad: `piece.toml:174`, `mosaic/piece.toml:174`, `config.rs:1191`
- Good: `/home/christian-koestlin/ESRLabs/quantum/workspace/mosaic-core-rs/mosaic/piece.toml:174`

Rules:

1. Resolve the path before citing it. Many filenames repeat across the workspace (e.g. ~37 `piece.toml` files), so a bare name is ambiguous.
2. Keep the `:line` suffix when a line is relevant.
3. Apply to subagent prompts too, and demand full paths back from subagents. Convert any relative path a subagent returns before relaying it.
4. Applies in every mode, including caveman/terse modes: the path is payload, not fluff.
