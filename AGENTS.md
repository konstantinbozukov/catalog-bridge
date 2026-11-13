# AGENTS.md — Catalog Bridge

Rules for any Cursor agent working in this repository. Read this before editing. The 2026-11-13 evidence audit is `docs/AUDIT.md`. If the tree and that audit disagree, trust the tree.

## What this is

Catalog Bridge (hub idea `018`) is a planned kitchen constraint-comparison catalog that earns Amazon Associates commissions. A later phase, the Answer Insertion Engine, would find purchase-intent questions in existing communities and link to our comp pages.

Two products, one hard rule:

1. **Catalog** — constraint pages such as “best air fryer for a small kitchen under $100”, with Associates links only on our site.
2. **Answer Insertion Engine (phase 2+)** — helpful answers in communities that link to a matching comp page, then a weekly P&L.

**Hard rule:** external posts never use raw Amazon affiliate URLs. They point at our comp pages. Match the page to the question. Do not write “we tested” unless the page records a real test. Do not make health, medical, or pressure-cooker safety claims.

Success for this idea is affiliate P&L only. Do not import metrics from the separate “007 Production Factory” project.

This repository is the planning scaffold for that product. It is not a running site. See maturity notes in `docs/AUDIT.md`.

## Stack

Proposed in `TECH_PLAN.md` (lines 67–81). Nothing below is installed in this repo. There is no `package.json`, lockfile, or `apps/` tree.

| Layer | Proposed choice | In this repo today |
|-------|-----------------|--------------------|
| Site | Next.js 15 App Router, static/SSG, under `apps/web/` | Directory absent |
| Content | MDX (preferred) or JSON in `content/comps/` | Only `content/comps/.gitkeep` |
| Styling | Tailwind | Not set up |
| Hosting | Vercel, or a static CDN | No `vercel.json`, no GitHub deployments |
| Analytics | Plausible or similar, plus outbound click events | Not chosen or wired |
| Associates | Tag in env, link builder on our domain only | Names only, in `.env.example`, all commented |
| Venue ops (v0) | `data/venue-queue.json` and `data/pl/` | Queue file present; P&L template is a header row |
| Drafting (v0) | Manual, optional local LLM, human approves posts | No code |
| Data (phase 2) | Supabase Postgres | Not in use. Do not add it during v0 |
| Engine (phase 2) | Thin orchestrator plus local writer | Out of scope until the vertical slice is proven |

`npm view next version` on 2026-11-13 returned `16.4.6`. The plan still says Next.js 15. **TODO: confirm with Konstantin** before scaffolding an app. Also confirm MDX vs JSON, and npm vs pnpm vs yarn. `.gitignore` mentions all three package managers.

## Folder layout

Actual tree (tracked files as of `6c5c74b` on `master`, plus these two docs):

```
AGENTS.md                 # this file
BUSINESS_CASE.md          # market, risks, pursue decision
README.md                 # product summary and doc index
TECH_PLAN.md              # architecture and phases
VERTICAL_SLICE.md         # first proof path and acceptance checklist
.env.example              # variable names only; every assignment is commented
.gitignore
content/comps/.gitkeep    # comp pages belong here; none written
data/venue-queue.json     # 20 venue rows (17 candidate, 3 deferred)
data/post-log.json        # empty array
data/pl/week-template.csv # header only
docs/AUDIT.md             # evidence audit, 2026-11-13
docs/comp-methodology.md
docs/va-quora-playbook.md
docs/va-pinterest-playbook.md
docs/va-medium-playbook.md
```

`README.md` (around the “Repo layout” section) and `VERTICAL_SLICE.md` (“Repo layout (target)”) also name `apps/web/` for a phase-1 Next.js site. That directory is not in the repo. Do not create it unless Konstantin asks for the site scaffold.

Local path recorded in `README.md`: `E:\AI\catalog-bridge`. GitHub: https://github.com/konstantinbozukov/catalog-bridge. **TODO: confirm with Konstantin** whether the Windows folder has commits that were never pushed. GitHub `pushed_at` for this repo is 2026-08-10.

## Install, run, test, lint, build

There is no install, dev server, test, lint, or build command in this repo. Verified from `/workspace` on 2026-11-13 (Node v22.23.4, npm 10.9.9). Do not treat a failure below as a broken app. There is no app.

| Command | Result |
|---------|--------|
| `npm install` | Exit with `ENOENT`: cannot read `/workspace/package.json` |
| `npm test` | Same `ENOENT` on `package.json` |
| `pnpm test` | `ERR_PNPM_NO_IMPORTER_MANIFEST_FOUND` (no `package.json` in `/workspace`) |
| `python3 -m pytest` | `No module named pytest` (no Python project here) |
| `make test` | `No rule to make target 'test'` (no Makefile) |
| `npx --no-install next lint` | Canceled before install: `missing packages and no YES option: ["next@16.4.6"]`. Nothing was linted and nothing was added to the repo |

No `*.test.*`, `*.spec.*`, or `tests/` directory exists. When a real app exists, document the exact scripts here after running them. Until then, do not invent `npm run dev` or `npm run build` as project commands.

`data/venue-queue.json` parses as JSON (20 objects, 20 unique `id`s). `data/post-log.json` is `[]`.

## Deploy

Documented target: Vercel for the catalog site (`TECH_PLAN.md` integrations table: status “Not deployed”). No deploy is implemented.

- No `vercel.json`, no `.vercel/`, no Dockerfile, no `.github/workflows`.
- GitHub Actions workflow count: 0. GitHub deployments: 0. GitHub Pages: not enabled (API 404). Default branch `master` is not protected.
- This audit’s Vercel token could not list projects (HTTP 403 on its default team, and the team list was empty). That check is not evidence about Konstantin’s own Vercel account. **TODO: confirm with Konstantin** whether a Vercel project already exists outside this repo.

Do not deploy, link a Vercel project, or add a pipeline unless Konstantin asks.

## External services

| Service | Role in the plan | Wired in this repo |
|---------|------------------|--------------------|
| Amazon Associates | Commission attribution. P0 | Status in the plan: unknown. No tag value is stored |
| Vercel | Host the catalog. P0 | Not deployed, per the plan and the GitHub deployment list |
| Google Search Console | Index comps after 3+ pages. P1 | Not set up here |
| Plausible (or similar) | Page views and outbound clicks. P1 | Optional; domain env name only |
| Quora, Pinterest, Medium | VA-assisted posts. P1 | Playbooks only. `data/post-log.json` is empty |
| Reddit API | Discover and post. P2 | Deferred in the venue queue (`account_gate` `karma`). No OAuth app in repo |
| Supabase | Engine database. P2 | Not used. Connected Supabase account visible to this audit has projects named EDGE and Askelon. Neither name appears in this repo. **TODO: confirm with Konstantin** they are unrelated |
| Facebook Groups | Later audience. P3 | Explicitly out of v0 |
| AWS | — | Not mentioned and not used |

Phase 2 services stay out of the repo until the vertical slice in `VERTICAL_SLICE.md` is accepted.

## Coding conventions

There is no application code yet. Match the files that exist:

- Planning docs start with a title, work name, hub idea `018`, and a status line. Cross-link with relative markdown links.
- Comp slugs are kebab-case, as in `VERTICAL_SLICE.md` (for example `air-fryer-small-kitchen-under-100`).
- `data/venue-queue.json` is a JSON array, 2-space indent, snake_case keys: `id`, `adapter`, `name`, `comp_slugs`, `status`, `rules_notes`, `account_gate`. `id` is kebab-case. `status` values used today are `candidate` and `deferred`. `adapter` values used today are `quora`, `pinterest`, `medium`, `reddit`, `seo`.
- `data/pl/week-template.csv` uses snake_case headers and an `_usd` suffix on money columns.
- VA playbooks are checklists: warm the account, match the constraint, catalog URL only, disclose, log the post.
- Comp pages, when written, follow `docs/comp-methodology.md`: constraint in the title, 3–5 ASINs, best-for / skip-if, comparison table, FTC disclosure, `updated_at`, price as a band or “as of” date.
- Keep secrets out of git. Env examples stay commented placeholders.

Do not add a new framework, database, or posting automation as a drive-by. v0 scope is the acceptance checklist in `VERTICAL_SLICE.md`.

## Never touch

- **Secrets.** Do not commit or paste Associates tags, Reddit client secrets, Supabase service-role keys, or analytics private keys. Do not print secret values in docs, PRs, or chat.
- **`.env` files.** `.env`, `.env.local`, and `.env.*.local` are gitignored. Edit `.env.example` names only, and keep assignments commented. Do not copy a real env file into the repo.
- **Production data.** Do not invent commission dollars, click counts, or a filled P&L. `data/pl/*.local.*` is gitignored local data. Leave it alone. Do not mark the vertical-slice checklist done unless the live page, post, or Associates report exists.
- **Migrations.** There is no database and no migration directory. Do not add or apply Supabase migrations in v0.
- **Store and deploy config.** Do not add or edit Vercel project settings, `vercel.json`, Edge Config, blob stores, or DNS. `.vercel/` is gitignored.
- **Live posting.** Do not publish Quora, Pinterest, Medium, or Reddit posts, and do not put raw `amazon.com` affiliate links into playbooks, the queue, or comp pages’ off-site copy.

`.env.example` variable names (no values are set): `ASSOCIATES_TAG`, `ASSOCIATES_TRACKING_ID`, `NEXT_PUBLIC_SITE_URL`, `NEXT_PUBLIC_PLAUSIBLE_DOMAIN`, `REDDIT_CLIENT_ID`, `REDDIT_CLIENT_SECRET`, `SUPABASE_URL`, `SUPABASE_SERVICE_ROLE_KEY`. Two tag names are listed. **TODO: confirm with Konstantin** which name the future link builder should read before any code uses them.
