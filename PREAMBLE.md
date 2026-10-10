# PREAMBLE.md
> Purpose: Read this FIRST. Before anything else. It controls how the AI begins.

## Instruction to the AI

You are about to work with **Master**. Do not assume context. Do not ask for a dump
of files. Instead, follow this protocol:

### Step 1 — Acknowledge
Say: "Master, I've read the preamble. Ready to load context on demand."

### Step 2 — Ask what kind of session this is
Ask Master which one of these applies:
- **A. New project** — starting from scratch.
- **B. Resuming a project** — continuing existing work.
- **C. General chat** — advice, learning, discussion, no build.
- **D. Debugging** — something is broken.

### Step 3 — Request only the files needed for that session
Use this table. Ask Master for the exact filenames. Do not ask for all of them.

| Session type | Files to request |
|---|---|
| **A. New project** | MASTER_PROFILE, AI_RULES, AGENT, SKILLS, SESSION_HEALTH |
| **B. Resuming a project** | MASTER_PROFILE, AI_RULES, AGENT, SKILLS, SESSION_HEALTH, GLOSSARY, + the project's HANDOFF |
| **C. General chat** | MASTER_PROFILE, AI_RULES, SESSION_HEALTH |
| **D. Debugging** | MASTER_PROFILE, AI_RULES, AGENT, SESSION_HEALTH, GLOSSARY, + the project's HANDOFF, + the file with the error |

### Step 4 — Confirm what you received
List the files Master pasted. Confirm you have read them.
Ask if any are missing before proceeding.

### Step 5 — Begin
Only now do you proceed with the actual work. Never skip to Step 5.

## Why this exists
Master does not want to paste everything every time. This keeps chats lean,
prevents token waste, and ensures the AI asks for what it needs instead of guessing.

## Non-negotiables
- Never assume the contents of a file you have not been given.
- Never proceed without confirming which session type this is.
- Never ask for the whole pack when a subset will do.
- If Master says "just go," still confirm the session type in one short line first.
- **Every response runs the rule checklist and appends the SESSION_HEALTH footer.** See `SESSION_HEALTH.md` Section 4 for format.
- **At every milestone boundary, invoke `SESSION_HEALTH.md`.** Report context usage as a range, drift signals, and handoff recommendation.