# GLOSSARY.md
> Purpose: Plain-language definitions of every term we use.
> If Master asks "what does X mean?", it should already be here.
> Add new terms as they come up.

## Roles
| Term | Meaning |
|---|---|
| **Master** | You. The product owner. Final decision-maker. |
| **AI** | Me. Architect, developer, translator. Adviser, not decider. |

## Product & Business
| Term | Meaning |
|---|---|
| **SaaS** | Software as a Service. Software you access online, usually by subscription. |
| **CRM** | Customer Relationship Management. Software to track contacts, leads, deals, and jobs. |
| **Multi-tenant** | One system serving many businesses. Each business is a "tenant." |
| **Tenant** | A single business using the platform. Has its own data, users, and settings. |
| **tenant_id** | A unique identifier stamped on every record so tenants never see each other's data. |
| **MVP** | Minimum Viable Product. The smallest version that is useful and shippable. |
| **PRD** | Product Requirements Document. The "what and why" of the product. |
| **Handoff** | A single file summarizing project state so the next chat can continue cleanly. |
| **Freemium** | Free tier plus paid tiers. |
| **Per-seat** | Pricing per user. 5 users = 5 seats. |
| **Trial** | Low-cost introductory period before full pricing kicks in. |
| **Add-on** | Extra feature or user purchased on top of a plan. |

## Development Environment
| Term | Meaning |
|---|---|
| **Node.js** | A runtime that runs JavaScript outside a browser. Next.js needs it. |
| **Node 22** | The version of Node.js we use. Even numbers are stable (LTS). |
| **npm** | Node Package Manager. Downloads and tracks libraries. Comes with Node. |
| **pnpm** | Faster alternative to npm. Shares libraries across projects. We chose npm. |
| **yarn** | Another alternative to npm. We do not use it. |
| **package.json** | A file listing every library the project needs. |
| **node_modules** | The folder where installed libraries live. |
| **Lockfile** | A file (`package-lock.json`) recording exact versions installed. |
| **Codespaces** | GitHub's cloud dev environment. Runs VS Code in the browser. |
| **Devcontainer** | A config file that defines the dev environment (Node version, extensions). |
| **GitHub** | Where our code lives. Repo host. |
| **Repo** | Short for repository. The project folder tracked by Git. |
| **Commit** | A saved snapshot of changes. |
| **Push / Sync** | Sending commits from Codespace to GitHub. |
| **Private repo** | A repo only you can see. |

## Frameworks & Code
| Term | Meaning |
|---|---|
| **Next.js** | The React framework we use. Handles pages, routing, and server logic. |
| **App Router** | Next.js's modern routing system. Files in `app/` become routes. |
| **React** | The UI library Next.js is built on. Components, state, rendering. |
| **TypeScript** | JavaScript with types. Catches errors before runtime. |
| **Tailwind CSS** | Utility-first styling. Classes like `flex p-4` instead of CSS files. |
| **shadcn/ui** | Pre-built, copy-paste UI components built on Tailwind. |
| **Server Components** | React components that run on the server, not the browser. |
| **Server Actions** | Functions that run on the server when a form is submitted. |
| **Route handlers** | Server-side functions that respond to HTTP requests (like `/api/...`). |
| **Middleware** | Code that runs before a request reaches a page. Used for auth, tenant checks. |

## Cloudflare
| Term | Meaning |
|---|---|
| **Cloudflare** | The platform we deploy on. CDN, edge compute, database, storage. |
| **Workers** | Cloudflare's serverless runtime. Where our app runs. |
| **Pages** | Cloudflare's older hosting product. We pivoted away from it. |
| **Pages Functions** | Serverless functions tied to Pages. Superseded by Workers for us. |
| **vinext** | Cloudflare's official Next.js-on-Workers adapter. Vite-based. Our choice. |
| **next-on-pages** | The legacy Pages adapter for Next.js. We do not use it. |
| **OpenNext** | A third-party adapter. Safer but slower. We did not choose it. |
| **Vite** | The build tool vinext is built on. Fast. |
| **D1** | Cloudflare's SQL database (SQLite). Our main DB. |
| **KV** | Cloudflare's key-value store. Fast. We use it for sessions and cache. |
| **R2** | Cloudflare's file storage (like S3). For future file uploads. |
| **Cron Triggers** | Scheduled Workers jobs. We use them for recurring jobs and reminders. |
| **Bindings** | How Workers connect to D1, KV, R2. Declared in config. |
| **Compatibility date** | A date that tells Cloudflare which runtime behavior to use. |
| **nodejs_compat** | A flag that enables Node.js APIs in Workers. On by default for recent dates. |
| **Workers runtime / V8** | The JavaScript engine Workers runs on. Not Node.js. |

## Auth & Security
| Term | Meaning |
|---|---|
| **OAuth** | Logging in via a third party (e.g. "Sign in with Google"). |
| **Session** | A record that says "this user is logged in." Stored in KV. |
| **Password hash** | A scrambled version of a password. Never store plain passwords. |
| **GDPR** | EU privacy law. Strong user rights over data. |
| **CCPA / CPRA** | California privacy laws. Similar to GDPR. |
| **DPA** | Data Processing Agreement. Contract between controller and processor. |
| **Subprocessor** | A third party the platform uses (e.g. Cloudflare, Stripe). |
| **Consent** | Explicit user agreement. Must be logged with timestamp and IP. |
| **Controller** | The party deciding why and how data is used. Tenant = controller for its customers. |
| **Processor** | The party acting on the controller's behalf. Platform = processor. |

## Compliance & Rights
| Term | Meaning |
|---|---|
| **Data export** | Giving users their data in a usable format (CSV, PDF, Google Sheets). |
| **Right to erasure** | User's right to have their data deleted. |
| **Soft delete** | Marking data as deleted but keeping it recoverable for 30 days. |
| **Hard delete** | Permanently removing data from the database. |
| **Audit log** | A record of who did what, when. |

## Terms We Do Not Use
| Term | Why |
|---|---|
| **Wrangler** | Cloudflare's CLI. Master does not use CLI. Dashboard only. |
| **npm CLI** | Master does not use terminal for package management outside Codespaces. |
| **Turbopack / webpack configs** | Not applicable with vinext. |