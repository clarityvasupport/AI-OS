# AI_OS_REBUILD_HANDOFF.md
> Version: v1.7
> Status: **Cleanup pass 3 of 5 done. Session cap raised to 500K (temporary).**
> Next step: **Finish Cleanups 3 and 4.**
> Build status: Worker live. Manifest drift fixed. Footer spec corrected. CODE_RUNNER auto-return confirmed.

---

## UPDATE — what changed this session (v1.6 → v1.7)

**Cap raised: 300K → 500K (temporary).**
- SESSION_HEALTH Section 1 denominator updated to 500,000. Footer examples now show /500K.
- Section 6 and Section 9 reference the 500K cap.
- Marked temporary — may change again.

**CODE_RUNNER auto-return confirmed.**
- Output from BDS:AUTO:CODE_RUNNER flows back to the AI without copy-paste. Automation works.
- **Caveat: CODE_RUNNER strips backslashes before execution.** A regex with escaped characters failed at runtime.
- Rule: build base64url with split().join() and a while loop. Build newlines with String.fromCharCode(10). No regex, no escapes.
- Logged as CORRECTIONS #7.

**Prior session (v1.5 → v1.6), still current:**
- manifest.md mirror created; manifest.json updated to v2.2 (drift fixed). Commits 17778a5d, 150746c0.
- SESSION_HEALTH footer + thresholds corrected. Commit 43b85b41.
- CORRECTIONS #6 logged. Commit c5e6df05.
- Handoff v1.6 committed. Commit e6cf9438.

### Operational learnings

- **push_files batches multiple files in one commit.** Array of {path, content}.
- **MCP writes can fail silently.** Verify via directory listing before claiming success.
- **Directory listings work** even when the file itself is .json.
- **CODE_RUNNER strips backslashes.** Write escape-free code.
- **Direct .json fetch fails** — use the raw-content route.

---

## TAG — where we are

**Complete:**
- ✅ Live layer (Skill + Memory + system prompts)
- ✅ Repo layer (structure, files, manifest)
- ✅ MCP Worker live end-to-end; 26 tools discovered
- ✅ manifest drift fixed; manifest.md mirror created
- ✅ SESSION_HEALTH footer + threshold fixes applied
- ✅ Session cap raised to 500K (temporary)
- ✅ CORRECTIONS #6 and #7 logged
- ✅ CODE_RUNNER auto-return confirmed

**Open — the remaining cleanup pass:**
- ⚠️ **Cleanup 3** — CRM_SAAS_LEGAL_DRAFT.md referenced in UpdateContext/CONTEXT_INDEX.md but absent from repo. Either commit the draft or remove the reference.
- ⚠️ **Cleanup 4** — README.md structure section still shows the old 3-folder layout.

**Known bugs / issues:**
- Direct .json fetch fails silently — use the raw-content route.
- MCP writes occasionally drop silently — verify after every write.
- CODE_RUNNER strips backslashes — write escape-free code.
- The + button import still fails on .json files.

---

## NEXT — immediate next action

**Finish the cleanup pass.** Two files remain, batched via one push_files call:

### Cleanup 3 — Resolve the CRM_SAAS_LEGAL_DRAFT.md reference
- **File:** UpdateContext/CONTEXT_INDEX.md
- **Change:** commit the draft, or remove the reference.

### Cleanup 4 — Update the README.md structure section
- **File:** README.md
- **Change:** add Projects/, list both handoffs, reflect the current tree.

### Order of operations
1. Read CONTEXT_INDEX.md and README.md (both .md — no fetch issues).
2. Prepare both edits.
3. Batch via ONE push_files call.
4. Verify via root directory listing.

---

## Open Questions for Master

1. **Cleanup 3 direction** — commit the LEGAL_DRAFT, or remove the reference?
2. **Cap 500K** — temporary. Revisit when?

---

## Version History

- v1.0 — 2026-10-10 — Initial handoff.
- v1.1 — Fetch tag path closed. Import confirmed. Resume Workflow skill created.
- v1.2 — Fetch pattern verified. MCP Path A deployed.
- v1.3 — Step 1 complete. Sub-path, callback, secrets, 26 tool names verified.
- v1.4 — Steps 2 and 3 complete. OAuth apps, secrets, bindings confirmed.
- v1.5 — Steps 4 and 5 complete. MCP path live. resolveExternalToken patch.
- v1.6 — Cleanup 3 of 5 done. Manifest drift fixed. Footer corrected. Override permanent.
- **v1.7 — 2026-10-10 — Cap raised to 500K (temporary). CODE_RUNNER auto-return confirmed; backslash caveat logged as #7. Remaining: Cleanups 3 and 4.**
