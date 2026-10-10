# SESSION_HEALTH.md
> Purpose: Track context usage and guard against hallucination.
> Invoked on EVERY response — not just milestones.

---

## Section 1 — Operational Budget

**Denominator: 300,000 tokens** (Master's operational cap, not the model's maximum).

The model can hold 1M. Master has chosen a conservative 300K budget for sharper reasoning and lower drift.

**Handoff thresholds are OVERRIDDEN.** Per Master's standing instruction:

| Usage | Action |
|---|---|
| < 50% | Continue freely. |
| 50–99% | Continue working. **Do NOT stop for handoff.** |
| **100%** | Handoff required — write it at the cap, not before. |

The 35%/50%/65% escalation ladder is suspended. Do not initiate handoff at any intermediate threshold. Only an explicit Master instruction or a 100% reading triggers a handoff.

---

## Section 2 — Verified Calibration (M0 session)

Two real measurements through Veprompts:

| Sample | Content | Chars/token |
|---|---|---|
| Synthetic prose (573 words) | Pure English | 5.43 |
| CRM handoff v0.6 (1,643 words) | Markdown + tables + code | 3.74 |

**Effective ratio for our mixed-content conversation: ~3.8 chars/token.**

| Content type | Chars per token |
|---|---|
| Pure English prose | ~5.4 |
| Mixed markdown + code + tables | ~3.8 |
| Dense code / terminal output | ~3.3 |
| Symbols, JSON, dates | ~2.5 |

---

## Section 3 — Estimation Method

AI estimates at three points:
- **Every response** — lightweight running total, shown in the footer
- **Every milestone** — careful item-by-item recount
- **When Master asks** — full breakdown

Method:
1. Estimate characters of inbound + outbound since last checkpoint
2. Divide by 3.8 (mixed) or per-content ratio from Section 2
3. Add ~15,000 tokens for system overhead (unknowable to AI)
4. Report as a range with ±20% error bars

**Known bias:** AI over-estimates its own long outputs by ~50–60%. Apply downward correction.

---

## Section 4 — Footer Format (appended to every response)

**Format:**
```
— [Env: ~[X] | Msg: ~[Y] | Session: ~[Z]K / 300K ([W]%) | Files: [list]]
```

**Example:**
```
— [Env: ~9K | Msg: ~15 | Session: ~30K / 300K (10%) | Files: SESSION_HEALTH, AI_OS_REBUILD_HANDOFF]
```

**Field meanings:**

| Field | Meaning |
|---|---|
| **Env** | Full envelope read this turn — system prompt + Skill + Memory + all prior history + Master's new message |
| **Msg** | Master's new message only (typically ~10–50) |
| **Session** | Running total of envelope + output since chat start, with % of 300K |
| **Files** | Project files retrieved this turn. Write `—` if none. |

**Honest limitation:** Env and Msg are estimates, not measurements. ±20–40% error each turn. The Session total accumulates these estimates; the real number could be ±20% off. Never presented as precise.

**If a rule file is missing from the list**, the AI did not consult it. That is a signal to Master.

---

## Section 5 — External Verification

**Tool:** https://veprompts.com/tools/tokenizer/
**Model:** ChatGPT Sol Pro

- Sol reads ~0.5% higher than cl100k_base on mixed content
- cl100k_base is the industry-standard fallback for DeepSeek (no public tokenizer)
- Combined error vs true DeepSeek: ±5–10%

**Exact method (if needed):**
```python
from transformers import AutoTokenizer
tokenizer = AutoTokenizer.from_pretrained("deepseek-ai/DeepSeek-V4-Flash")
print(len(tokenizer.encode(text)))
```

---

## Section 6 — Handoff Triggers

Trigger a handoff when:
- Master explicitly says so, OR
- Estimated usage reaches **100%** of the 300K budget, OR
- Master says "tokens are tight," OR
- A major milestone closes AND Master requests a handoff, OR
- A major pivot or context shift occurs AND Master requests a handoff.

**Intermediate thresholds (35%, 50%, 65%) are suspended by Master's standing override.**

---

## Section 7 — Hallucination Prevention (Three-Strike Rule)

1. **Before saying "verified"** → actually check the source. If not checked, say "I believe" or "unverified."
2. **Before stating an assumption as fact** → flag it. "I haven't confirmed this, but..."
3. **If wrong** → correct in the same message. No burying, no deflecting.

**Case study (M0 session — 4 AI errors, all logged):**
- Claimed `cloudflare.config.ts` did not exist. Wrong — the handoff was right.
- Claimed Sol reads 1.6% higher than DeepSeek. Wrong — Sol/cl100k differ ~0.5%.
- Stated sample text was "~1000 tokens." Wrong — actual 680.
- Stated estimates as precise. Wrong — they are ranges.

**Rule enforced:** never call something verified without checking.

---

## Section 8 — Milestone Checkpoint Format

```
=== SESSION HEALTH CHECK ===
Milestone: [M# — name]
Estimated usage: ~[X]K / 300K ([W]%)
Range: [low]% — [high]%
Drift signals: [none / describe]
Recommendation: [continue / flag / handoff]
```

---

## Section 9 — Rules

- AI never fabricates a token percentage.
- AI reports estimates as ranges, never false-exact numbers.
- AI appends the footer to every response.
- AI recalibrates after each Master-side Sol check.
- **AI does not initiate a handoff at 35%, 50%, or 65% — override in effect.**
- AI writes the handoff only at 100% of the 300K cap, or when Master says to.
- AI flags drift the moment it notices it.
- AI flags every self-correction explicitly; no silent fixes.
