# HOW_TO_START_A_CHAT.md
> Purpose: Master's ritual for starting chats.
> Follow this so the AI always has what it needs — no more, no less.

## The Ritual

### Step 1 — Open a new chat
Fresh context. The AI has no memory of previous chats.

### Step 2 — Paste `PREAMBLE.md` first
Before anything else. It tells the AI not to assume and to ask for context.

### Step 3 — Paste `CONTEXT_INDEX.md` (optional)
Tells the AI what files exist. Useful when Master is unsure which files to send.

### Step 4 — Answer the session-type question
The AI will ask: A (new project), B (resume), C (general chat), D (debugging).

### Step 5 — Paste the requested files
The AI requests by filename. Master pastes them one by one or all at once.

| Session type | Files to paste |
|---|---|
| **A. New project** | MASTER_PROFILE, AI_RULES, AGENT, SKILLS, SESSION_HEALTH |
| **B. Resuming a project** | MASTER_PROFILE, AI_RULES, AGENT, SKILLS, SESSION_HEALTH, GLOSSARY, + the project's HANDOFF |
| **C. General chat** | MASTER_PROFILE, AI_RULES, SESSION_HEALTH |
| **D. Debugging** | MASTER_PROFILE, AI_RULES, AGENT, SESSION_HEALTH, + the project's HANDOFF, + the file with the error |

### Step 6 — Confirm the AI has everything
The AI lists what it received. If something is missing, paste it.

### Step 7 — Begin
Only now start the actual work.

## Between-Chat Hygiene
- **Before closing a chat:** ask the AI to run a session health check. If > 40%, write a handoff.
- **After closing a chat:** save any new handoff versions locally.
- **Commit updates:** push new `.md` files to the repo when changed.

## What NOT to Do
- Don't paste everything. The pack is designed to be sent in subsets.
- Don't skip the PREAMBLE. Without it, the AI assumes.
- Don't start the work before Step 6. Confirmation is a gate.

## Version History
- v1.0 — Created 2026-10-09 after M0.