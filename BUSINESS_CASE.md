# Business Case & Market Research — Catalog Bridge

**Work name:** Catalog Bridge  
**Hub idea:** `018` Amazon Affiliate Catalog  
**Status:** Graduated 2026-08-10  
**Product repo:** `E:\AI\catalog-bridge`

## Executive summary

Affiliate operators need **attributable purchase traffic** without building a follower graph from zero. **Catalog Bridge** pairs a **kitchen constraint-comparison catalog** (Amazon Associates rail #1) with an **Answer Insertion Engine** that places helpful recommendations into existing communities — always via our catalog, never naked affiliate links. **Beachhead:** kitchen use-case appliances & tools. **Buyer:** there is no SaaS buyer; revenue = commissions. **Operator:** founder + VA for account warm/posting. **Decision: pursue** — graduate to product repo and prove P&L on a narrow vertical slice before scaling venues or niches.

## Problem & opportunity

**Pain:** Curated Amazon affiliate catalogs fail when (a) pages are thin ASIN dumps, (b) traffic depends on cold social accounts, or (c) there is no repeatable path from “someone asked a question” → “our useful answer” → “attributed purchase.”

**Frequency:** Kitchen gadget purchase questions recur daily across Quora, Reddit, Pinterest, and search (“best air fryer for small kitchen under $100”).

**Cost of status quo:** Manual answering doesn’t scale; spam automation burns accounts and Associates standing; generic “top 10” SEO pages don’t convert in crowded niches.

**Who feels it most:** Operator trying to prove commission ≥ opex; shoppers with real constraints (small kitchen, budget cap, beginner meal prep).

## Solution

Two coupled products:

1. **Catalog** — constraint-based comp pages with comparison tables, methodology, disclosures, Associates links on-site only.  
2. **Answer Insertion Engine (later)** — discover purchase intent → match comp → draft → gate → post → meter P&L. Few adapters × 100+ venues.

**Tech wedge:** Precision insertion + catalog utility (not spray-and-pray); P&L-as-product with kill/double-down allocator; catalog bridge compliance posture.

## Target market

### Beachhead

**Kitchen problem appliances & tools** — constraint comps (small kitchen, 1–2 person, under $X, dorm, beginners). Channels with existing traffic: Quora, Pinterest, Medium (fast eyes); SEO + Reddit (automation scale). Commission band ~4.5% Kitchen on typical $25–$100 AOV.

**Next niche (after proof):** Pet problem gear (non-medical) — higher venue count, more claims risk.

### Broader market (TAM / SAM / SOM — rough hypotheses)

| Layer | Estimate | Method / sources |
|-------|----------|------------------|
| TAM | US affiliate marketing (broad) | **Hypothesis** — industry reports cite $10B+ US affiliate; not kitchen-specific |
| SAM | Product-review / comparison affiliate sites + social insertion | **Hypothesis** — CartComps-style + Reddit/Quora answer economy |
| SOM (near-term) | Kitchen constraint long-tail via 20–100 venues | **Hypothesis** — trickle portfolio model; no live EPC yet |

**Note:** No validated EPC, conversion rate, or commission forecast — v0 exists to measure these.

## Customer & buyer

| | |
|--|--|
| **Revenue** | Amazon Associates commissions (later: other rails) |
| **“Customer”** | Shoppers who click through from search or community answers |
| **Operator** | Founder — P&L owner |
| **VA budget** | Account warm + assisted posting (Quora, Pinterest, Medium) |
| **WTP signal** | Founder initiated; strategy + feasibility research complete |
| **Success** | Sustained window where weekly commissions ≥ weekly opex |

**007 separation:** Factory autonomy metrics do not gate 018; cross-learn tooling only.

## Competition & alternatives

| Player / alternative | What they do | Gap vs us (hypothesis) |
|----------------------|--------------|------------------------|
| **CartComps** | Curated comparison catalog | We add venue insertion engine + constraint niche focus |
| **Wirecutter / major review sites** | Hands-on reviews, brand authority | We don’t compete on brand; long-tail constraints + community insertion |
| **Thin affiliate blogs** | Generic “best X” lists | Crowded SEO; we target constraint pages + existing-audience insertion |
| **Manual Reddit/Quora answers** | Helpful individuals | Doesn’t scale; no P&L metering |
| **Spam affiliate bots** | Low-quality link dumps | Burns accounts; we use match-only + catalog bridge |
| **Status quo (don’t build)** | No commission stream | No learning on unit economics |

*Competitive EPC and traffic estimates not validated — kitchen air fryer space is known-crowded in SEO.*

## Differentiation & moat (hypotheses)

- **Few adapters × 100+ venues** — portfolio trickle, not one mega-channel
- **Demand-driven comps** — community asks feed which pages we build
- **Catalog bridge only** — compliance + analytics hop + multi-rail ready
- **Constraint utility** (phase 4) — compare by budget/space/household, not thin lists
- **P&L capital allocator** — ruthless kill on underwater channels/venues
- **Local writer + thin orchestrator** — low marginal content cost

**Moat if it works:** Venue allowlist + comp library + posting reputation + P&L history — **hypothesis**, unproven.

## Business model

**Revenue:** Commission on qualifying Amazon purchases (Kitchen ~4.5% band — verify live fee table).

**Costs (weekly opex lines):**
- Domain + hosting
- VA hours (account warm, Quora/Pinterest/Medium post)
- Compute (local LLM electricity; thin cloud orchestrator later)
- Optional analytics

**Unit economics (hypothesis — must measure):**

```
EPC = commission / outbound Amazon clicks
Break-even clicks/week ≈ weekly_opex / EPC
```

Example only: if opex = $50/week and EPC = $0.50, need ~100 outbound clicks/week — **not validated**.

**Later:** second merchant rail when Amazon EPC weak; email list; embeds.

## Go-to-market (v0)

**First 5 comp pages:**
1. Air fryer — small kitchen under $100  
2. Air fryer — 1–2 people  
3. Compact electric kettle — dorm/small apartment  
4. Vegetable chopper — beginners  
5. Milk frother — under ~$25  

**First venues (~20 in queue):** Quora question clusters, Pinterest keyword sets, Medium tags — mapped 1:1 to comp pages.

**Posting rule:** Only when comp page exactly matches the ask.

**Dual track:**
- **Fast eyes (days–weeks):** Quora + Pinterest + Medium on top of comps  
- **Scale automation (weeks–months):** SEO indexing + Reddit after account warm  

**Kill rule (proposed):** 4–6 weeks of active posting with zero attributed commission → pause channel or revisit niche/pages.

## Risks

| Risk | Mitigation |
|------|------------|
| Associates account termination | Original constraint comps; disclosures; no thin dumps |
| Platform bans (Reddit, Quora, etc.) | Approve queue; rate limits; value-first answers |
| SEO doesn’t rank early | Community insertion for first clicks; long-tail only |
| Crowded air fryer niche | Constraint specificity; demand-driven expansion |
| Credibility without lab testing | Public methodology; spec + review consensus |
| FB Groups gating | Defer until VA warm; not v0 critical path |
| Over-building engine before proof | Vertical slice locks scope |
| Mixing 007 and 018 metrics | Explicit separation in ops reviews |

## Decision

**Pursue** — graduate to `E:\AI\catalog-bridge`. Prove catalog → click → commission with 5 comps and assisted venue posts before scaling insertion engine or venue portfolio.

**Not pursuing (v0):** paid ads → Amazon, unlimited ASIN catalog, pets niche, multi-rail, full automation.

## Sources

- Hub brief: `ideas/018-amazon-affiliate-catalog/README.md`
- Strategy notes: `ideas/018-amazon-affiliate-catalog/notes.md` (2026-08-10)
- [Amazon Associates compensation](https://affiliate-program.amazon.com/gp/associates/join/compensation.html) — verify live rates
- CartComps reference: https://www.cartcomps.com/
- Meta Groups API deprecation (~April 2024) — FB automation limits
