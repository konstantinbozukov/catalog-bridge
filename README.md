# Catalog Bridge

Kitchen constraint-comparison catalog + (later) Answer Insertion Engine for Amazon Associates commissions.

**Hub idea:** `018` — [tech-business-ideas](https://github.com/konstantinbozukov/tech-business-ideas)  
**Status:** v0 vertical slice in progress  
**Graduated:** 2026-08-10

## What this is

Two coupled products:

1. **Catalog** — constraint-based comp pages (“best air fryer for small kitchen under $100”) with Associates links **on our site only**
2. **Answer Insertion Engine** (phase 2+) — find purchase-intent questions in existing communities → helpful answer → catalog bridge link → meter P&L

**Hard rule:** external posts never use raw Amazon affiliate URLs — always point to our comp pages.

## v0 vertical slice

See [VERTICAL_SLICE.md](./VERTICAL_SLICE.md):

- 5 kitchen comp pages live
- Venue Queue v0 (~20 targets)
- 3+ VA-assisted posts (Quora / Pinterest / Medium)
- Outbound click tracking + week 1 P&L

## Docs

| Doc | Purpose |
|-----|---------|
| [TECH_PLAN.md](./TECH_PLAN.md) | Architecture, stack, phases |
| [BUSINESS_CASE.md](./BUSINESS_CASE.md) | Market, risks, pursue decision |
| [VERTICAL_SLICE.md](./VERTICAL_SLICE.md) | First proof path |
| [docs/comp-methodology.md](./docs/comp-methodology.md) | How we pick and present products |

## Repo layout

```
apps/web/           # Next.js site (phase 1)
content/comps/      # Comp pages (MDX)
data/               # Venue queue, P&L templates
docs/               # VA playbooks, methodology
```

## Beachhead niche

Kitchen use-case appliances & tools — constraint comps only. Pets gear is niche #2 after kitchen P&L proof.

## Related

- **007 Production Factory** — may dogfood tooling; **018 success = affiliate P&L only**
