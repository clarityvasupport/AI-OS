# AI_OS_REBUILD_HANDOFF.md
> Version: v1.0
> Status: Live layer complete. Repo layer complete. Ready for Project creation.
> Next step: **Create the BDS Project, then run the Project token test.**
> Build status: Extension configured. Repo rebuilt. Project not yet created.

---

## UPDATE — what changed this session

### Discovery
- Studied the Better DeepSeek (BDS) extension's capabilities and limitations.
- Key finding: the extension provides a **Skill** (fires every session), a **Project** (holds context), **Memory** (persistent pointers), and a **System Prompt** layer — a native replacement for the paste-based context ritual.

### The architectural pivot
The old AI OS was built for an AI that forgets. The extension removes that limitation. The system was rebuilt around three layers:

| Layer | Holds | Fires |
|---|---|---|
| **Skill** | Behavioral rules | Every session, automatically |
| **Project** | Context files | Per chat, retrieved on demand |
| **Repo** | Source of truth | Retrieved on demand |

### Live layer — configured
- **System prompt entry "AI OS Protocol"** — created, set to fire.
- **Default prompt** — kept active (Multi-System mode on, both firing).
- **Skill "Master Protocol"** — final version, fingerprint `5180`. Contains: context retention rule, the gate, one-step-at-a-time, address, session gate, communication, formatting, proactive behavior, never rules, persona, escalation, three-strike rule, footer (Env/Msg format), handoff thresholds, BDS environment note.
- **Memory entries** — three saved:
  - `user_identity` (always) — solo developer building an AI OS, address as "Master"
  - `current_project` (called) — AI OS protocol repository on GitHub
  - `workflow` (called) — re-imports the repo at each milestone

### Repo layer — rebuilt
- **Renamed** `Coding/` → `Core/`.
- **Created** `Projects/` folder; moved `CRM_SAAS_HANDOFF.md` into it.
- **Created** `PROTOCOL.md` — the new entry point.
- **Rewrote** `CONTEXT_INDEX.md` (v2.0), `HOW_TO_START_A_CHAT.md` (v2.0), `README.md` (v1.0), `manifest.json` (v2.1).
- **Deleted** `PREAMBLE.md`, `Coding/AI_RULES.md`, `Coding/AGENT.md` — content now lives in the Skill.

### Corrections applied
- **CORRECTIONS.md** — added correction #5: the footer Input field was misreported as the full envelope instead of Master's message (off by ~100×). Bumped to v1.1.
- **Footer format fixed** — replaced `Input/Output` with `Env/Msg`. `Env` = the whole envelope (what fills the 300K cap). `Msg` = Master's message only.
- **Skill updated** to reflect the new footer and add the rule: if a different quantity is substituted into a defined field, say so in the same turn.

---

## TAG — where we are

**Complete:**
- ✅ Live layer (Skill + prompts + Memory)
- ✅ Repo layer (structure, files, manifest)
- ✅ Corrections logged
- ✅ Footer format corrected

**Open — not blocking:**
- ⚠️ **`CRM_SAAS_LEGAL_DRAFT.md`** — referenced in `CONTEXT_INDEX.md` v2.0 as "not yet committed," but not present in the repo. Decision needed: commit the file, or remove the reference.
- ⚠️ **Project files behavior unknown** — unverified whether Project files auto-inject into every prompt (would burn ~20–30K/turn) or retrieve on demand (free). README suggests retrieval, but this is unconfirmed.
- ⚠️ **Extension version** — repo is on `v0.1.14`. Latest is `v0.1.15`. Not urgent. The BDS memory parser bug (swallowing closing tags on URL syntax) may or may not be fixed in v0.1.15.

**Known bugs encountered this session:**
- BDS memory parser garbles values containing URL syntax. Workaround: write memory as plain prose.
- `AUTO:REQUEST_GITHUB_FETCH` failed multiple times. `AUTO:REQUEST_WEB_FETCH` on a raw GitHub URL worked. Route around it when needed.
- Re-creating a skill with the same name creates a duplicate, not an update. Delete the old one manually.

---

## NEXT — immediate next action

**Step 1 — Create the BDS Project "AI OS"** (lean):
- Instructions: the gate, non-negotiables, session gate
- Files: `PROTOCOL.md` only
- Everything else stays fetchable, not attached — until the token test proves otherwise

**Step 2 — Run the Project token test:**
1. Start a chat with **no** Project attached. Note the Session number after one turn.
2. Start a chat **with** the Project attached. Note the Session number after one turn.
3. Compare. The delta tells us whether Project files auto-inject.

**Step 3 — Decide on the MCP layer:**
- Only after the Project test.
- Purpose: enable GitHub commits without manual download/paste.
- Not yet designed. Not yet approved.

---

## Open Questions for Master
1. `CRM_SAAS_LEGAL_DRAFT.md` — commit it, or remove the reference?
2. Project instructions — start with the template from this session, or revise first?
3. MCP — after the Project test, or skip entirely?

---

## Version History
- v1.0 — 2026-10-10 — Initial handoff. Documents the extension-driven rebuild of the AI OS.