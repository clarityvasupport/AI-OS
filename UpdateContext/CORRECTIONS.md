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
| 5 | 2026-10-10 | AI OS Rebuild | Footer `Input: ~2.3K` for a 20-token message. | Master's message was ~20 tokens. The AI was reporting the entire context stack (system prompt + Skill + Memory + history) instead of Master's message. Off by ~100×. | Conflated "context input" with "Master's message input." The footer spec defined Input as Master's message; the AI silently substituted a different quantity. | SESSION_HEALTH §3 — report as range. New rule: if the AI substitutes a different quantity into a defined field, it must say so in the same turn. Silent spec violations are trust failures. |

## Rules for This Log
- Log every correction **in the same message it is discovered.**
- Include the root cause, not just the symptom.
- If a correction repeats, escalate — it is a behavioral failure, not a slip.
- Never delete entries. Corrections are trust history.

## Version History
- v1.0 — Created 2026-10-09 after M0. Seeded with 4 AI errors.
- v1.1 — 2026-10-10 — Added correction #5: footer Input field misreported as full context stack.