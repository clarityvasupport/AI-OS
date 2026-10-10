# SKILLS.md
> Purpose: The skills the AI should bring to this project.
> Not a checklist. A standard.

## Technical Skills
- **Next.js (App Router):** server components, route handlers, server actions, middleware.
- **TypeScript:** strict mode, clear types, no `any` unless justified.
- **Tailwind CSS v4:** CSS-first config (no `tailwind.config.ts`). PostCSS plugin via `@tailwindcss/postcss`.
- **shadcn/ui:** mobile-first, accessible, clean.
- **Cloudflare stack:**
  - **vinext** — Vite-based Next.js adapter for Workers. Auto-registers `@vitejs/plugin-rsc` when `app/` exists.
  - **`cf` CLI (beta)** — new unified Cloudflare CLI. Replaces Wrangler on vinext path. Config lives in `cloudflare.config.ts`.
  - **`@cloudflare/vite-plugin@beta`** — 2.0 beta, Wrangler-free. Required for `cf` path.
  - **workerd** — Workers runtime. Requires **glibc 2.35+** (Debian 12 / Ubuntu 22.04+).
  - **Compatibility date** — must be ≤ newest date supported by local `workerd` binary. Align, don't update `workerd`.
  - **D1, KV, R2, Cron Triggers** — bindings added in `cloudflare.config.ts` under `env`.
- **Auth:** Google OAuth, email/password, session management in KV, hashing best practices.
- **Multi-tenancy:** tenant_id isolation at every layer. DB, API, frontend, logs, tests.
- **Data modeling:** SQL schema design, JSONB for flexible fields, migrations.
- **Security:** least privilege, input validation, no secrets in client bundles.
- **Compliance:** GDPR, CCPA/CPRA, consent logging, data export, right to erasure.

## Communication Skills
- Translate technical to plain language instantly.
- Visualize with tables, diagrams, and side-by-side comparisons.
- Give the "why" before the "how."
- Break complex steps into discrete, confirmable chunks.
- Summarize long answers up front, details below.
- Know when to stop and ask.

## Domain Knowledge
- SaaS CRM structure: contacts, leads, deals, jobs, activities, tasks, forms.
- Service businesses: cleaning, renovation, design, general service.
- Recurring jobs: recurrence rules, confirmations, skips, reschedules.
- No-code configuration: custom fields, editable stages, template systems.
- Form builders: field types, submission routing, CRM linking.
- Freemium per-seat billing: free tier, trials, add-ons, upgrades.

## Known vinext / Cloudflare Gotchas
- **Don't add `rsc()` manually** to `vite.config.ts` — vinext auto-registers it. Duplicate = build failure.
- **Port 5173**, not 3000.
- **No `wrangler.jsonc`** on the `cf` path — the file is `cloudflare.config.ts`.
- **Compatibility date must match local `workerd`**, or the dev server refuses to start.
- **`nodejs_compat`** is redundant for compat dates ≥ 2026-08-04 (harmless to specify, but may warn).
- **`cf workers types`** must be run before standalone TypeScript checks (types generate during dev/build into `.cloudflare/types`).

## Anti-Skills (Avoid)
- Over-engineering. Prefer the simple MVP path.
- Assumption-driven answers. Verify or ask.
- Jargon-heavy explanations without translation.
- Dumping multiple steps at once.
- Recommending tools the AI has not verified.
- Making decisions on Master's behalf.
- Rushing past confirmation gates.
- **Stating assumptions as verified facts.** If not verified, say "I believe" or "I need to check."

## Quality Bar
- Every file delivered complete, ready to paste.
- Every command tested mentally or verified from docs.
- Every recommendation paired with a trade-off.
- Every risk flagged in the same message it is discovered.
- Every handoff clean, current, and pasteable.