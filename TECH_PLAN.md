# Technology Implementation Plan — Catalog Bridge

**Work name:** Catalog Bridge  
**Hub idea:** `018` Amazon Affiliate Catalog  
**Status:** Graduated 2026-08-10  
**Product repo:** `E:\AI\catalog-bridge`

## Summary

Build a **kitchen constraint-comparison catalog** that earns Amazon Associates commissions, plus (later) an **Answer Insertion Engine** that finds purchase-intent questions in existing communities and bridges to our comps pages — never raw affiliate URLs off-site. v0 proves **catalog → click → commission** with five comp pages, Associates compliance, a venue queue, and VA-assisted posting. Scale path: few adapters × 100+ venues, P&L allocator, demand-driven comps.

## Goals & non-goals

**Goals**
- Publish 5 kitchen constraint comp pages with valid methodology (spec + review signal, not fake “we tested”)
- Associates-tagged “View on Amazon” links **only on our domain**; FTC disclosure on every page
- Venue Queue v0 (~20 targets) mapped to comp pages
- VA-assisted insertion on Quora/Pinterest/Medium (fast eyes) while SEO indexes
- Weekly P&L stub: commissions vs domain + hosting + VA + compute
- Repo scaffold ready for SEO comps adapter and Reddit adapter (phase 2)

**Non-goals (vertical slice / v0 graduate)**
- Full Answer Insertion Engine with auto-post at scale
- 100+ live venues or automated venue discovery loop
- Reddit OAuth auto-post (needs karma warm + commercial API check)
- Facebook Groups (no warmed allowlist yet)
- Multi-merchant commission rails
- Email capture, embeds, deal bursts, pets niche clone
- Dogfooding **007** factory metrics as 018 success criteria (learn across; don’t mix)

## Users & jobs

| Persona | Job to be done | Critical path |
|---------|----------------|---------------|
| Shopper | Pick the right kitchen gadget for a constraint (space, budget, household size) | Land on comp page → compare → Amazon |
| Operator (founder) | Prove weekly commissions ≥ opex on kitchen beachhead | Comps + venue posts + P&L |
| VA | Warm accounts; post approved drafts where API doesn’t exist | Checklists + approve queue |
| Engine (later) | Discover intent → match comp → draft → gate → post | Adapters after v0 proof |

## System overview

```
┌─────────────────────────────────────────────────────────────┐
│  Venues (Quora, Pinterest, Medium, Reddit, SEO, …)          │
│  External posts link ONLY to catalog-bridge comp URLs       │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│  Catalog Bridge (our site)                                   │
│  • Constraint comp pages (MDX/content)                       │
│  • Comparison tables, methodology, disclosure                │
│  • Associates-tagged Amazon outbound links                   │
│  • Analytics: page view → outbound click                   │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
                    Amazon Associates
                           │
                           ▼
                    Commission reports → P&L

Phase 2 — Answer Insertion Engine (not v0):
  adapters → discover → match comps → draft → gate → publish → meter
```

## Stack (proposed)

| Layer | Choice | Why |
|-------|--------|-----|
| Site | Next.js 15 App Router, static/SSG | SEO-friendly comps; same pattern as other graduated repos |
| Content | MDX or JSON comp schema in `content/comps/` | Versioned picks, tables, constraints; easy local LLM assist |
| Styling | Tailwind | Fast, consistent comparison tables |
| Hosting | Vercel (or static CDN) | Low opex; fast deploy |
| Analytics | Plausible or similar + outbound click events | Privacy-simple; measure catalog→Amazon |
| Associates | Site-stored tag; link builder utility | Tags never leave our domain in external posts |
| Venue ops (v0) | `venue-queue.json` + spreadsheet P&L | No DB until insertion engine needs it |
| Drafting (v0) | Manual + optional local LLM assist | Human/VA approves all external posts |
| Data (phase 2) | Supabase Postgres | Venue allowlist, post log, commission import, P&L lines |
| Engine (phase 2) | Thin orchestrator + local writer | Discover/match/post/meter per adapter |
| Secrets | Env vars / Cursor sync pattern | Associates IDs, API keys server-only |

## Core capabilities

### Shipped at graduation (scaffold)
1. Repo `catalog-bridge` with README, TECH_PLAN, BUSINESS_CASE, VERTICAL_SLICE
2. Content schema sketch for comp pages
3. `.gitignore`, `.env.example`
4. Placeholder site shell (optional minimal Next.js — can be phase 0.5 first commit in repo)

### Vertical slice (v0 — see VERTICAL_SLICE.md)
1. **5 comp pages** — kitchen constraints (air fryer ×2, kettle, chopper, frother)
2. **Methodology + disclosure** components on every page
3. **Amazon link helper** — tagged outbound URLs, `rel` attributes, price “as of” dates
4. **Venue Queue v0** — ~20 targets with adapter type, comp mapping, rules notes
5. **VA playbooks** — Quora/Pinterest/Medium post checklists (docs)
6. **P&L stub** — weekly template: commission vs opex by channel
7. **3+ live venue posts** pointing at comp pages (manual/VA)

### Phase 1 — Fast eyes + SEO base
- Index comps in Search Console
- Quora + Pinterest + Medium assisted posts from venue queue
- Demand log: unmatched asks → next comp candidates

### Phase 2 — Insertion engine MVP
- Reddit adapter: discover + draft; approve queue → OAuth post (after karma warm)
- SEO publish adapter: auto-deploy new comps from demand queue
- Ban/risk scoring; rate limits per venue
- Supabase: posts, clicks, venue health

### Phase 3 — Portfolio scale
- Venue discovery loop → 100+ allowlisted venues
- P&L capital allocator (throttle losers, raise caps on winners)
- Pinterest/Medium scheduling assist
- FB Groups after VA warm allowlist

### Phase 4 — Multipliers (post kitchen proof)
- CartComps-like compare utility (constraint picker)
- Email capture
- Second merchant rail
- Embed widget; deal-triggered bursts
- Pets niche clone playbook

## Data model (sketch)

### v0 (files)

| Entity | Storage | Fields |
|--------|---------|--------|
| `Comp` | `content/comps/*.mdx` | slug, title, constraint, picks[], table, updated_at, methodology |
| `Pick` | inside Comp | asin, label, best_for, skip_if, spec highlights |
| `Venue` | `data/venue-queue.json` | id, adapter, name, rules, comp_slugs[], status, notes |
| `P&L week` | spreadsheet / `data/pl/` | week, channel, opex, clicks, commission |

### Phase 2 (Supabase)

| Table | Purpose |
|-------|---------|
| `comps` | Published comp metadata + slug |
| `picks` | ASIN rows per comp |
| `venues` | Allowlisted surfaces |
| `intents` | Discovered questions/threads |
| `drafts` | Generated answers + approval state |
| `posts` | Published placement log |
| `clicks` | Catalog + outbound events |
| `pl_weeks` | Commission vs opex rollup |

## Integrations

| System | Purpose | Priority | Status |
|--------|---------|----------|--------|
| Amazon Associates | Commission attribution | P0 | **Unknown** — apply + verify 2026 rules |
| Vercel | Host catalog | P0 | Not deployed |
| Search Console | SEO indexing | P1 | After 3+ pages |
| Plausible / analytics | Traffic + outbound clicks | P1 | v0 |
| Reddit API | Discover + post | P2 | Blocked on karma + commercial ToS check |
| Quora | Assisted posting | P1 | VA manual v0 |
| Pinterest | Pins → catalog | P1 | VA manual v0 |
| Medium | Syndication | P1 | VA manual v0 |
| Local LLM | Draft answers/comps | P2 | Optional assist v0 |
| Supabase | Engine state | P2 | After v0 proof |
| Facebook Groups | High audience | P3 | VA warm project; no API |

## Security & compliance

- **Hard rule:** never post raw Amazon Associates URLs off-site — catalog bridge only
- FTC affiliate disclosure on every comp page and external post where required
- Associates **original content** posture: constraint analysis + comparison tables, not thin ASIN dumps
- No fake traffic, click fraud, or paid ads → Amazon (can disqualify commissions — verify live policy)
- Platform ToS: rate limits, disclosure, no spam; approve queue until ban rate known
- No automated mass account signup; VA/human for registration and warm-up
- Avoid health/medical/pressure-cooker safety claims in kitchen v0
- Price/spec data dated; refresh cadence documented

## Build phases

| Phase | Outcome | Rough effort |
|-------|---------|--------------|
| 0 — Graduate | Repo + plans + vertical slice doc | Done at graduation |
| 1 — Vertical slice | 5 comps live, venue queue, 3+ posts, P&L week 1 | 2–3 weeks |
| 2 — Fast eyes | Quora/Pinterest/Medium rhythm + SEO indexing | 2–4 weeks |
| 3 — Engine MVP | Reddit discover/draft + approve; demand-driven comps | 4–6 weeks |
| 4 — Portfolio | 50+ venues, P&L allocator, compare utility | 2–3 months |
| 5 — Expand | Email, 2nd rail, pets clone | After kitchen P&L proof |

## Risks & unknowns

| Risk | Impact | Mitigation |
|------|--------|------------|
| Associates approval denied or terminated | No revenue | Niche comps, disclosures, no thin affiliate; bridge-only links |
| 2026 qualifying-purchase / content rules | Commission loss | Original constraint comps; no ad-arbitrage |
| Reddit commercial API / ban rate | Engine scale blocked | Approve queue; fast eyes first; karma warm |
| SEO crowding (air fryer) | Slow organic | Long-tail constraints only |
| Recs without hands-on testing | Trust / credibility | Public methodology; “best for / skip if”; no false “we tested” |
| Venue spam classification | Account bans | Match-only posting; rate caps; catalog bridge |
| FB Groups API dead | No clean FB automation | VA parallel; not on v0 critical path |
| Unit economics unknown | Opex > commission | Weekly P&L kill rule; channel-level metering |
| ASIN/price drift | Stale pages | `updated_at` + refresh cadence |

## Open technical decisions

- Domain name (TBD) — register before Associates application
- MDX vs JSON comp schema (MDX preferred for SEO prose + components)
- Analytics vendor (Plausible vs self-hosted)
- When to add Supabase (after first commission signal vs at engine MVP)
- Reddit commercial API contract — spike before auto-post
- Local LLM model choice for draft quality vs cost
- Compare-tool UX scope (phase 4 vs earlier if conversion weak)

## Success metrics (018 only — not 007)

| Metric | v0 target |
|--------|-----------|
| Live comp pages | ≥ 5 |
| Venue posts live | ≥ 3 (manual/VA) |
| Catalog → Amazon outbound clicks | > 0 |
| Attributed commission | > $0 (any amount proves loop) |
| Weeks underwater | Kill rule TBD — suggest 4–6 weeks with zero commission after active posting |
