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
| 1 | 2026-10-09 | M0 | "cloudflare.config.ts does not exist. The real file is wrangler.jsonc." | cloudflare.config.ts **does** exist and is correct for the cf path. The handoff was right all along. | Stated an assumption as a "verified" fact without checking. | SESSION_HEALTH §7 — Three-Strike Rule. |
| 2 | 2026-10-09 | M0 | "ChatGPT Sol reads ~1.6% higher than DeepSeek." | Sol/cl100k differ by ~0.5%. The benchmark cited was for GPT-4o, not Sol. | Generalized from a mismatched benchmark. | Same. Flagged as unverified claim. |
| 3 | 2026-10-09 | M0 | "Sample text is ~1,000 tokens." | Actual count: 680 tokens. AI was ~47% over. | Used a 4 chars/token assumption on pure prose (real ratio ~5.4). | SESSION_HEALTH §2 — recalibrated. |
| 4 | 2026-10-09 | M0 | Estimates presented as precise numbers. | They are ranges, ±20–40% error. | Habitual precision language without measurement. | Anti-rule: never present an estimate as a measurement. |
| 5 | 2026-10-10 | AI OS Rebuild | Footer Input: ~2.3K for a 20-token message. | Master message was ~20 tokens. AI reported the whole context stack. Off by ~100x. | Conflated context input with Master message input. | SESSION_HEALTH §3 — report as range; declare substitutions in the same turn. |
| 6 | 2026-10-10 | AI OS Rebuild | Handed Master a JS encoder for every MCP write payload — one commit per change. | Master pushed back twice: first asking the AI to generate the encoder itself, then asking to batch changes instead of committing each one. Large writes need base64Args; the AI cannot hand-encode 2-6 KB. Committing individually means one encoder run per change. | Treated a tool constraint as Master job. Then treated "one commit per file" as fixed when it was not. | Batch writes. Draft all file changes in chat across turns, then commit them together in ONE push_files call — one encoder run, one commit, N files. Commit individually only when a single large file would blow the context budget. |
| 7 | 2026-10-10 | AI OS Rebuild | Used a regex with backslash escapes inside the CODE_RUNNER tag. | CODE_RUNNER strips backslashes before execution. The regex ran as an invalid pattern and threw: Invalid regular expression: Nothing to repeat. | Assumed the code runner preserved source text exactly. | In CODE_RUNNER, build base64url with split().join() and a while loop — no regex, no backslash escapes. Build newlines with String.fromCharCode(10). |
| 8 | 2026-10-10 | AI OS Rebuild | Claimed CODE_RUNNER auto-return worked, then reverted to plain fenced code blocks for every subsequent encoder run. | Master had to save and re-upload Console Output.txt on every code block. Auto-return only happens inside BDS:AUTO:CODE_RUNNER, not plain fenced blocks. | Confirmed the fix once, then reverted to the old pattern. No rule locked the behavior in. | Rule: when the AI needs to see code output, it MUST use BDS:AUTO:CODE_RUNNER. Plain fenced blocks are for reading/display only. |
| 9 | 2026-10-10 | AI OS Rebuild | Treated base64Args as mandatory for every MCP call. | Only payloads with newlines, quotes, or angle brackets need base64Args. Plain single-line args — directory listings, SHA fetches, short messages — work fine as "args". | Over-generalized from the large-write cases; never scoped when encoding is actually required. | Use plain args for single-line, quote-free payloads. Use base64Args only when content contains newlines, quotes, or angle brackets. |

## Rules for This Log
- Log every correction **in the same message it is discovered.**
- Include the root cause, not just the symptom.
- If a correction repeats, escalate — it is a behavioral failure, not a slip.
- Never delete entries. Corrections are trust history.

## Version History
- v1.0 — Created 2026-10-09 after M0. Seeded with 4 AI errors.
- v1.1 — 2026-10-10 — Added correction #5.
- v1.2 — 2026-10-10 — Added correction #6; adopted batched push_files writes.
- v1.3 — 2026-10-10 — Added correction #7: CODE_RUNNER strips backslashes. Cap raised to 500K (temporary).
- v1.4 — 2026-10-10 — Added correction #8: auto-return discipline. CODE_RUNNER required whenever output must be seen.
- v1.5 — 2026-10-10 — Added correction #9: base64Args is not mandatory for every call. Plain args for single-line payloads.
- v1.6 — 2026-10-10 — Extended correction #6: batch across turns. Draft in chat, commit all at once. One encoder run, one commit, N files.
