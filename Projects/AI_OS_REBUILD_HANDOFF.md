# AI_OS_REBUILD_HANDOFF.md
> Version: v1.4
> Status: **OAuth fully wired. Cloudflare secrets set. Ready for BDS connection and first test.**
> Next step: **Step 4 — Add MCP URL to BDS Advanced Settings.**
> Build status: Worker live and configured at `https://github-mcp-server.clarityva-support.workers.dev`

---

## UPDATE — what changed this session (v1.3 → v1.4)

### Steps 2 and 3 complete — OAuth and secrets

**Step 2 — Two GitHub OAuth Apps created (Production + Development).**

| App | Homepage URL | Callback URL | Purpose |
|---|---|---|---|
| **Production** | `https://github-mcp-server.clarityva-support.workers.dev` | `https://github-mcp-server.clarityva-support.workers.dev/callback` | Live Worker |
| **Development** | `http://localhost:8788` | `http://localhost:8788/callback` | Local dev only |

**Production Client ID (safe to record):** `Ov231iICcQWHQdDcp4G`
**Production Client Secret:** stored by Master (not in this chat).
**Development Client ID / Secret:** stored by Master (not in this chat). Unused for now.

**Step 3 — Three Cloudflare secrets verified set** (they were already present from a prior session; Master re-pasted the two OAuth values to confirm names and values, left `COOKIE_ENCRYPTION_KEY` untouched):

- ✅ `OAUTH_GITHUB_CLIENT_ID` — set
- ✅ `OAUTH_GITHUB_CLIENT_SECRET` — set
- ✅ `COOKIE_ENCRYPTION_KEY` — set (value unchanged from prior session; Master holds a backup copy of a fresh key)

**Bindings confirmed present on the Worker:**

- ✅ Durable Object: `MCP_OBJECT` → `github-mcp-server_GitHubMCP`
- ✅ KV Namespace: `OAUTH_KV` → `github-mcp-server`

### Critical finding — the source README is unreliable

Verified across this session: the README of `leelakanakala/github-mcp` disagrees with the source code on **three separate claims.**

| Claim | README says | Code says | Winner |
|---|---|---|---|
| MCP sub-path | `/sse` | `/mcp` | Code |
| Secret env var names | `GITHUB_CLIENT_ID` / `GITHUB_CLIENT_SECRET` | `OAUTH_GITHUB_CLIENT_ID` / `OAUTH_GITHUB_CLIENT_SECRET` | Code |
| Tool names/count | 26, correct names | 26, correct names | Both agree here |

**Rule going forward: trust the code, not the README.** When in doubt, fetch `src/*.ts`.

### Verified facts (from source, this session)

**MCP endpoint URL (for BDS):**
    https://github-mcp-server.clarityva-support.workers.dev/mcp

**OAuth callback URL (set in Production app):**
    https://github-mcp-server.clarityva-support.workers.dev/callback

**OAuth scopes requested** (from `github-handler.ts`):
    repo read:user read:org

**Environment variables the Worker reads** (from `types.ts`):
    OAUTH_GITHUB_CLIENT_ID      — string
    OAUTH_GITHUB_CLIENT_SECRET  — string
    COOKIE_ENCRYPTION_KEY       — string
    MCP_OBJECT                  — DurableObjectNamespace (binding)
    OAUTH_KV                    — KVNamespace (binding)

**Full OAuth route map:**

| Route | Purpose |
|---|---|
| `/mcp` | MCP protocol endpoint (BDS connects here) |
| `/authorize` | OAuth login entry point |
| `/callback` | GitHub redirect handler |
| `/token` | OAuth token exchange |
| `/register` | OAuth client registration |

**All 26 tools the deployed server exposes:**

| Category | Tools |
|---|---|
| **Files** (3) | `create_or_update_file`, `push_files`, `get_file_contents` |
| **Repos** (4) | `search_repositories`, `create_repository`, `fork_repository`, `create_branch` |
| **Issues** (5) | `create_issue`, `list_issues`, `update_issue`, `add_issue_comment`, `get_issue` |
| **PRs** (10) | `create_pull_request`, `list_pull_requests`, `get_pull_request`, `merge_pull_request`, `create_pull_request_review`, `get_pull_request_files`, `get_pull_request_status`, `update_pull_request_branch`, `get_pull_request_comments`, `get_pull_request_reviews` |
| **Search** (3) | `search_code`, `search_issues`, `search_users` |
| **Commits** (1) | `list_commits` |

### Handoff drift corrected this session

- ❌ **v1.2 banner said "Step 1 — Create two GitHub OAuth Apps."** Actual Step 1 was confirming the MCP sub-path. Banner was wrong; NEXT section was right.
- ❌ **v1.2 TAG section said "OAuth apps not created, three secrets not set."** Secrets were already set from a prior session — Cloudflare showed all three rows filled with "Value encrypted" the moment Master opened the page. Flag was stale.
- ❌ **v1.2 claimed 6 tools with names `list_repo_files`, `read_file`, `commit_files`, `list_open_pull_requests`.** None exist. Real list is 26 tools above.
- ❌ **v1.2 said README said "6 tools."** README actually says 26. Handoff misquoted its own source.
- ⚠️ **Mid-session self-correction logged:** I claimed "the README is the more authoritative source for its own repo." Wrong. The code is. Corrected in the same session.

### Session behavior notes

- Master invoked **handoff override** — thresholds suspended. Work continued past 50% / 65% / 75%. Handoff written at ~100% as instructed.
- Override is stored in Memory as `handoff_override`. If Master wants thresholds back for future sessions, this Memory entry needs removal.

---

## TAG — where we are

**Complete:**

- ✅ Live layer (Skill "Master Protocol" + system prompts + Memory)
- ✅ Repo layer (structure, files, manifest)
- ✅ BDS Project "AI OS" created, instructions in, PROTOCOL.md attached
- ✅ Fetch pattern verified for `.md`; `.json` gap documented
- ✅ MCP Path A chosen
- ✅ MCP Worker deployed — `https://github-mcp-server.clarityva-support.workers.dev`
- ✅ `account_id` fixed in `wrangler.jsonc`
- ✅ **MCP sub-path verified: `/mcp`**
- ✅ **OAuth callback URL verified: `.../callback`**
- ✅ **Secret names verified against `types.ts`**
- ✅ **Tool list verified: 26 tools**
- ✅ **OAuth scopes verified: `repo read:user read:org`**
- ✅ **Production + Development OAuth Apps created**
- ✅ **Cloudflare secrets set and verified**
- ✅ **Bindings confirmed: `MCP_OBJECT`, `OAUTH_KV`**

**Open — not blocking:**

- ⚠️ **MCP URL not yet added to BDS Advanced Settings** — URL known exactly: `https://github-mcp-server.clarityva-support.workers.dev/mcp`
- ⚠️ **No connection test yet** — Step 5 pending
- ⚠️ **`CRM_SAAS_LEGAL_DRAFT.md`** — referenced in `CONTEXT_INDEX.md` but absent from repo
- ⚠️ **`Core/SESSION_HEALTH.md` footer format is stale** — file uses `[Input: | Output: | ...]`, live Skill uses `[Env: | Msg: | ...]`. This is the root cause of the footer-math bug already logged as CORRECTIONS #5.
- ⚠️ **`manifest.json` drift** — v2.1 manifest does not list `Projects/AI_OS_REBUILD_HANDOFF.md`
- ⚠️ **Dev OAuth app port 8788 is unverified** — README-only claim, not confirmed against code. Only relevant if local dev becomes necessary.
- ⚠️ **Handoff override in Memory** — if Master wants thresholds back, remove `handoff_override` entry.

**Known bugs / issues:**

- `.json` fetch fails silently (2 attempts, v1.2 session)
- README of `leelakanakala/github-mcp` is stale across multiple claims — trust the code

---

## NEXT — immediate next action

**Step 4 — Add MCP URL to BDS Advanced Settings.**

1. Open the BDS settings / Advanced Settings area where MCP servers are configured.
2. Add a new MCP server entry.
3. **Server URL:** `https://github-mcp-server.clarityva-support.workers.dev/mcp`
4. Save.
5. BDS will likely open a browser window on first connection attempt to complete GitHub OAuth (this is the expected flow — the Worker will redirect to `/authorize`, then to GitHub, then back to `/callback`).
6. Authorize the OAuth app for the `clarityvasupport` GitHub account.

**Step 5 — Test with `get_file_contents`.**

Once connected, run this tool call to prove the MCP path works end-to-end:

- Tool: `get_file_contents`
- `owner`: `clarityvasupport`
- `repo`: `AI-OS`
- `path`: `manifest.json`
- `branch`: `main`

**Success looks like:** the contents of `manifest.json` returned as JSON text.

**Failure modes to watch for:**

| Symptom | Likely cause |
|---|---|
| BDS can't reach the URL | Wrong path — must end in `/mcp`, not `/sse` |
| Redirect to GitHub succeeds, then error | Callback URL mismatch — check Production app on GitHub |
| "bad_verification_code" from GitHub | Dev credentials pasted into Cloudflare instead of Production |
| Tool call returns "Not logged in" | OAuth flow not completed; retry in browser |

**Then return to the outstanding cleanup list:**

- Fix `Core/SESSION_HEALTH.md` — replace `[Input: | Output: | ...]` with `[Env: | Msg: | ...]` to match live Skill
- Add `Projects/AI_OS_REBUILD_HANDOFF.md` to `manifest.json`
- Resolve `CRM_SAAS_LEGAL_DRAFT.md` flag — commit it or remove the reference
- Decide whether to keep handoff override in Memory or restore thresholds

---

## Open Questions for Master

1. **Repo trim** — worth reducing `+` import cost now that fetch works for `.md`? (Not urgent.)
2. **Handoff override** — permanent change to working style, or one-session exception? If permanent, SESSION_HEALTH.md §9 should be edited to match.

---

## Version History

- v1.0 — 2026-10-10 — Initial handoff.
- v1.1 — 2026-10-10 — Fetch tag path closed. `+` import confirmed. Resume Workflow skill created.
- v1.2 — 2026-10-10 — Fetch pattern verified (`.md` ✅, `.json` ❌). Two self-corrections logged. MCP Path A deployed.
- v1.3 — 2026-10-10 — Step 1 complete. Sub-path `/mcp`, callback `/callback`, secret names, and all 26 tool names verified against source. README of `leelakanakala/github-mcp` confirmed unreliable — code is truth.
- **v1.4 — 2026-10-10 — Steps 2 and 3 complete. Two OAuth Apps created (Production + Development). Cloudflare secrets verified set. Bindings confirmed. Handoff drift in v1.2 flagged (stale "secrets not set", wrong tool list, wrong Step 1 banner). Handoff override in effect; session ran to 100%.**