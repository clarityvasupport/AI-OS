# AI_OS_REBUILD_HANDOFF.md
> Version: v1.1
> Status: Live layer complete. Repo layer complete. Repo import path resolved.
> Next step: **Decide whether the BDS Project is still needed, now that the `+` button import is confirmed working.**
> Build status: Extension configured. Repo rebuilt. Resume Workflow skill active. Project not yet created.

---

## UPDATE — what changed this session

### The fetch-tag question is settled
Prior handoff assumed the GitHub fetch tag (`BDS:AUTO:REQUEST_GITHUB_FETCH`) was the primary path and that the token might be the blocker. **Both assumptions were wrong.**

- The tag **fails in this environment** — three attempts, all returned empty, including one after a valid token was saved.
- The token was **never the problem**. Configuration was correct: classic PAT, `repo` scope, saved in Advanced Settings.
- DeepResearch confirmed the tag is a documented core feature — so the failure is environmental, not architectural.

### The working path is confirmed
Master imported the full repo successfully via the **`+` button → Import a GitHub repository**. This path:
- Does **not** depend on the tag parser.
- Works today, no setup required beyond the token already saved.
- Brings in the entire repo (~25–30K tokens) in one action.

### Resume Workflow skill created
- Skill **"Resume Workflow"** — fingerprint `2047`. Active.
- Fires on "resume" / "continue the project" / naming a handoff.
- If the import is absent, it outputs the exact `+` button click path. If present, it runs the read sequence.
- Encodes the rule: **never retry the fetch tag; use the `+` button.**

### Memory corrected
- `current_project` (always) — **updated** to *"AI OS Rebuild. Repository clarityvasupport AI-OS on GitHub, main branch."*
- `repo_import_method` (called) — **new**. Records the `+` button path and that the fetch tag fails.

### Version awareness
- Repo is on **v0.1.14**. Latest is **v0.1.15** (2026-10-09 — one day old).
- v0.1.15 adds Multi-Forge import (GitHub, GitLab, HuggingFace, Codeberg) and bug fixes.
- **Unverified** whether v0.1.15 fixes the fetch tag. Update is plausible, not confirmed.

### New discrepancy logged
- **Footer math disagrees with itself.** Master Protocol footer uses the *envelope* method (~113K this session). `SESSION_HEALTH.md` uses a *file-based* method (~42K). Gap is ~2.5×. Unreconciled. Flagged, not fixed.

### Search provider note
- DuckDuckGo and Bing both returned anti-bot challenges or empty results during DeepResearch. **Direct fetches to known URLs work.** Prefer fetch over search when the URL is known.

---

## TAG — where we are

**Complete:**
- ✅ Live layer (Skill "Master Protocol" + system prompts + Memory)
- ✅ Repo layer (structure, files, manifest)
- ✅ Corrections logged (5 entries)
- ✅ Footer format corrected
- ✅ Resume Workflow skill created and active
- ✅ Repo import path resolved and confirmed

**Open — not blocking:**
- ⚠️ **`CRM_SAAS_LEGAL_DRAFT.md`** — referenced in `CONTEXT_INDEX.md` as "not yet committed," but absent from the repo. Decision needed: commit it, or remove the reference.
- ⚠️ **Project token test not run** — whether Project files auto-inject is still unverified. May now be moot, since the import path works.
- ⚠️ **v0.1.15 update decision** — pending. Not urgent.
- ⚠️ **Footer math discrepancy** — envelope vs file-based methods disagree by ~2.5×. Needs a decision on which method is canonical.
- ⚠️ **Repo token cost** — each import costs ~25–30K tokens. May warrant trimming the repo before every resume.

**Known bugs encountered:**
- BDS memory parser garbles values containing URL syntax. Workaround: write memory as plain prose.
- `AUTO:REQUEST_GITHUB_FETCH` fails in this environment. `AUTO:REQUEST_WEB_FETCH` on raw GitHub URLs works. `+` button import is the primary path.
- Re-creating a skill with the same name creates a duplicate, not an update. Delete the old one manually.

---

## NEXT — immediate next action

**Step 1 — Decide the Project question.**
The old plan was "create a lean BDS Project, then run the token test." That plan assumed the fetch tag was primary. Now that the `+` import works, the Project may be **redundant** — it could duplicate context and burn tokens.

Decision required:
- **If the Project is still wanted** — create it lean (Instructions + `PROTOCOL.md` only), then run the token test.
- **If the Project is dropped** — rely on the `+` import alone. Simpler, cheaper, already proven.

**Step 2 — Resolve the legal draft flag.**
Commit `CRM_SAAS_LEGAL_DRAFT.md`, or remove its reference from `CONTEXT_INDEX.md`.

**Step 3 — Reconcile the footer math.**
Decide which method is canonical — envelope or file-based — and align both `SESSION_HEALTH.md` and the Master Protocol footer to match.

---

## Open Questions for Master
1. **Project** — create it, or drop it in favor of the `+` import?
2. **Legal draft** — commit it, or remove the reference?
3. **Footer math** — envelope method, file-based method, or a reconciled third?
4. **Repo trim** — worth reducing token cost per import, or accept it?

---

## Version History
- v1.0 — 2026-10-10 — Initial handoff. Documented the extension-driven rebuild of the AI OS.
- **v1.1 — 2026-10-10 — Fetch-tag path closed (confirmed failure). `+` button import confirmed as the working path. Resume Workflow skill created. Memory corrected. Footer math discrepancy logged. Project question reopened.**