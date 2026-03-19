---
name: deploy
description: End-of-session deploy — commit and push to GitHub, update the session log, check off completed TODO items, and write any significant learnings to memory. Run this at the end of any work session or milestone.
---

You are executing the `/deploy` skill. Follow every step in order.

## Step 1 — Review what was done this session

Look back at the current conversation and identify:
- What was built, fixed, or changed
- Any bugs found and what caused them
- Decisions made that future sessions should know about
- Anything non-obvious or surprising that's worth remembering

## Step 2 — Update the TODO file

Find the project's TODO file (`CLAUDE-TODO.md`, `TODO.md`, or equivalent). Then:
- Check off completed items (`- [x]`)
- Add any new items discovered this session with appropriate difficulty ratings if the project uses them
- Remove anything that's no longer relevant

If no TODO file exists, skip this step.

## Step 3 — Update the session log

Find the project's session log (`CLAUDE-SESSION-LOG.md` or equivalent). Add one row summarizing what was done:

```
| YYYY-MM-DD | Short factual summary of what was completed |
```

If no session log exists, skip this step.

## Step 4 — Write to memory for significant items

If any of the following are true, write a memory file and update the memory index:
- A new pattern, system, or architectural decision was introduced
- A non-obvious bug was found and fixed (so it won't be repeated)
- The user corrected your approach or gave a preference during this session
- A new external service, API, or dependency was added

Skip memory if the session was routine with no lasting lessons.

## Step 5 — Commit and push

Stage all modified files. Write a clear commit message. Push to the main branch.

Commit format:
```
<concise summary of what was done>

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
```

## Step 6 — Report back

Summarize in plain language:
- Commit hash and what was pushed
- What was logged in the session log
- What (if anything) was written to memory and why

---

> **Claude Code users:** Copy this file to `.claude/skills/deploy/SKILL.md` in your project (or `~/.claude/skills/deploy/SKILL.md` for all projects) to use it as a slash command.
>
> **OpenClaw users:** This file is already in the right place. OpenClaw loads skills from `<workspace>/skills/` automatically.
