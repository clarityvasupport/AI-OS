# AI_OS_REBUILD_HANDOFF.md
> Version: v1.5
> Status: **MCP connection live end-to-end. `get_file_contents` verified against `clarityvasupport/AI-OS`. Rebuild goal met.**
> Next step: **Cleanup pass — see NEXT section.**
> Build status: Worker live, patched, and returning GitHub data to BDS.

---

## UPDATE — what changed this session (v1.4 → v1.5)

### Steps 4 and 5 complete — MCP path verified end-to-end

**Step 4 — MCP URL and credentials added to BDS.**

- Server URL: `https://github-mcp-server.clarityva-support.workers.dev/mcp`
- API Key field: **a GitHub Personal Access Token** (classic, `repo` + `read:user` scopes)

**Key discovery — BDS field semantics.** The BDS "API Key (optional)" field is sent as an `Authorization: Bearer <value>` header on every MCP request. It is **not** a static shared secret. This matters because the Worker's OAuth-provider layer rejects unknown bearer tokens by default.

**Step 5 — Test with `get_file_contents`.**

- Tool: `get_file_contents`
- `owner`: `clarityvasupport`
- `repo`: `AI-OS`
- `path`: `manifest.json`
- `branch`: `main`
- **Result: ✅ returned the real file.** MCP path proven.

### Major pivot — the fork was NOT needed

Original v1.4 plan was Option A: fork `leelakanakala/github-mcp` and add static PAT auth as a new stateless handler. Reading the source of `@cloudflare/workers-oauth-provider` (v1.2.3) revealed a callback that was already in the library since **v0.0.9**: **`resolveExternalToken`**.

What it does: when a bearer token is not found in the internal KV store, this callback fires. It lets the deployer validate a foreign credential (PAT, API key, JWT from another issuer) and return props — exactly the shape the 26 tools already read from `this.props`.

**Result:** the entire patch collapsed to **one new property** on the existing `OAuthProvider` constructor in the deployed fork.

### The patch — self-contained for the next session

**File:** `clarityvasupport/github-mcp-server` → `src/index.ts`

**Location:** bottom of the file, inside the `new OAuthProvider({...})` call, after `tokenEndpoint: "/token",`

**Content added:**

```ts
resolveExternalToken: async ({ token }) => {
	// BDS sends a GitHub PAT as the bearer. Match GitHub PAT prefixes only;
	// anything else is a stale internal token or foreign credential.
	if (!/^(ghp_|ghs_|gho_|ghu_|ghr_|github_pat_)/.test(token)) return null;

	const response = await fetch("https://api.github.com/user", {
		headers: {
			Authorization: `Bearer ${token}`,
			Accept: "application/vnd.github+json",
			"User-Agent": "github-mcp-server",
			"X-GitHub-Api-Version": "2022-11-28",
		},
	});
	if (!response.ok) return null;

	const user = (await response.json()) as {
		login: string;
		name?: string;
		email?: string;
	};

	return {
		props: {
			accessToken: token,
			login: user.login,
			name: user.name ?? user.login,
			email: user.email ?? "",
		},
	};
},
```

**No other file changed.** No new dependencies. The 26 tools are untouched. The OAuth flow is untouched.

### Verified facts (this session)

- **`resolveExternalToken` exists in `@cloudflare/workers-oauth-provider` from v0.0.9 onward.** Deployed fork pins `^0.2.3`.
- **Return shape used:** `{ props: { ... } }` — no `audience` field. This worked as-is on the 0.2.x version. (My v1.4-era flag about `audience` being required was correct only for 1.x; the deployed 0.2.x did not require it.)
- **BDS tool list:** all 26 GitHub tools discovered automatically after Save.
- **BDS "API Key" = Bearer token.** This is the crucial mental model to retain.
- **Worker URL still:** `https://github-mcp-server.clarityva-support.workers.dev`

### Session behavior notes

- **Massive context burn this session.** Three full repo imports (`leelakanakala/github-mcp`, `cloudflare/workers-oauth-provider`, `clarityvasupport/github-mcp-server`) plus the AI-OS import. Session ran **well past 300K** (approaching/exceeding 3× the cap by the end). Override was active throughout.
- **Override worked, but at real cost.** For future sessions of this size, consider: read one file at a time; only import the specific repo needed for the current question.

---

## TAG — where we are

**Complete:**

- ✅ Live layer (Skill "Master Protocol" + Memory + system prompts)
- ✅ Repo layer (structure, files, manifest)
- ✅ BDS Project "AI OS" created, instructions in, PROTOCOL.md attached
- ✅ MCP Worker deployed at `github-mcp-server.clarityva-support.workers.dev`
- ✅ MCP sub-path verified: `/mcp`
- ✅ OAuth callback URL verified: `.../callback`
- ✅ Secret names verified against `types.ts`
- ✅ Tool list verified: 26 tools
- ✅ OAuth scopes verified: `repo read:user read:org`
- ✅ Production + Development OAuth Apps created
- ✅ Cloudflare secrets set: `OAUTH_GITHUB_CLIENT_ID`, `OAUTH_GITHUB_CLIENT_SECRET`, `COOKIE_ENCRYPTION_KEY`
- ✅ Bindings confirmed: `MCP_OBJECT`, `OAUTH_KV`
- ✅ **Worker patched with `resolveExternalToken`** — supports PAT auth for BDS
- ✅ **MCP entry added to BDS with PAT in API Key field**
- ✅ **BDS connection test: all 26 tools discovered**
- ✅ **Step 5 live test: `get_file_contents` returned real `manifest.json` from AI-OS repo**

**Open — not blocking, cleanup candidates:**

- ⚠️ **`CRM_SAAS_LEGAL_DRAFT.md`** — referenced in `CONTEXT_INDEX.md` but absent from repo. Commit it or remove the reference.
- ⚠️ **`Core/SESSION_HEALTH.md` footer format is stale** — file uses `[Input: | Output: | ...]`, live Skill uses `[Env: | Msg: | ...]`. Root cause of footer-math CORRECTIONS #5. **File needs editing to match live Skill.**
- ⚠️ **`manifest.json` drift** — v2.1 manifest does not list `Projects/AI_OS_REBUILD_HANDOFF.md`. Needs updating.
- ⚠️ **Handoff override in Memory** — if Master wants thresholds restored, remove `handoff_override` entry AND update `SESSION_HEALTH.md §9` back to standard thresholds.
- ⚠️ **Dev OAuth app port 8788 unverified** — README-only claim, not confirmed against code. Only relevant if local dev becomes necessary.
- ⚠️ **`README.md` still lists stale 3-folder structure** — new folders (`Projects/AI_OS_REBUILD_HANDOFF.md`, etc.) not reflected.
- ⚠️ **`CLOUDFLARE_ACCESS.md`, `TOOLS.md` (fork repo only)** — not in AI-OS repo, no action needed.

**Known bugs / issues:**

- `.json` fetch via the + button import — still fails silently (unrelated to MCP path; pre-existing).
- **README of `leelakanakala/github-mcp` is unreliable across multiple claims** — trust the code.

---

## NEXT — immediate next action

**Cleanup pass.** Master explicitly requested this after Step 5 succeeded. Suggested order, one file at a time with Master's go between each:

### Cleanup 1 — Fix `Core/SESSION_HEALTH.md` footer spec

- **File:** `Core/SESSION_HEALTH.md` (AI-OS repo)
- **Change:** Section 4 footer format from `[Input: | Output: | ...]` → `[Env: | Msg: | ...]`
- **Also:** Section 9 — remove or update the "AI initiates handoff suggestion at 35%" rule if the override is now permanent.
- **Why:** Root cause of CORRECTIONS #5. The stale spec is still active in the repo, so any fresh session reading this file gets contradictory instructions.

### Cleanup 2 — Add handoff file to `manifest.json`

- **File:** `manifest.json` (AI-OS repo)
- **Change:** Add entry for `Projects/AI_OS_REBUILD_HANDOFF.md` under `files`, tier `project`.
- **Why:** Manifest is the file map. A missing entry means a resume session that reads the manifest won't know the handoff exists.

### Cleanup 3 — Resolve `CRM_SAAS_LEGAL_DRAFT.md` reference

- **File:** `UpdateContext/CONTEXT_INDEX.md` (AI-OS repo)
- **Change:** Either (a) commit the draft, or (b) remove the reference from the index.
- **Why:** Index refers to a file that isn't in the repo. Decays trust in the index.

### Cleanup 4 — Update `README.md` structure section

- **File:** `README.md` (AI-OS repo)
- **Change:** The `## Structure` tree still shows the old 3-folder layout. Add `Projects/`, list both handoffs, reflect the current tree.
- **Why:** Reader's first glance at the repo is wrong.

### Cleanup 5 — Decide handoff override status

- **File:** `Core/SESSION_HEALTH.md` §9 (AI-OS repo) + Memory entry `handoff_override`
- **Decision needed:** Permanent working-style change, or a one-session exception?
- **If permanent:** edit §9 to match, keep Memory entry.
- **If temporary:** remove Memory entry, restore standard thresholds, edit §9 back.

### Cleanup 6 — Optional: trim the repo

- **Not urgent.** Now that the + import works, Master can retrieve only what a session needs. Consider whether some context files (e.g., `HOW_TO_START_A_CHAT.md`) still earn their place or are fully superseded by the Skill.

---

## Open Questions for Master

1. **Handoff override** — permanent or one-session? Determines whether Cleanup 5 edits or deletes.
2. **Cleanup order** — do all 5 in one session, or one per session? The first four are small and could be batched.

---

## Version History

- v1.0 — 2026-10-10 — Initial handoff.
- v1.1 — 2026-10-10 — Fetch tag path closed. `+` import confirmed. Resume Workflow skill created.
- v1.2 — 2026-10-10 — Fetch pattern verified (`.md` ✅, `.json` ❌). Two self-corrections logged. MCP Path A deployed.
- v1.3 — 2026-10-10 — Step 1 complete. Sub-path `/mcp`, callback `/callback`, secret names, and all 26 tool names verified against source.
- v1.4 — 2026-10-10 — Steps 2 and 3 complete. OAuth apps created, Cloudflare secrets verified, bindings confirmed. Handoff override in effect; session ran to 100%.
- **v1.5 — 2026-10-10 — Steps 4 and 5 complete. MCP path live end-to-end. Major pivot: fork was unnecessary — `resolveExternalToken` callback in the OAuth-provider library solved PAT auth with a single-file patch. `get_file_contents` verified against `clarityvasupport/AI-OS`. Rebuild goal met. Cleanup pass queued.**