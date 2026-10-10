# CORRECTIONS.md
> Purpose: Running log of AI errors and their fixes.
> Trust record. Read when the AI seems to repeat itself.
> If a correction repeats, the AI is failing at its own rule.

## Format
| # | Date | Milestone | AI claimed | Truth | Root cause | Rule applied |
|---|---|---|---|---|---|---|

## Corrections Log

| # | Date | Milestone | AI claimed | Truth | Root cause | Rule applied |
|---|---|---|---|---|---|---|
| 1 | 2026-10-09 | M0 | "`cloudflare.config.ts` does not exist. The real file is `wrangler.jsonc`." | `cloudflare.config.ts` **does** exist and is correct for the `cf` path. The handoff was right all along. | Stated an assumption as a "verified" fact without checking. | SESSION_HEALTH §7 — Three-Strike Rule. Never call something verified without checking. |
| 2 | 2026-10-09 | M0 | "ChatGPT Sol reads ~1.6% higher than DeepSeek." | Sol/cl100k differ by ~0.5%. The benchmark cited was for GPT-4o, not Sol. | Generalized from a mismatched benchmark. | Same. Flagged as unverified claim. |
| 3 | 2026-10-09 | M0 | "Sample text is ~1,000 tokens." | Actual count: 680 tokens. AI was ~47% over. | Used a 4 chars/token assumption on pure prose (real ratio ~5.4). | SESSION_HEALTH §2 — recalibrated against real measurements. |
| 4 | 2026-10-09 | M0 | Estimates presented as precise numbers. | They are ranges, ±20–40% error. | Habitual precision language without measurement. | AI_RULES anti-rule added: never present an estimate as a measurement. |

## Rules for This Log
- Log every correction **in the same message it is discovered.**
- Include the root cause, not just the symptom.
- If a correction repeats, escalate — it is a behavioral failure, not a slip.
- Never delete entries. Corrections are trust history.

## Version History
- v1.0 — Created 2026-10-09 after M0. Seeded with 4 AI errors.