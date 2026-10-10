# SESSION_HEALTH.md
> Purpose: Track context usage and guard against hallucination.
> Invoked on EVERY response — not just milestones.

---

## Section 1 — Operational Budget

**Denominator: 300,000 tokens** (Master's operational cap, not the model's maximum).

The model can hold 1M. Master has chosen a conservative 300K budget for sharper reasoning and lower drift.

| Usage | Action |
|---|---|
| < 20% | Continue freely. |
| 20–35% | Normal. Mention at milestones. |
| 35–50% | Flag at next milestone. Prepare handoff draft. |
| **50%+** | **Handoff required.** Stop new work. Write handoff. |
| 65%+ | Emergency handoff. Close immediately. |

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
— [Input: ~[X] | Output: ~[Y] | Session: ~[Z]K / 300K ([W]%) | Files: [list]]
```

**Example:**
```
— [Input: ~120 | Output: ~2K | Session: ~50.5K / 300K (17%) | Files: AGENT, AI_RULES, SESSION_HEALTH]
```

**Field meanings:**

| Field | Meaning |
|---|---|
| **Input** | Estimated tokens from Master's message this turn |
| **Output** | Estimated tokens of this AI response |
| **Session** | Running total in K, with % of 300K |
| **Files** | Rule files consulted as active contracts this turn |

**Honest limitation:** Input and Output are estimates, not measurements. ±30% error each turn. The Session total accumulates these estimates; the real number could be ±20% off. Never presented as precise.

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
- Estimated usage **≥ 50%**, OR
- Master says "tokens are tight," OR
- AI detects drift (forgetting, contradiction, repetition), OR
- A major milestone closes naturally, OR
- A major pivot or context shift occurs.

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
Estimated usage: ~[X]K / 300K ([Y]%)
Range: [low]% – [high]%
Drift signals: [none / describe]
Recommendation: [continue / flag / handoff]
```

---

## Section 9 — Rules

- AI never fabricates a token percentage.
- AI reports estimates as ranges, never false-exact numbers.
- AI appends the footer to every response.
- AI recalibrates after each Master-side Sol check.
- AI initiates handoff suggestion at 35%; required at 50%.
- AI flags drift the moment it notices it.
- AI flags every self-correction explicitly; no silent fixes.