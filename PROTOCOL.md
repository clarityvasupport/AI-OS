# PROTOCOL.md
> Purpose: The entry point. Read this first.
> This file describes how the AI OS works. It does not contain rules — the rules live in the Skill.

---

## What this is

The AI OS is a three-layer system for working with an AI across sessions without losing context.

| Layer | Lives in | Fires when |
|---|---|---|
| **Skill** | BDS Skill "Master Protocol" | Every session, automatically |
| **Project** | BDS Project attached to chat | Every chat in the project |
| **Repo** | This repo | Retrieved on demand |

## The three layers

### Skill — behavioral rules
The Skill carries the rules that must fire every session: how to address Master, the gate, the footer, handoff thresholds, communication style, the three-strike rule. It is always on. Nothing in this repo duplicates it.

### Project — context container
The BDS Project holds the repo files and project-level instructions. It is attached to the chat. Files are retrieved on demand — never all at once.

### Repo — source of truth
This repo. The Skill says how to behave. The repo holds what the AI needs to know: who Master is, what the stack is, what decisions were made, what errors happened, what the current project state is.

## The session gate

At the start of every new chat, confirm session type:

| Type | Meaning | Context used |
|---|---|---|
| **A** | New project | Core + Project instructions |
| **B** | Resume project | Core + handoff |
| **C** | General chat | Core only |
| **D** | Debugging | Core + handoff + the broken file |

## What's in the repo

| Path | Purpose |
|---|---|
| `PROTOCOL.md` | This file. The entry point. |
| `manifest.json` | Machine-readable map of every file. |
| `Core/MASTER_PROFILE.md` | Who Master is, how to communicate. |
| `Core/SKILLS.md` | Technical skills, stack, gotchas. |
| `Core/SESSION_HEALTH.md` | Token calibration, footer detail, hallucination guard. |
| `Projects/` | One handoff per active project. |
| `UpdateContext/CONTEXT_INDEX.md` | Full map of the pack. |
| `UpdateContext/GLOSSARY.md` | Plain-language definitions. |
| `UpdateContext/CORRECTIONS.md` | Running log of AI errors. |
| `UpdateContext/DECISION_LOG.md` | Chronological decisions. |
| `UpdateContext/HOW_TO_START_A_CHAT.md` | Master's ritual. |

## The handoff convention

When context reaches 50% of the 300K cap — or Master calls it — the AI writes a handoff in three sections:

- **UPDATE** — what changed this session
- **TAG** — where we are now
- **NEXT** — the immediate next action

Handoffs live in `Projects/`. One per project. Versioned.

## Version History

- v1.0 — 2026-10-10 — Rewritten for the Skill-based architecture. Replaces `PREAMBLE.md`.