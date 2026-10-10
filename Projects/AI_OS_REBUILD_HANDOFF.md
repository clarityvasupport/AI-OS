# AI_OS_REBUILD_HANDOFF.md
> Version: v1.2
> Status: MCP Worker **deployed and verified**. Awaiting OAuth + secrets + BDS connection.
> Next step: **Step 1 — Create two GitHub OAuth Apps, then set three Worker secrets via Cloudflare Dashboard.**
> Build status: Worker live at `https://github-mcp-server.clarityva-support.workers.dev`.

---

## UPDATE — what changed this session

### Fetch pattern — verified with caveats

| Test | File | Result |
|---|---|---|
| `AUTO:REQUEST_WEB_FETCH` on raw GitHub `.md` | GLOSSARY.md, README.md, SESSION_HEALTH.md, CRM_SAAS_HANDOFF.md | ✅ All succeeded — full content, ~14K char file handled cleanly |
| `AUTO:REQUEST_WEB_FETCH` on raw GitHub `.json` | manifest.json (×2 attempts) | ❌ Silent failure — no file, no card, no error |

**Caveats on fetched `.md`:**
- Markdown escaping applied (`\#`, `\*\*`, `\_`) — prose readable, not paste-executable.
- Code blocks collapse to one line — the Python snippet in SESSION_HEALTH.md arrived as a single line.
- **Fetch is a reference tool, not a code-delivery tool.**

### Two self-corrections logged this session

1. **"Auto-injection works ✅"** — overstated. Master clarified Project files are selected via manual checkbox, not auto-injected. Corrected.
2. **"Fork + Deploy button is web-UI-only"** for `clemdevlin/github-pr-mcp` — described before verification. On verification, its README shows CLI setup. Corrected.

### MCP path — decision and outcome

**Goal:** give the AI on-demand read/write/move access to the repo — like a local filesystem mirrored to GitHub — without CLI.

**Paths evaluated:**

| Path | Repo | Deploy button | Auth | Verdict |
|---|---|---|---|---|
| A | `leelakanakala/github-mcp` | ✅ Yes | OAuth (2 apps + KV + 3 secrets) | **Chosen — deployed** |
| B | `clemdevlin/github-pr-mcp` | ❌ No | PAT (simple) | ❌ CLI-blocked |
| C | Custom build on Cloudflare template | ✅ Yes | TBD | Fallback — unused |

**Path A — deployed and verified:**

- ✅ Deploy button worked — cloned to `clarityvasupport/github-mcp-server`
- ❌ First build failed: `account_id` placeholder in `wrangler.jsonc`
- ✅ Fix applied: `account_id` set to `462fe9ef1a7169a830c9d9958e37ea06`
- ✅ Second build succeeded — `✨ Success! Build completed.`
- ✅ **Worker URL:** `https://github-mcp-server.clarityva-support.workers.dev`
- ✅ **Version ID:** `e89410db-ecb4-490d-864e-830d69d5dd3f`
- ✅ **Bindings attached:** `MCP_OBJECT` (Durable Object), `OAUTH_KV` (KV Namespace)

**Tools the deployed server exposes** (per source README):
`list_repo_files`, `read_file`, `create_branch`, `commit_files`, `create_pull_request`, `list_open_pull_requests`.

### BDS environment findings (new)

- **BDS v0.1.11+ supports MCP servers** — HTTP/Streamable HTTP only, no stdio. Configured in Advanced Settings. This removes the previously assumed blocker; BDS can act as an MCP client.

### Footer-math discrepancy — cause identified

- The **live Skill** uses `[Env: | Msg: | Session: | Files:]`
- `Core/SESSION_HEALTH.md` **on disk still shows** `[Input: | Output: | Session: | Files:]`
- **The file is stale — not mathematically wrong.** Fix is a file edit, not a math debate.

---

## TAG — where we are

**Complete:**
- ✅ Live layer (Skill "Master Protocol" + system prompts + Memory)
- ✅ Repo layer (structure, files, manifest)
- ✅ BDS Project "AI OS" created, instructions in, PROTOCOL.md attached
- ✅ Fetch pattern verified for `.md`; `.json` gap documented
- ✅ MCP Path A chosen
- ✅ **MCP Worker deployed** — `https://github-mcp-server.clarityva-support.workers.dev`
- ✅ `account_id` fixed in `wrangler.jsonc`

**Open — not blocking:**
- ⚠️ **OAuth apps not created** — two required (Production + Development)
- ⚠️ **Three secrets not set** — `OAUTH_GITHUB_CLIENT_ID`, `OAUTH_GITHUB_CLIENT_SECRET`, `COOKIE_ENCRYPTION_KEY`
- ⚠️ **MCP sub-path unverified** — Worker base URL is known, but the MCP protocol endpoint path (likely `/mcp` or `/sse`) is not confirmed. Next session's first step: fetch the README to confirm.
- ⚠️ **MCP URL not yet added to BDS Advanced Settings**
- ⚠️ **`CRM_SAAS_LEGAL_DRAFT.md`** — referenced in CONTEXT_INDEX.md but absent from repo
- ⚠️ **Footer math discrepancy** — cause identified (stale SESSION_HEALTH.md). Fix pending.
- ⚠️ **Repo trim** — each `+` import costs ~25–30K tokens

**Known bugs encountered this session:**
- `.json` fetch fails silently (2 attempts)
- Earlier Path B framing was wrong before verification
- Earlier "auto-injection works" claim was overstated

---

## NEXT — immediate next action

**Step 1 — Confirm the MCP sub-path.**
Fetch the `leelakanakala/github-mcp` README and locate the exact path the MCP protocol is exposed on (likely `/mcp` or `/sse`). Confirm before proceeding — the OAuth callback URLs depend on it.

**Step 2 — Create two GitHub OAuth Apps.**
github.com/settings/developers → New OAuth App. Create Production + Development. Callback URLs from the repo README.

**Step 3 — Set three secrets via Cloudflare Dashboard.**
Worker `github-mcp-server` → Settings → Variables & Secrets → Add:
- `OAUTH_GITHUB_CLIENT_ID`
- `OAUTH_GITHUB_CLIENT_SECRET`
- `COOKIE_ENCRYPTION_KEY` — any 32+ character random string

**Step 4 — Add MCP URL to BDS Advanced Settings.**
Full URL: `https://github-mcp-server.clarityva-support.workers.dev` + the confirmed sub-path from Step 1.

**Step 5 — Test with `list_repo_files` on `clarityvasupport/AI-OS`.**

**Then return to the original NEXT list:**
- Resolve the `CRM_SAAS_LEGAL_DRAFT.md` flag
- Fix `SESSION_HEALTH.md` — replace old footer format with live Env/Msg format

---

## Open Questions for Master

1. **OAuth complexity** — with Path A deployed, is the OAuth setup still acceptable, or do we pivot later to a lighter PAT-based server? (Not urgent — deploy succeeded.)
2. **Repo trim** — worth reducing `+` import cost (~25–30K per import) now that fetch works for `.md`?

---

## Version History
- v1.0 — 2026-10-10 — Initial handoff.
- v1.1 — 2026-10-10 — Fetch tag path closed. `+` import confirmed. Resume Workflow skill created.
- **v1.2 — 2026-10-10 — Fetch pattern verified (`.md` ✅, `.json` ❌). Two self-corrections logged. MCP Path A deployed and verified at `https://github-mcp-server.clarityva-support.workers.dev`. Footer-math cause identified: stale SESSION_HEALTH.md, not a math error. Session ended at ~72% (past 70% override).**