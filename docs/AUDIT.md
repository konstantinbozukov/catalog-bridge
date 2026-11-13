# Catalog Bridge — repository audit

**Date:** 2026-11-13  
**Repo:** https://github.com/konstantinbozukov/catalog-bridge  
**Audited commit:** `6c5c74b86bec74300b5c9e013d79df4c6f20b7c1` on `master` (the tip before this doc PR)  
**Scope:** documentation-only. No application code, config, dependencies, CI, or infrastructure was changed. Commands below were run from a clean checkout at `/workspace`.

## Summary

Catalog Bridge is a written plan for a kitchen-gadget comparison site that would earn Amazon Associates commissions, plus a later step that answers existing community questions and links back to those pages. On 2026-11-13 the repo is a **prototype / planning scaffold**, not a working product and not production. It holds two commits from 2026-08-10. `git show --stat` reports 979 insertions in `9602ee2` and 2 insertions in `6c5c74b`. `wc -l` on the tracked files summed to 980 because `data/post-log.json` is the two characters `[]` with no trailing newline. `content/comps/` contains only a `.gitkeep`. There is no site, no domain recorded in the repo, no Associates tag value, no tests, and no deploy. The vertical-slice acceptance checklist in `VERTICAL_SLICE.md` (lines 123–132) is entirely unchecked. GitHub reports the project as public, language empty, 0 issues, 0 pull requests, and `pushed_at` 2026-08-10 — 95 days before this audit.

## Stack and hosting

The stack is a proposal in `TECH_PLAN.md` lines 67–81, not a dependency tree.

- **Site:** Next.js 15 App Router, static/SSG, intended at `apps/web/`. That directory is absent.
- **Content:** MDX preferred, or JSON, in `content/comps/`. Only `content/comps/.gitkeep` (lines 1–2) is present.
- **Styling:** Tailwind, version not named.
- **Hosting:** Vercel (or a static CDN). Plan status at `TECH_PLAN.md` line 153: “Not deployed”.
- **Analytics:** “Plausible or similar”, not chosen.
- **v0 data:** JSON and CSV in `data/`. No database.
- **Phase 2:** Supabase Postgres and a thin orchestrator. Both are explicitly deferred.
- **AWS:** not referenced.

GitHub Pages is not enabled (Pages API returned 404). The repo `homepage` field is null. No Dockerfile and no `vercel.json`.

The Supabase account connected to this audit lists two projects, named **EDGE** and **Askelon**. Neither name appears in this repository, and `SUPABASE_URL` is unset. **TODO: confirm with Konstantin** that neither project is meant for Catalog Bridge.

## Deploy path

No deploy path runs today.

1. The plan says: build the Next.js catalog and host it on Vercel (`TECH_PLAN.md` lines 74 and 153).
2. The repo has no framework config, no install manifest, and no `.github/` directory.
3. GitHub Actions workflow count is 0. GitHub deployment count is 0. `master` has no branch protection.
4. A Vercel project list could not be completed with the token available to this audit (HTTP 403 on that token’s default team; team list length 0). Treat “not deployed” as the repo and GitHub evidence, not as a search of Konstantin’s Vercel teams. **TODO: confirm with Konstantin** if a project exists on another account.

Nothing in this repo builds an artifact that a host could serve.

## Environment variable names

Names only, from `.env.example` lines 4–17. Every line is a comment. No values are set. The placeholder text on those lines (`your-tag-20`, `...`, `https://your-domain.com`, `your-domain.com`) is not a credential.

| Name | Noted role |
|------|------------|
| `ASSOCIATES_TAG` | Amazon Associates tag |
| `ASSOCIATES_TRACKING_ID` | Second tag-like name; which one the link builder should read is undefined |
| `NEXT_PUBLIC_SITE_URL` | Public site origin |
| `NEXT_PUBLIC_PLAUSIBLE_DOMAIN` | Optional analytics domain |
| `REDDIT_CLIENT_ID` | Phase 2, commented |
| `REDDIT_CLIENT_SECRET` | Phase 2, commented |
| `SUPABASE_URL` | Phase 2, commented |
| `SUPABASE_SERVICE_ROLE_KEY` | Phase 2, commented |

No other env names appear in the tree. `NEXT_PUBLIC_*` would be exposed to the browser under Next.js conventions. The Reddit secret and the Supabase service-role key are not given a `NEXT_PUBLIC_` prefix.

`.gitignore` lines 13–15 ignore `.env`, `.env.local`, and `.env.*.local`. Line 31 ignores `.vercel`. Line 34 ignores `data/pl/*.local.*`.

## Tests

**Coverage: none. Nothing passed, because there is no suite.**

Searched the tree for `package.json`, `pnpm-lock.yaml`, `package-lock.json`, `yarn.lock`, `requirements.txt`, `pyproject.toml`, `go.mod`, `Cargo.toml`, `Makefile`, `Dockerfile`, `vercel.json`, and any `*.yml` workflow. None are present. No `*.test.*`, `*.spec.*`, or `tests/` files.

Runtime used: Node v22.23.4, npm 10.9.9, pnpm and yarn installed on the machine, system `python3`.

| Command | What happened |
|---------|----------------|
| `npm test` | Failed immediately. `npm error code ENOENT`, `syscall open`, path `/workspace/package.json`, `errno -2`, “Could not read package.json”. |
| `npm install` | Same `ENOENT` on `/workspace/package.json`. No packages were installed. |
| `pnpm test` | `ERR_PNPM_NO_IMPORTER_MANIFEST_FOUND`. No `package.json` (or `package.yaml` / `package.json5`) in `/workspace`. |
| `python3 -m pytest` | `/usr/bin/python3: No module named pytest`. |
| `make test` | `make: *** No rule to make target 'test'. Stop.` |
| `npx --no-install next lint` | Did not lint. `npx canceled due to missing packages and no YES option: ["next@16.4.6"]`. The cancel avoided adding Next.js to the machine as part of this audit. |

`data/venue-queue.json` was parsed with Python: 20 objects, 20 unique `id`s, statuses `candidate` 17 and `deferred` 3, adapters `quora` 5, `seo` 5, `pinterest` 4, `medium` 3, `reddit` 3. Slugs referenced: `air-fryer-small-kitchen-under-100`, `air-fryer-1-2-people`, `compact-kettle-small-kitchen`, `vegetable-chopper-beginners`, `milk-frother-under-25`. Those match the proposed slugs in `VERTICAL_SLICE.md` lines 38–42. No file under `content/comps/` implements them.

## Bugs and likely bugs

These are documentation and data defects. There is no application to crash.

1. **The Medium playbook assumes a live domain.** `docs/va-medium-playbook.md:7` says “Host domain already has some SEO juice”. `TECH_PLAN.md:202` still lists the domain as TBD. `TECH_PLAN.md:153` lists Vercel as not deployed. No comp page exists to syndicate (`content/comps/.gitkeep:1-2`). A VA following the playbook is told the site already ranks.

2. **Venue rows are not postable targets.** Each object has `id`, `adapter`, `name`, `comp_slugs`, `status`, `rules_notes`, and `account_gate` (shape at `data/venue-queue.json:2-10`). None of the 20 objects has a question URL, pin URL, or subreddit permalink. The queue cannot be executed as written. The “~20 venues” count in `VERTICAL_SLICE.md:57` and the checkbox at `VERTICAL_SLICE.md:128` match the file’s length, and that checkbox is still open.

3. **One Pinterest row points at two different comps.** `data/venue-queue.json:51` maps `pinterest-air-fryer-small` to both `air-fryer-small-kitchen-under-100` and `air-fryer-1-2-people`. `docs/va-pinterest-playbook.md:16` says the pin link is a single comp-page URL. Those two slugs are different constraints (budget vs household size) in `VERTICAL_SLICE.md:38-39`. The pin has no single destination. Reddit rows at `data/venue-queue.json:114`, `:123`, and `:132` also list two slugs; those are deferred (`status` `deferred`) and a human can pick one thread. The Pinterest row is `candidate`.

4. **SEO rows use the same status as venues a person can post to.** `data/venue-queue.json:137-145` is a Google query with `"adapter": "seo"` and `"status": "candidate"`, same status as the Quora rows. Nothing in the file tells a poster to skip `seo`. Five of the twenty rows are search intents, not accounts.

5. **Post log has no shape, and the playbooks disagree on its name.** `data/post-log.json:1` is `[]`. `docs/va-quora-playbook.md:17` names `data/post-log.json`. `docs/va-pinterest-playbook.md:25` says “post log” with no path. `docs/va-medium-playbook.md:31` says “post log” with no path. `VERTICAL_SLICE.md:60` asks for URL, venue, comp slug, and date. An empty array does not record that shape, so the first logger has to invent it.

6. **P&L template can double-count shared costs.** `data/pl/week-template.csv:1` is a header only: `week_start`, `channel`, and separate columns for hosting, domain, VA, compute, misc, `total_opex_usd`, clicks, and commission. `VERTICAL_SLICE.md:69-70` asks for cost lines (hosting, domain, VA hours, compute, commission) and a week-1 row even if commission is $0. The CSV is one wide row per `channel`. A row per channel that also fills `hosting_usd` and `domain_usd` repeats fixed costs. No week-1 data row exists. `total_opex_usd` has no formula in the repo.

7. **Copying `.env.example` defines nothing, and two tag names collide.** Line 1 says to copy the file to `.env.local`. Lines 4–17 are comments, so the copy sets no variables. `ASSOCIATES_TAG` (line 4) and `ASSOCIATES_TRACKING_ID` (line 5) are both present with no note about which one a link builder must read.

8. **Comp slugs in the queue do not resolve to pages.** Example: `data/venue-queue.json:6` references `air-fryer-small-kitchen-under-100`. The only content file is `content/comps/.gitkeep:1-2`. Expected for a scaffold, and still a broken reference if the queue is treated as current work.

9. **“Content schema sketch” is easy to over-read.** `TECH_PLAN.md:87` lists a content schema sketch as shipped at graduation. The field list is the prose table at `TECH_PLAN.md:129-133` (`slug`, `title`, `constraint`, `picks[]`, `table`, `updated_at`, `methodology`). There is no sample MDX, no frontmatter, and no JSON Schema. The optional Next.js shell at `TECH_PLAN.md:89` is explicitly optional and is absent, which matches that line.

10. **Reddit gate has no number.** `data/venue-queue.json:116` sets `"account_gate": "karma"` with no threshold. `rules_notes` says “Karma warm required” and does not say how much.

The acceptance checklist (`VERTICAL_SLICE.md:123-132`) correctly stays open for the live site, disclosures on real pages, tagged outbound links, click analytics, three live posts, a filled post log, a week-1 P&L row, and a check that no external post used a raw affiliate URL. Those items are not done. They are missing deliverables, listed here so the open boxes are tied to files.

## Security

**Committed secrets: none found.**

- Full-text search for private keys, AWS access-key ids, GitHub and Slack token prefixes, and JWT-shaped strings matched one line: the word “Secrets” in `TECH_PLAN.md:81` (a table cell that says secrets belong in env vars). No live token was in that line.
- GitHub secret-scanning alerts for this repo: 0. Secret scanning is enabled and push protection is enabled. Dependabot security updates are **disabled**.
- `.env.example` holds commented placeholders only. Do not treat `your-tag-20` as a tag that was issued.
- The repo is **public** (`visibility: public`, created 2026-08-10). The venue list, rate limits, and kill rules are world-readable. That is an information exposure of the operating plan, not a credential leak. **TODO: confirm with Konstantin** that public is intentional.
- `master` is not protected. Anyone with write access can push straight to it. That is low impact while the repo is docs, and it becomes the default path for a future app.
- `data/post-log.json` is tracked. It is empty. If later rows include account emails or session notes, those would be public. URLs of public posts are fine to commit.
- Phase 2 names `REDDIT_CLIENT_SECRET` and `SUPABASE_SERVICE_ROLE_KEY` are server-side names (no `NEXT_PUBLIC_` prefix). Keep them that way. A service-role key bypasses row-level security; it must not ship to the browser.
- No migrations, no production database, and no committed `.env`. Affiliate-policy risk (thin pages, raw affiliate links off-site, undisclosed commissions) is written down in `TECH_PLAN.md` lines 164–173 and `docs/comp-methodology.md`. The risk is unrealized because no pages or posts exist.

## Dependencies and staleness

- **Dependencies installed or declared: none.** No lockfile, so `npm outdated` does not apply. `.gitignore` already ignores `node_modules/`, `.next/`, `out/`, `dist/`, `.pnp`, npm and pnpm debug logs, and `.vercel`, ahead of any toolchain choice.
- **Proposed Next.js pin vs the registry on audit day.** `TECH_PLAN.md:71` says Next.js 15. `npm view next version` returned `16.4.6`. The highest `15.x` version printed by `npm view next@15 version` was `15.5.29`. Tailwind, React, and MDX are not version-pinned at all. **TODO: confirm with Konstantin** whether a future scaffold should stay on Next.js 15 or track current 16. This audit did not install Next.js.
- **Doc staleness.** Last commit is 2026-08-10 (`6c5c74b`, author `bozukov <kbozukov@gmail.com>`, “Add GitHub remote URL to README”). The graduation commit is `9602ee2` the same day. No product commit has landed in the 95 days since. `BUSINESS_CASE.md:88` and `:149` already say the ~4.5% kitchen commission band must be checked against Amazon’s live fee table. This audit did not re-verify that rate.
- **Dependabot** security updates are disabled. Harmless with zero packages. It will matter on the first `package.json` if it stays off.
- No CI config exists to go stale.

## Top 5 improvements

Ranked by value toward the only stated success metric (attributed commission versus weekly opex), then by how much machinery each one adds. Calendar estimates from the existing plans are not repeated here.

1. **Publish the five comp pages on a real site.** Value is highest: every other step links to pages that do not exist. Effort is medium: a new Next.js app at the planned `apps/web/` path and five pages under `content/comps/` matching `docs/comp-methodology.md` and the slugs in `VERTICAL_SLICE.md:38-42`. No existing code has to be migrated. Do this before any insertion engine.

2. **Register the domain and submit the Associates application.** Value is high because commissions cannot be attributed without a tag, and `TECH_PLAN.md:152-153` marks Associates as unknown and hosting as not deployed. Effort is low in the repo (record the decision; keep the tag in an untracked env file) and is gated by Amazon’s review, which is outside the codebase. Start it in parallel with item 1. `TECH_PLAN.md:202` still has the domain as an open decision.

3. **Turn the queue into three real posts plus a week-1 P&L row.** Value is high: this is the proof path in `VERTICAL_SLICE.md` lines 11–15. Effort is low in git once pages exist: add a concrete URL field on the rows that will actually be posted, append objects to `data/post-log.json`, and add one real week row to the P&L only from actual opex. Do not fabricate clicks or commission. Leave Reddit and the engine deferred.

4. **Record outbound clicks on on-site Amazon links.** Value is high for the same proof (acceptance line `VERTICAL_SLICE.md:127`). Effort is low once a page exists: one click event on the on-site outbound link, into Plausible or a log. The vendor is still an open decision at `TECH_PLAN.md:204`. Without this, `outbound_clicks` in the CSV cannot be filled from evidence.

5. **Add a check that runs with the first page.** Value is medium-high: it protects the Associates account, which `BUSINESS_CASE.md` treats as the termination risk. Effort is low: a small script or, later, CI that fails when a comp is missing disclosure, methodology, or `updated_at`, and when tracked files outside the site’s outbound helper contain a raw Amazon affiliate URL. In the same change, delete the duplicate tag name so only one of `ASSOCIATES_TAG` or `ASSOCIATES_TRACKING_ID` remains. Ship this with the first page, not after a large app exists.

Explicitly not in this list: Supabase, Reddit OAuth, the 100-venue loop, Facebook, pets, or a second merchant rail. The current plans already defer them, and the slice that would justify them is unstarted.

## Open questions for Konstantin

1. Does `E:\AI\catalog-bridge` contain work that was never pushed? GitHub’s last push is 2026-08-10.
2. Has a domain been chosen, and has an Amazon Associates application been started, approved, or not started?
3. Which env name should a link builder read, `ASSOCIATES_TAG` or `ASSOCIATES_TRACKING_ID`?
4. For the first scaffold: MDX or JSON, which package manager, and Next.js 15 or current 16 (`16.4.6` on the registry on 2026-11-13)?
5. What should count outbound clicks for v0: Plausible, another vendor, or server logs?
6. Is the public GitHub repo intentional, given that `data/venue-queue.json` and the VA playbooks are world-readable?
7. Are the Supabase projects EDGE and Askelon unrelated to this repo? Is there already a Vercel project this audit could not see?
8. Who posts the first three placements — a hired VA, or you — and is the proposed 4–6 week zero-commission pause the rule to use?
9. Should the five `seo` rows stay in `data/venue-queue.json`, or move to a separate indexing list so `candidate` means “a person can post this”?
10. Is the kitchen fee still about 4.5% on Amazon’s current compensation table? The business case flags that figure as unverified (`BUSINESS_CASE.md:88` and `:149`).
