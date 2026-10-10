# manifest.md — AI-OS File Map

> Human-readable mirror of `manifest.json`. The extension reads this file; tooling reads the `.json`. **Update both together when the file map changes.**

| Field | Value |
|---|---|
| Repo | `clarityvasupport/AI-OS` |
| Branch | `main` |
| Architecture | Skill + Project + Repo |
| Version | 2.2 |
| Updated | 2026-10-10 |

## Files

| Path | Tier | Description |
|---|---|---|
| `README.md` | meta | Project overview. Explains the three-layer architecture. |
| `PROTOCOL.md` | meta | The entry point. Describes how the AI OS works. |
| `manifest.json` | meta | Machine-readable map of every file. |
| `manifest.md` | meta | Human-readable mirror of `manifest.json`. Read by the extension. |
| `Core/MASTER_PROFILE.md` | core | Who Master is, how to communicate. |
| `Core/SKILLS.md` | core | Technical skills, stack, known gotchas. |
| `Core/SESSION_HEALTH.md` | core | Token calibration, footer detail, hallucination guard. |
| `Projects/AI_OS_REBUILD_HANDOFF.md` | project | Full state of the AI OS rebuild. |
| `Projects/CRM_SAAS_HANDOFF.md` | project | Full state of the CRM SaaS project. |
| `UpdateContext/CONTEXT_INDEX.md` | context | Map of the pack. What exists and when to retrieve it. |
| `UpdateContext/GLOSSARY.md` | context | Plain-language definitions of every term. |
| `UpdateContext/CORRECTIONS.md` | context | Running log of AI errors and fixes. |
| `UpdateContext/DECISION_LOG.md` | context | Chronological decisions and reasons. |
| `UpdateContext/HOW_TO_START_A_CHAT.md` | context | Master's ritual under the new architecture. |

## Removed

| Path | Reason |
|---|---|
| `PREAMBLE.md` | Rules moved to the Master Protocol Skill. |
| `Coding/AI_RULES.md` | Behavioral rules moved to the Master Protocol Skill. |
| `Coding/AGENT.md` | Role definition moved to the Master Protocol Skill. |
