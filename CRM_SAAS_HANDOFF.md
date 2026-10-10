# CRM_SAAS_HANDOFF.md
> Version: v0.6
> Status: Skeleton built, running locally, committed. Ready for first deploy.
> **Major update:** v0.5 corrections — `cloudflare.config.ts` confirmed correct, `cf` CLI replaces Wrangler, devcontainer fixed to Bookworm.
> Next step: **Step 5 — first deploy via Cloudflare Dashboard.**
> Build status: Working locally. Dev server runs on port 5173. No deploy yet.

---

## 1. Product Vision
(Unchanged from v0.5)

Build a mobile-first, multi-tenant, no-code SaaS CRM for service businesses.

- Serves any service business, especially startups.
- Supports cleaning, renovation, design, and other service fields.
- Pre-built templates by business type, editable by tenant.
- One-time and recurring jobs.
- Built-in customizable form builder.
- Optional Google Sheets link.
- Google Workspace integrations first.
- Mobile-first, desktop second.
- Strict tenant isolation by `tenant_id`.
- Hundreds of tenants, ~5 users per tenant common case.

---

## 2. Tenant Model and Isolation
(Unchanged)
- Multi-tenant SaaS, shared DB, `tenant_id` on every record.
- Enforced at DB, API, backend, frontend, logs, tests.
- Later: separate DBs for larger clients.
- Tenants only see their own records.

---

## 3. Business Types and Templates
(Unchanged)
- Cleaning, Renovation, Design, **Other** (user fills in text field).
- Templates editable: rename stages, add/remove fields, reorder.

---

## 4. Core Records
(As in Data Model v1.0 — 20 entities)
- Tenant, User, Contact, Lead, Pipeline, Stage, Deal, Job, Recurring Plan, Activity, Task, Form, Form Submission, Quote, Custom Field, Tag, Automation, Integration, Audit Log, Consent Record.
- Every table includes `tenant_id` (except `Tenant`).

---

## 5. Recurring Jobs
(Unchanged)
- Weekly, bi-weekly, monthly, custom.
- Reminder before next occurrence.
- Confirmation, skip single occurrence, reschedule single occurrence.
- Next occurrence generated automatically.
- Recurring plan separate from individual jobs.

---

## 6. Form Builder and Integrations
(Unchanged)
- Built-in form builder, linkable to CRM.
- Submissions become leads or contacts.
- Optional Google Sheets link.
- Google Workspace first. Later Gmail, Calendar, Drive, Stripe.
- Integrations may be premium by plan.

---

## 7. Plans, Billing, Trials
(Unchanged)
- Free: 5 users, 5 custom fields, 100 automations/mo, Google Sheets export.
- Premium: 20 users, 50 custom fields, 5,000 automations/mo, Google Workspace + Stripe.
- Business: 50+ users, unlimited custom fields, 50,000 automations/mo, all integrations, priority support.
- Trial: $1/mo for 3 months.
- Add-on user: $5/user/mo.
- Premium: $29/user/mo. Business: $49/user/mo. Annual: 2 months free.
- **MVP: only Free plan active.** Billing integration later.

---

## 8. User Roles
(Unchanged)
- MVP: Owner, Admin, Staff.
- Owner: full + billing.
- Admin: full except billing.
- Staff: assigned records only.
- Manager, Read-only, custom roles: later.

---

## 9. MVP Feature List
(Unchanged)
1. Tenant signup and login
2. Business type selection with pre-built template
3. Contacts and Leads
4. Custom fields on Contacts and Leads
5. One pipeline with editable stages
6. Jobs, one-time and recurring
7. Recurrence rule with confirmation and skip
8. Activities: calls, emails, notes
9. Tasks and reminders
10. Built-in form builder
11. Form to CRM link
12. Form to Google Sheets optional link
13. Mobile-first UI
14. Roles: Admin and Staff minimum
15. Free plan: 5 users, 5 custom fields, 100 automations/mo
16. Per-seat add-on for 6th user
17. Privacy: data export, data deletion, consent checkbox
18. Terms and Privacy Policy agreement on signup

**Not in MVP:** Stripe for tenants' customers, advanced automations, multiple pipelines, deep reporting, separate DBs, SMS, file uploads, tenant API.

---

## 10. Privacy and Compliance
(Unchanged)
- GDPR and CCPA/CPRA built in.
- Consent checkbox at signup, unchecked by default.
- Log user ID, tenant ID, timestamp, IP, version accepted.
- Data export and deletion.
- Tenant is controller, platform is processor.
- DPA and subprocessor list available.
- Attorney review required.
- Legal draft: CRM_SAAS_LEGAL_DRAFT.md (Master-approved for attorney review).

---

## 11. Tech Stack — CORRECTED (v0.6)

| Layer | Decision |
|---|---|
| **Frontend framework** | Next.js (App Router) with TypeScript |
| **Styling** | Tailwind CSS v4 (CSS-first config — **no `tailwind.config.ts`**) |
| **UI components** | shadcn/ui (to be added later) |
| **Deploy target** | **Cloudflare Workers** |
| **Next.js adapter** | **vinext** (Vite-based, Cloudflare's official recommendation) |
| **Dev runtime** | Node 22 (locked in Codespaces via devcontainer) |
| **Dev OS** | Debian 12 Bookworm (glibc 2.36 — required by `workerd`) |
| **Dev server port** | **5173** (vinext/Vite default, not 3000) |
| **Runtime** | Cloudflare Workers runtime (workerd, V8-based) |
| **Node compat flag** | `nodejs_compat` (on by default for compat dates ≥ 2026-08-04) |
| **Compatibility date** | **`2026-10-07`** (must match local `workerd` binary's newest supported date) |
| **Cloudflare config** | **`cloudflare.config.ts`** (typed, imported from `cf/config`) |
| **Deploy tool** | **`cf` CLI (beta)** — the new unified Cloudflare CLI, replaces Wrangler on this path |
| **Wrangler** | **Not used on this path.** `@cloudflare/vite-plugin@beta` does not depend on Wrangler. |
| **Database** | Cloudflare D1 |
| **Sessions / cache** | Cloudflare KV |
| **File storage (future)** | Cloudflare R2 |
| **Auth** | Google OAuth + email/password via Workers + KV sessions |
| **Cron** | Cloudflare Workers Cron Triggers |
| **Repo** | GitHub (`crm-saas`, private) |
| **Dev environment** | GitHub Codespaces |
| **Cloudflare management** | **Dashboard only** (no CLI, no Wrangler for Master) |
| **Billing** | Stripe (later) |
| **Export** | CSV, PDF (client-side), Google Sheets |

### Corrections from v0.5
- ❌ `cloudflare.config.ts` "does not exist" — **WRONG.** It **does** exist and is the correct config file. The v0.5 handoff was right. This was an AI error in the prior session.
- ❌ `wrangler.jsonc` — **not used** on the `cf` path. It exists on the legacy Wrangler path only.
- ❌ `tailwind.config.ts` — **not generated.** Tailwind v4 uses CSS-first config.
- ❌ `compatibilityDate: "2026-10-06"` — corrected to **`2026-10-07`**.
- ❌ `cloudflare.config.ts` — confirmed **present and correct**.
- ❌ Scripts named `dev:vinext` / `build:vinext` — actual scripts are **`dev` / `build` / `start` / `deploy`**.
- ❌ Port 3000 — actual dev server port is **5173**.

**Why the pivot still stands:** Cloudflare recommends vinext on Workers for full-stack Next.js apps. The Pages + `next-on-pages` path is legacy. Known vinext gaps (Cache Components, `use cache`, build-time image optimization, native modules like `sharp`) do not affect us.

---

## 12. Navigation Map
(Unchanged)
- Mobile: TikTok-style vertical feed. Bottom nav: Home, Contacts, Jobs, Forms, Profile.
- Desktop: Left sidebar.
- Key screens: Landing (deferred), Signup/Login, Onboarding, Dashboard Feed, Contact Detail, Job Detail, Form Builder, Settings.

---

## 13. Data Model
(As in Data Model v1.0 — 20 entities listed in section 4)

---

## 14. Delivery Plan — CORRECTED

**Constraint:** GitHub web UI + Codespaces for dev. Cloudflare Dashboard for all cloud resources. No CLI, no Wrangler for Master (deploy commands run on Cloudflare's servers via Workers Builds).

**Actual repo structure (as built):**

**Key files:**
- `cloudflare.config.ts` — Worker name, entrypoint (`vinext/server/fetch-handler`), compatibility date, bindings (currently only `ASSETS`).
- `vite.config.ts` — plugins `[vinext(), cloudflare({ viteEnvironment: { name: "rsc", childEnvironments: ["ssr"] } })]`.

**Milestones:**
| # | Milestone | Status |
|---|---|---|
| M0 | Repo + skeleton | ✅ **Done** — scaffolded, running locally, committed |
| M1 | First deploy (Workers) | **Next** — via Cloudflare Dashboard |
| M2 | D1 + KV provisioned | Pending |
| M3 | Auth — email/password | Pending |
| M4 | Auth — Google OAuth | Pending |
| M5 | Tenant onboarding | Pending |
| M6 | Home feed shell | Pending |
| M7 | Contacts + Leads CRUD | Pending |
| M8 | Jobs + Recurring | Pending |
| M9 | Forms + Google Sheets | Pending |
| M10 | Export + Deletion | Pending |
| M11 | Landing page | Pending |

---

## 15. Current State (as of v0.6)

**Done:**
- Step 1: GitHub repo `crm-saas` created, private.
- Step 2: Codespace opened.
- Step 3: `.devcontainer/devcontainer.json` and `.node-version` committed. Node 22.
- Step 4: Skeleton created via `npm create vinext-app@latest crm-saas` in `/tmp`, then copied into repo root.
- `npm install` run successfully.
- `.gitignore` merged (added `next-env.d.ts`, `.vinext/`, `.cloudflare/`).
- Dev server verified working: page renders, API route returns JSON.
- Skeleton committed to `main`.

**Issues resolved this session:**
- `write EPIPE` on dev server start → caused by Debian 11 glibc 2.31. **Fixed by upgrading devcontainer to Debian 12 Bookworm** (glibc 2.36).
- Compatibility date mismatch → `workerd` binary supports up to `2026-10-07`. **Fixed by editing `cloudflare.config.ts`.**
- Git LFS pre-push hook blocked push → **Fixed by removing `.git/hooks/pre-push`.** We do not use LFS.

**Outstanding flags (not blocking):**
- **15 npm vulnerabilities** (4 moderate, 11 high). **Do not run `npm audit fix --force`** — could break the beta toolchain. Investigate before first deploy.
- `@cloudflare/vite-plugin@beta` — expected for the `cf` path. Monitor for breaking changes.
- `cf` CLI is **beta** — commands and config may evolve.

**Decided:**
- Package manager: **npm**.
- Node version: **22**.
- Deploy target: **Cloudflare Workers + vinext** (not Pages).
- Deploy mechanism: **Cloudflare Workers Builds** connected to GitHub repo, configured via Dashboard only.
- README: **kept original** (Option A) — will rewrite when app does something.

**Next:**
- Step 5: First deploy via **Cloudflare Dashboard → Workers & Pages → Create → Import a repository**.

---

## 16. Open Questions for Master
**None.** All resolved.

---

## 17. Version History
- v0.1 — Initial handoff.
- v0.2 — PRD v1.0 confirmed.
- v0.3 — Data Model, Tech Stack, Navigation Map v0.1 drafted.
- v0.4 — All v1.0 artifacts confirmed.
- v0.5 — Pivoted to Workers + vinext. Repo created. Node 22 locked.
- **v0.6 — Skeleton built and running locally. Devcontainer fixed to Debian 12. Compatibility date corrected to 2026-10-07. `cf` CLI confirmed as the deploy path. Multiple v0.5 tech-stack corrections applied. Ready for first deploy.**