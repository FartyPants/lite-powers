---
name: claude-handoff
description: Hand the current conversation off to a fresh background Claude session that picks up the work immediately.
argument-hint: "What will the next session be used for?"
disable-model-invocation: true
---

1. Write the handoff document exactly as [handoff](../handoff/SKILL.md) describes. It becomes the new agent's prompt, so redaction matters doubly.
2. Launch a background session seeded with it, using the Bash tool (Git Bash on Windows) so the file content is passed verbatim, with no backtick or `$` expansion:

   ```bash
   claude --bg --name "<short descriptive name>" -- "$(cat "<handoff file>")"
   ```

   Always set `--name` (e.g. `"Fix login bug"`); it's the label in `claude agents`, the session picker and the terminal title. The session starts in the current working directory and the command returns immediately.
3. Tell the user the name and the handoff file path. They manage the session with `claude agents`, `claude attach <name>` and `claude logs <name>`.
