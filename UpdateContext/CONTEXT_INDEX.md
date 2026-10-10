# CONTEXT_INDEX.md
> Purpose: Map of every file in the AI OS. Use it to know what exists and when to retrieve it.
> This file does not contain rules. The rules live in the Skill.

---

## The layers

| Layer | Where | Fires when |
|---|---|---|
| **Skill** | BDS Skill "Master Protocol" | Every session, automatically |
| **Project** | BDS Project "AI OS" | Every chat in the project |
| **Repo** | This repository | Retrieved on demand |

## Tier 1 — Core (retrieve when needed)

| File | Purpose | Retrieve when |
|---|---|---|
| `Core/MASTER_PROFILE.md` | Who Master is, how to communicate. | Resuming work, calibrating tone. |
| `Core/SKILLS.md` | Technical skills, stack, known gotchas. | Technical or build work. |
| `Core/SESSION_HEALTH.md` | Token calibration, footer detail, hallucination guard. | Auditing the footer, checking calibration. |

## Tier 2 — Context (retrieve as needed)

| File | Purpose | Retrieve when |
|---|---|---|
| `UpdateContext/GLOSSARY.md` | Plain-language definitions. | AI uses an unfamiliar term. |
| `UpdateContext/DECISION_LOG.md` | Chronological decisions and reasons. | Resuming a project — understanding why things are the way they are. |
| `UpdateContext/CORRECTIONS.md` | Log of AI errors and fixes. | AI seems to repeat a mistake. |
| `UpdateContext/HOW_TO_START_A_CHAT.md` | Master's ritual. | Reference only — mostly superseded by the Skill. |
| `UpdateContext/CONTEXT_INDEX.md` | This file. | When unsure what exists. |

## Tier 3 — Project (one per project)

| File | Purpose | Retrieve when |
|---|---|---|
| `Projects/CRM_SAAS_HANDOFF.md` | Full state of the CRM SaaS project. | Working on CRM SaaS. |
| `Projects/CRM_SAAS_LEGAL_DRAFT.md` | Legal draft for attorney review. | Discussing legal. *(Status: not yet committed — see flag below.)* |

## Loading order

The Skill fires automatically. After confirming session type:

1. **Type A (new)** — Core only.
2. **Type B (resume)** — Core + the project handoff.
3. **Type C (general)** — Core only.
4. **Type D (debug)** — Core + handoff + the broken file.

Never retrieve more than the session type requires.

## Folder structure

```
AI-OS/
├── README.md
├── PROTOCOL.md
├── manifest.json
├── Core/
│   ├── MASTER_PROFILE.md
│   ├── SKILLS.md
│   └── SESSION_HEALTH.md
├── Projects/
│   └── CRM_SAAS_HANDOFF.md
└── UpdateContext/
    ├── CONTEXT_INDEX.md
    ├── GLOSSARY.md
    ├── CORRECTIONS.md
    ├── DECISION_LOG.md
    └── HOW_TO_START_A_CHAT.md
```

## Open flag

**`CRM_SAAS_LEGAL_DRAFT.md`** — referenced in the previous index v1.1 but **not present in the repo**. Either it was never committed, or it lives elsewhere. Confirm and either commit it or remove the reference. Tracked for resolution.

## Maintenance

- Add new files here when created.
- Version bump when structure changes.
- Keep it honest. If a file is not used, remove it.

## Version History

- v1.0 — Initial context pack.
- v1.1 — Added PREAMBLE.md. Added Tier 2 files. Clarified loading order.
- **v2.0 — 2026-10-10 — Rewritten for the Skill-based architecture. Rules moved to Skill. Folders renamed (Coding → Core). Projects folder introduced. PREAMBLE.md, AI_RULES.md, AGENT.md removed.**