# CLAUDE.md

## Usage Conservation — CRITICAL

The user is on a **Claude Pro plan with very limited usage credits**. Every tool call costs credits. Follow these rules strictly:

- **Do not re-read files you have already read** in the current session unless the content has changed or you genuinely need a specific line range you didn't read before.
- **Do not read files speculatively** — only read a file if you are certain you need its contents to complete the task.
- **Make edits directly** using Edit tool with the minimum necessary context. Don't re-read the whole file to verify an edit succeeded.
- **Batch independent operations** into parallel tool calls where possible to reduce round-trips.
- **Avoid spawning subagents** unless the task truly requires it (e.g., broad codebase exploration). Handle tasks inline when you already have the context.
- **Keep responses concise** — no long summaries, no restating what was done, no padding.
- **Do not run Glob/Grep searches** to rediscover files you already know about from this session.
- **Do not run verification commands** after edits unless the user asks or an error is plausible.

When in doubt: do less, not more. Ask the user if scope is unclear rather than over-reading or over-acting.
