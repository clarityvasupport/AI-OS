# DECISION_LOG.md
> Purpose: Chronological log of decisions and reasons.
> Read when resuming a project to see why things are the way they are.
> The handoff summarizes state. This file explains history.

## Format
| # | Date | Project | Decision | Reason | Reversible? |
|---|---|---|---|---|---|

## Decisions Log

### CRM SaaS

| # | Date | Decision | Reason | Reversible? |
|---|---|---|---|---|
| 1 | 2026-10-06 | Package manager: **npm** | Simpler, universally compatible, ships with Node. | Yes, but costly. |
| 2 | 2026-10-06 | Node version: **22** | Latest LTS. Locked via devcontainer + `.node-version`. | Yes, but breaks lock. |
| 3 | 2026-10-06 | Deploy target: **Cloudflare Workers + vinext** | Cloudflare now recommends vinext for Next.js. Pages path is legacy. | Hard — foundational. |
| 4 | 2026-10-06 | Cloudflare management: **Dashboard only** | Master prefers no CLI. Workers Builds runs commands server-side. | No — working style. |
| 5 | 2026-10-09 | Dev server port: **5173** (vinext/Vite default) | Align devcontainer forwardPorts with actual port. | Yes. |
| 6 | 2026-10-09 | README: **keep original** (Option A) | The scaffold's README is generic. We'll write ours when the app does something. | Yes. |
| 7 | 2026-10-09 | Devcontainer: **bullseye → bookworm** | `workerd` requires glibc 2.35+. Debian 12 ships 2.36. | Yes, but dev server breaks. |
| 8 | 2026-10-09 | Compatibility date: **2026-10-09 → 2026-10-07** | Local `workerd` binary supports up to 2026-10-07. Match the binary, don't update it. | Yes. |
| 9 | 2026-10-09 | Git LFS hook: **removed** | Not needed. Codespaces did not ship `git-lfs`, blocking push. | Yes. |
| 10 | 2026-10-09 | Operational token budget: **300K, handoff at 50%** | Conservative cap for sharper reasoning and lower drift. | Yes — Master's call. |
| 11 | 2026-10-09 | External tokenizer: **Veprompts (ChatGPT Sol Pro)** | Sol approximates DeepSeek within ~0.5% on mixed content. | Yes. |
| 12 | 2026-10-09 | Footer on every AI response | Visibility into token usage and rules consulted. | Yes. |
| 13 | 2026-10-09 | File structure: **AI OS with Coding/ + Update Context/** | Keeps rules, references, and projects separated. | Yes. |
| 14 | 2026-10-09 | `GLOSSARY.md` — include in every project resume | Cheap (~2.5K tokens), high value, prevents jargon detours. | Yes. |

## Rules for This Log
- Add a row whenever a decision is finalized.
- Never delete rows. Decisions are history.
- If a decision is reversed, add a new row noting the reversal — don't edit the old one.

## Version History
- v1.0 — Created 2026-10-09. Seeded with M0 decisions.