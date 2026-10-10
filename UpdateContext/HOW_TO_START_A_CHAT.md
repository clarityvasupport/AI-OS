# HOW_TO_START_A_CHAT.md
> Purpose: Master's ritual for starting chats under the new architecture.
> The old paste ritual is gone. The Skill and Project handle context now.

---

## The Ritual

### Step 1 — Open a new chat
The Skill fires automatically. The Project carries the files.

### Step 2 — Confirm the session type
The AI will ask which applies:

| Type | Meaning |
|---|---|
| **A** | New project |
| **B** | Resume a project |
| **C** | General chat |
| **D** | Debugging |

Answer in one line: "B" or "C — general".

### Step 3 — The AI retrieves what it needs
No pasting. The AI pulls from the Project based on session type and lists what it retrieved. If something is missing, it says so.

### Step 4 — Begin
Only after the AI confirms it has what it needs.

---

## What changed from v1.0

| Old ritual | New ritual |
|---|---|
| Paste `PREAMBLE.md` first | Not needed — the Skill asks the session type automatically |
| Paste `CONTEXT_INDEX.md` | Not needed — the AI knows the index |
| Paste requested files one by one | Not needed — the Project holds them |
| Confirm the AI received the files | Still happens — the AI lists what it retrieved |

**Net effect:** fewer steps, no pasting, same discipline.

---

## Between-Chat Hygiene

- **Before closing a chat** — if the AI flags the 35% threshold, draft a handoff. At 50%, the handoff is required.
- **After closing a chat** — save the new handoff version to `Projects/` and commit.
- **After a milestone** — re-import the repo so the Project context stays fresh.

---

## What NOT to Do

- Don't paste files manually. The Project has them. If it doesn't, that's a bug — fix the Project, not the habit.
- Don't skip the session gate. The type determines what gets retrieved.
- Don't start work before the AI confirms it has what it needs.

---

## Version History

- v1.0 — 2026-10-09 — Original paste ritual.
- **v2.0 — 2026-10-10 — Rewritten. The paste ritual is replaced by the Skill + Project architecture.**