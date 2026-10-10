# AI-OS

> A three-layer system for working with an AI across sessions without losing context.

---

## What this is

The AI OS keeps an AI collaborator aligned, informed, and honest across long projects and many chat sessions. It splits the work into three layers, each with one job.

| Layer | Lives in | Fires when |
|---|---|---|
| **Skill** | BDS Skill "Master Protocol" | Every session, automatically |
| **Project** | BDS Project "AI OS" | Every chat in the project |
| **Repo** | This repository | Retrieved on demand |

## Why three layers

A chat session has no memory. An AI asked to help on a long project will drift — forget decisions, repeat mistakes, assume file contents it does not have.

The AI OS fixes this by splitting the problem:

- **The Skill** carries the rules that must fire every session — how to behave, how to address Master, when to stop, when to hand off. It is always on.
- **The Project** holds the repo so the AI can retrieve what it needs without Master pasting anything.
- **The repo** is the source of truth — the profile, the stack, the decisions, the corrections, the current project state.

## Start here

1. Read `PROTOCOL.md` — it explains the architecture.
2. Read `UpdateContext/CONTEXT_INDEX.md` — it maps every file.
3. Attach the BDS Project and confirm the session type when the AI asks.

## Structure

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

## The handoff convention

When context reaches 50% of the 300K cap, or Master calls it, the AI writes a handoff in three sections:

- **UPDATE** — what changed this session
- **TAG** — where we are now
- **NEXT** — the immediate next action

Handoffs live in `Projects/`. One per project. Versioned.

## Version History

- v1.0 — 2026-10-10 — Rewritten for the Skill-based architecture.