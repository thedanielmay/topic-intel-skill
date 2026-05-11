---
name: topic-intel
description: "Deep, multi-source intelligence research on any non-company topic. Use this skill whenever the user wants to research a market, industry, policy, regulation, standard, technology category, geographic community, trend, movement, academic field, or public figure. Triggers include: 'research [topic]', 'deep dive on [topic]', 'brief me on [topic]', 'what do we know about [topic]', 'landscape of [topic]', 'state of [topic]', 'market analysis of [topic]', 'intelligence on [topic]', 'look into [topic]', 'topic-intel: [topic]', or any request to understand a non-company subject in depth. Also triggers on: market research requests, policy analysis requests, regulatory landscape requests, technology comparison requests, community ecosystem mapping, trend analysis, and public figure background research. Do NOT use for research on a specific named company or organisation — use company-intel instead. When the subject could be either a company or a topic, show the disambiguation prompt."
---

# Topic Intelligence Research

## Overview

A structured intelligence-gathering skill that performs deep, multi-source research on any non-company topic. Produces a comprehensive dossier with provenance-tracked findings, organised into a navigable folder structure.

**Core principles:**
- Define scope before searching — never research blind
- Every claim needs a source and a confidence rating
- Triangulate across independent sources — one source is not enough
- Negative space is intelligence — what doesn't exist is as important as what does
- KITs (Key Intelligence Topics) drive everything — orient every phase toward answering them
- The goal is actionable intelligence, not just information

**Announce at start:** "Using the topic-intel skill to conduct a structured intelligence investigation."

**Reference files in `references/`:**
- `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md` — **Read this first at the start of every investigation.** Universal standards governing all intelligence work: STOP gate, scoping document, KIT Answer Register, confidence ratings, saturation rule, wave thresholds, progress reporting, synthesis sequence, universal deliverables, disambiguation routing, tool check, ethical boundaries.
- `~/.claude/skills/shared/references/web-access-layers.md` — Four-layer web access architecture. Read this at the start of every investigation.
- `~/.claude/skills/shared/references/layer-4-playwright-protocol.md` — Complete Layer 4 Playwright protocol. Read when Layer 4 is triggered.
- `references/topic-sources.md` — Source register by topic type (APIs, databases, access notes). (expanded — includes ~25 new sources and source effort caps per tier)
- `references/topic-outputs.md` — Output templates for each topic type.
- `references/topic-analytical-frameworks.md` — Analytical frameworks adapted for topic research.
- `topic-expert-panels.md` — Expert panel personas for Phase 11 synthesis QA. Seven module-specific expert sets with 3 integration modes: Synthesis Reviewer (default), On-Demand Consultant, Pre-Output QA.
- `topic-osint.md` — OSINT techniques scoped to topic research: advanced search operators, Wayback Machine advanced techniques, social media signal extraction (TRD/PRS), document metadata (SCI/POL).
- `topic-agent-dispatch.md` — Wave-level parallelisation guidance: agent briefing templates, handoff format, per-wave notes, timeout fallback, and tier effort caps.

---

## Disambiguation: topic-intel vs company-intel

Before starting, confirm the subject is a topic, not a company.

| Subject type | Skill to use |
|---|---|
| Named legal entity (company, charity, trust, government agency) | company-intel |
| Policy, framework, standard, regulation | topic-intel |
| Industry, market, or sector | topic-intel |
| Technology category or product class (not one specific company) | topic-intel |
| Geographic area or community | topic-intel |
| Trend, movement, or time-bounded phenomenon | topic-intel |
| Public figure (not a company they run) | topic-intel |
| Scientific or academic field | topic-intel |
| Ambiguous — could be company or topic | Show disambiguation prompt |

**Disambiguation prompt** (show when ambiguous):
```
Is this research about the organisation itself, or the broader topic?

  C — The organisation [Name] — company-intel (dossier, people, financials)
  T — The broader topic or landscape — topic-intel (market, policy, ecosystem)
  B — Both — company-intel first, then topic-intel for sector context

(C / T / B)
```

**If user selects C:** Exit this skill. Invoke the company-intel skill instead — it is purpose-built for named legal entities.
**If user selects T:** Continue with this skill.
**If user selects B:** Run company-intel first for the entity, then return to topic-intel for the sector context.

Full routing logic and natural hand-off points between skills are documented in `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md` Section 14.

---

## Invocation

### Menu-driven (no topic specified)

```
TOPIC INTELLIGENCE RESEARCH
───────────────────────────
1. New Investigation      — Research a topic from scratch
2. Resume Investigation   — Continue an existing topic dossier
3. Refresh Investigation  — Update a dossier with new data
4. Quick Scan             — Fast overview (~30 min equivalent)

Which would you like? (1–4)
```

### Refresh Investigation protocol (Menu option 3)

When a user selects Refresh Investigation:

1. Load `00-summary/topic-scoping.md` and `research-log.md` from the existing investigation folder
2. Identify what has changed since the last run:
   - Time elapsed (has the monitoring plan's refresh cadence been reached?)
   - Monitoring plan trigger events (have any triggers from `00-summary/monitoring-plan.md` fired?)
   - New KITs added by the user in this session
3. Determine refresh scope: which phases need re-running vs. which findings can be carried forward unchanged
4. Run only the affected phases — do not re-run the full investigation
5. Update these files: `kit-answers.md`, `critical-findings.md`, `rag-dashboard.md`, `monitoring-plan.md`
6. Append a refresh block to `research-log.md`:
   ```
   ## Refresh — [YYYY-MM-DD]
   What was refreshed: [phases re-run]
   What was carried forward: [phases not re-run and why]
   Material changes: [what changed vs. prior run]
   Next refresh: [date or trigger]
   ```
7. Do not regenerate the PDF unless findings have materially changed

If New Investigation selected and no topic given:
```
What topic would you like to research?

Examples:
  · A market or industry     ("Australian textile recycling market")
  · A policy or regulation   ("Victorian Social Procurement Framework")
  · A technology area        ("circular economy ERP software")
  · A geographic community   ("social enterprise ecosystem, inner Melbourne")
  · A trend or movement      ("impact investing in Australia, 2023–2025")
  · A public figure          ("name, role/context")
  · An academic/science area ("microplastics in marine environments")

Topic:
```

### Direct invocation (topic specified)

When a topic is named — whether explicit (`topic-intel: [topic]`) or implicit (`Research the Victorian Social Procurement Framework`) — skip the menu and go directly to the flow below.

**Skipping the menu does not skip Phase 0.** If topic is given but investigation depth and KITs are not specified, still perform Steps 2 and 3 of Phase 0 before any research begins.

---

## Phase 0: Intelligence Requirements Definition

**This is the most important phase. Do not skip it.**

### Step 1: Confirm topic and classify type

State the topic and its type:
```
TOPIC INTELLIGENCE RESEARCH
───────────────────────────
Topic:   [topic as stated]
Type:    [topic type — see table below]
```

**Topic type codes:**

| Code | Type | Examples |
|---|---|---|
| MKT | Market / industry | "Australian textile recycling market", "B2B stationery sector" |
| POL | Policy / regulatory | "Victorian Social Procurement Framework", "ISSB climate standards" |
| SCI | Academic / scientific | "microplastics in marine environments", "social impact measurement" |
| TEC | Technology / product | "circular economy ERP platforms", "AI tools for NFP fundraising" |
| GEO | Geographic / community | "social enterprise ecosystem in inner Melbourne" |
| TRD | Trend / event | "impact investing trends 2023–2025", "post-COVID supply chain shifts" |
| PRS | Person (public figure) | Research on a named public figure's career, views, record |

If type is ambiguous between two codes, ask the user to confirm before proceeding.

### Step 2: Offer investigation depth

```
INVESTIGATION DEPTH
───────────────────
1. Quick Scan          — Core sources, key facts, recent developments. ~30 min.
                         Produces: 1-page briefing note.
2. Standard            — All of Quick Scan + stakeholder map, data sources,
                         analysis, synthesis. ~2 hrs.
                         Produces: Multi-section dossier + executive summary.
3. Full Investigation  — (default) Everything. All sources, cross-reference,
                         expert QA review, PDF synthesis report. ~4–6 hrs.
                         Produces: Complete dossier + PDF.

Which depth? (1–3, default: 3)
```

### Step 3: KIT elicitation

KITs are the 2–5 questions you most need answered. Everything in the research is oriented toward answering them.

```
KEY INTELLIGENCE TOPICS
───────────────────────
What are the 2–5 questions you most need this research to answer?
These drive the entire investigation — the more specific, the better.

[Show examples relevant to the topic type — see Topic Type Modules below]

Your KITs (list them, one per line):
```

If user gives vague or no KITs, propose a set based on the topic type defaults (see each module) and ask for confirmation.

### Step 4: Topic Scoping Document

Before any research begins, propose and confirm this document:

```
TOPIC SCOPING DOCUMENT
──────────────────────
Topic:        [topic as refined]
Topic type:   [code and name]
Depth tier:   [1/2/3 — name]
Output:       [what will be produced]

Key Intelligence Topics:
  1. [KIT 1]
  2. [KIT 2]
  3. [KIT 3]
  [additional KITs if provided]

Scope inclusions:
  [Geographic boundaries, time range, sub-topics, source types to prioritise]

Scope exclusions:
  [What is explicitly out of scope — prevents drift]

Primary sources to target:
  [3–6 named source types most relevant to this topic type]

Output location:
  _topic-{slug}/

──────────────────────
Does this capture what you need? (yes / adjust scope / change KITs)
```

**Research does not begin until the user confirms this document.**

**STOP — do not run any searches, WebFetch calls, or tool checks until the user has confirmed this document.** The temptation to begin research while awaiting confirmation is a common failure mode. Resist it.

Save to: `_topic-{slug}/00-summary/topic-scoping.md`

---

## Web Access — Four-Layer Architecture

Read `~/.claude/skills/shared/references/web-access-layers.md` for the full protocol.

**Summary:**
- **Layer 1 — API-first:** Use public APIs where available (ABS, legislation.gov.au, Semantic Scholar, CrossRef, ACNC, etc.)
- **Layer 2 — Search as router:** Use WebSearch to find the exact URL, then fetch only that URL. Never crawl blind.
- **Layer 3 — Scraping service:** If WebFetch fails, route through ScrapingBee/ZenRows/BrightData if configured.
- **Layer 4 — Playwright:** Universal fallback for all other failures.

**Universal fallback rule:** If Layers 1, 2, and 3 have all failed or are inapplicable, AND the source contains interactive or JS-rendered content, ALWAYS attempt Layer 4 (Playwright) before declaring a source inaccessible. Read `~/.claude/skills/shared/references/layer-4-playwright-protocol.md` for the complete decision tree, execution templates, and source-logging format.

**Pre-investigation tool check** (run once at the start of every investigation):
```bash
/usr/bin/python3 -c "import playwright; print('playwright ok')" 2>/dev/null || echo "playwright NOT installed"
/usr/bin/python3 -c "from playwright_stealth import Stealth; print('stealth ok')" 2>/dev/null || echo "stealth NOT installed"
echo "ScrapingBee: ${SCRAPINGBEE_API_KEY:+configured}"
echo "BrightData:  ${BRIGHTDATA_API_KEY:+configured}"
echo "ZenRows:     ${ZENROWS_API_KEY:+configured}"
```

Run the check and record the actual results in `research-log.md`. Note any tools that are unavailable before proceeding.

See `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md` Section 15 for the standard tool check format and Python binary note.

---

## Confidence Rating System

See `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md` Section 4 for the full confidence rating system and source independence rule. Apply ★★★/★★/★/⚠ ratings to every piece of information recorded.

**Source independence matters.** Two sources citing the same original report = one source.

---

## Parallel Execution Strategy

Run phases in waves to maximise efficiency:

**Wave 1** (must complete first): Phase 0 — scope definition and tool check
**Wave 2** (parallel): Primary source identification + stakeholder/actor mapping + data and statistics search
**Wave 3** (after Wave 2): Media and community sources + policy/regulatory/technical depth + cross-reference
**Wave 4** (after all data): Framework analysis and synthesis
**Wave 5** (final): Expert QA review + deliverable generation

**Wave-level parallelisation:** Each wave can be executed using parallel agents rather than sequentially. When doing so:
- Include the confirmed scoping document, current KIT status, and research log in every agent briefing
- Use Explore agents for Waves 2-3 (source discovery), general-purpose agents for Waves 4-5 (analysis and synthesis)
- Apply tier effort caps: Tier 1 = Layer 2 max; Tier 2 = Layer 3 max; Tier 3 = Layer 4 allowed
- After each agent completes, it writes a structured block to `research-log.md` (see `topic-agent-dispatch.md`)
- Sequential execution remains valid — dispatch is a performance optimisation, not a requirement

See `topic-agent-dispatch.md` for full guidance: agent briefing templates, per-wave notes, handoff format, and timeout fallback. See `superpowers:dispatching-parallel-agents` skill for Claude Code implementation.

**Wave 2 minimum threshold:** If primary source identification returns fewer than 3 substantive sources for any KIT, pause and report to the user before proceeding to Wave 3. Do not proceed to synthesis on a thin evidence base.

See `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md` Section 6 for the full wave execution rules.

---

## Source Saturation Rule

See `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md` Section 5 for the source saturation rule. Module SCI's additional threshold (≥50% overlap) applies to academic searches and takes precedence over the global rule.

---

## Topic Type Modules

Each module defines the phase set, source priorities, KIT defaults, and output format for that topic type. Select the module matching the classified type and follow its guidance throughout the investigation.

---

### Module MKT — Market / Industry Research

**Examples:** "Australian textile recycling market", "B2B stationery sector in Victoria"

**Default KITs:**
1. Who are the key organisations and what are their market positions?
2. What is the size, value, and growth trajectory of the market?
3. What policy and regulatory drivers are shaping the market?
4. What revenue models are viable and which are financially sustainable?
5. What are the gaps, underserved segments, or emerging opportunities?

**Phase set:**

| Phase | Content | Tier |
|---|---|---|
| M1 | Market definition and sizing — anchor the scope with official statistics before any other research | All |
| M2 | Key player identification — who operates in this space, at what scale, with what model | All |
| M3 | Regulatory and policy environment — what laws, standards, or government programs shape this market | All |
| M4 | Revenue model analysis — how do organisations in this space make money | 2/3 |
| M5 | Competitive dynamics — market concentration, barriers to entry, substitutes | 2/3 |
| M6 | Stakeholder ecosystem — funders, intermediaries, advocacy bodies, regulators | 2/3 |
| M7 | Opportunity and gap analysis — what is underserved, emerging, or changing | 2/3 |
| M8 | Synthesis — market landscape brief, opportunity map, KIT answers | All |

**Primary sources:** ABS industry statistics, ACCC market studies, ACNC filings for NFP operators, IBISWorld (flag if paywalled), trade associations, GrantConnect/AusTender for government procurement, Trove for historical context, media.

**Output format:** Market landscape brief — size, segmentation, key players, regulatory environment, opportunity map, KIT answers.

**Expert panel:** See `topic-expert-panels.md` — MKT expert set (3 personas). Invoke in Mode 1 during Phase 11 synthesis; Mode 2 on-demand during research phases; Mode 3 pre-PDF QA.

---

### Module POL — Policy / Regulatory Research

**Examples:** "Victorian Social Procurement Framework", "ISSB climate reporting standards 2024–2026"

**Default KITs:**
1. What does this policy/framework require, and of whom?
2. What are the thresholds, timelines, or triggering conditions?
3. What are the enforcement mechanisms and consequences of non-compliance?
4. How is the policy being implemented in practice vs. on paper?
5. Who benefits and who bears the costs?

**Phase set:**

| Phase | Content | Tier |
|---|---|---|
| P1 | Legislative/regulatory instrument identification — find the primary legal instrument and confirm its current version | All |
| P2 | Source authority assessment — map the hierarchy: primary legislation → subordinate legislation → ministerial guidelines → agency guidance → practice notes | All |
| P3 | Scope and obligation analysis — who must comply, what must they do, by when | All |
| P4 | Implementation landscape — how is it being applied; audit reports, case decisions, guidance documents | 2/3 |
| P5 | Stakeholder positions — who lobbied for/against, who benefits, who bears costs; parliamentary submissions | 2/3 |
| P6 | Timeline reconstruction — when was it introduced, what has changed, what is upcoming | 2/3 |
| P7 | Practical implications — what this means for a specific actor or context (derive from KITs) | All |
| P8 | Synthesis — policy brief, compliance landscape, KIT answers | All |

**Primary sources:** legislation.vic.gov.au or legislation.gov.au (primary instrument), AustLII (all jurisdictions, regulations, tribunal decisions), Parliament Hansard (debates, committee hearings), Victorian Auditor-General / ANAO (implementation audits), consultation submissions, Social Traders / peak body commentary for sector-specific policy, Productivity Commission reports.

**Output format:** Policy brief — scope and purpose, key obligations, implementation status, compliance landscape, advocacy positions, gaps and ambiguities, practical implications.

**Expert panel:** See `topic-expert-panels.md` — POL expert set (3 personas). Invoke in Mode 1 during Phase 11 synthesis; Mode 2 on-demand during research phases; Mode 3 pre-PDF QA.

---

### Module SCI — Academic / Scientific Research

**Examples:** "textile recycling technology advances 2020–2026", "evidence base for work-integrated social enterprises"

**Default KITs:**
1. What is the current state of evidence/knowledge on this topic?
2. Where is the evidence strong and where is it contested or absent?
3. Who are the leading researchers and institutions?
4. What is the direction and momentum of recent research?
5. What practical applications emerge from current evidence?

**Phase set:**

| Phase | Content | Tier |
|---|---|---|
| S1 | Evidence landscape mapping — how much research exists, what databases cover it, what date range | All |
| S2 | Systematic literature search — date-bounded, keyword-controlled search of Semantic Scholar and CrossRef | All |
| S3 | Citation filtering — apply citation-count threshold appropriate to field maturity; identify most-cited and most-recent work | All |
| S4 | Key researcher and institution mapping — who is publishing, where, with what funding | 2/3 |
| S5 | Funding landscape — ARC grants database, NHMRC, international equivalents; what is government funding in this area | 2/3 |
| S6 | Contested claims register — identify areas of genuine expert disagreement; apply ACH | 2/3 |
| S7 | Practical application mapping — what emerging from the literature has real-world application | 2/3 |
| S8 | Synthesis — evidence landscape map, KIT answers, research gaps | All |

**Primary sources (access via API where possible):**
- Semantic Scholar API: `https://api.semanticscholar.org/graph/v1/paper/search?query={terms}&fields=title,authors,year,citationCount,abstract`
- CrossRef API: `https://api.crossref.org/works?query={terms}&sort=relevance&rows=20`
- Google Scholar (WebFetch — partial access, no API)
- ARC Grant Discovery: `https://dataportal.arc.gov.au/` (search by keyword)
- CSIRO publications: `https://www.csiro.au/en/research/publications`
- arXiv (for pre-prints in STEM): `https://arxiv.org/search/?query={terms}`

**Source saturation rule:** When new searches return references already in the literature register, stop searching — saturation reached. Do not expand scope to fill the word count.

**Output format:** Research brief — state of evidence, key findings with confidence levels, contested areas, major researchers, funding landscape, practical implications, identified gaps.

**Phase S2 — invoking `literature-review`:**
At Phase S2, invoke the `literature-review` skill to conduct the systematic search. The skill handles:
- Multi-database Boolean search (PubMed, arXiv, Semantic Scholar, Google Scholar, Web of Science, OpenAlex)
- Two-stage screening (title/abstract then full-text) with documented exclusion reasons
- PRISMA flow diagram generation (auditability artifact for Full Investigations)
- Study quality assessment: Cochrane Risk of Bias (RCTs), Newcastle-Ottawa Scale (observational), CASP (qualitative)

After the `literature-review` skill returns its findings, continue with S3 (citation filtering) and S4 (researcher mapping) in this skill. The saturation rule in `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md` Section 5 applies throughout.

**Fallback (if `literature-review` not installed):** Use the existing S2 guidance directly: Semantic Scholar API + CrossRef API + arXiv API as documented in `topic-sources.md` Module SCI section.

**Expert panel:** See `topic-expert-panels.md` — SCI expert set (3 personas). Invoke in Mode 1 during Phase 11 synthesis; Mode 2 on-demand during research phases; Mode 3 pre-PDF QA.

---

### Module TEC — Technology / Product Research

**Examples:** "circular economy ERP platforms", "AI tools for NFP fundraising"

**Default KITs:**
1. What platforms/solutions exist in this category?
2. How do they compare on capability, usability, and pricing?
3. Which are being adopted by organisations like ours, and with what outcomes?
4. What are the implementation and integration considerations?
5. What is the vendor landscape — which are stable vs. at risk?

**Phase set:**

| Phase | Content | Tier |
|---|---|---|
| T1 | Category definition — establish what is and is not in scope for this technology category | All |
| T2 | Vendor/solution landscape — identify all significant players via G2, Capterra, GitHub, search | All |
| T3 | Feature and capability comparison — build a comparison matrix from public sources | 2/3 |
| T4 | Pricing and commercial model analysis — published pricing, free tier, enterprise negotiation signals | 2/3 |
| T5 | Adoption evidence — case studies, customer reviews, Australian/sector-specific users | 2/3 |
| T6 | Vendor viability — funding status (Crunchbase), team size (LinkedIn), support quality (reviews) | 2/3 |
| T7 | Integration landscape — what does each solution connect to, what are the technical dependencies | 3 |
| T8 | Synthesis — vendor comparison matrix, recommendation matrix, KIT answers | All |

**Primary sources:** G2 / Capterra / Gartner Peer Insights, GitHub (OSS activity — stars, commits, issues), Crunchbase (funding and acquisition), vendor websites (feature lists, pricing pages, case studies), conference talks and product demos (YouTube), trade press, Australian user communities.

**Output format:** Technology landscape — capability map, vendor comparison matrix, maturity/viability assessment, pricing summary, adoption evidence, recommendation by use case.

**Expert panel:** See `topic-expert-panels.md` — TEC expert set (3 personas). Invoke in Mode 1 during Phase 11 synthesis; Mode 2 on-demand during research phases; Mode 3 pre-PDF QA.

---

### Module GEO — Geographic / Community Research

**Examples:** "social enterprise ecosystem in inner Melbourne", "disability services landscape in regional Victoria"

**Default KITs:**
1. Who are the significant organisations and connectors in this space?
2. What networks, intermediaries, and support infrastructure exist?
3. What funding and support is available for [type of organisation] in this geography?
4. What gaps or unmet needs exist?
5. How does [named organisation] position relative to peers in this ecosystem?

**Phase set:**

| Phase | Content | Tier |
|---|---|---|
| G1 | Geographic boundary definition — confirm the specific area, what is and is not included | All |
| G2 | Anchor institution mapping — major organisations, peak bodies, government bodies in the geography | All |
| G3 | Network and intermediary mapping — who connects whom; events, convening bodies, shared platforms | 2/3 |
| G4 | Funder landscape — philanthropy, government grants, community foundations with local focus | 2/3 |
| G5 | Needs and gaps analysis — what is underserved, what do community plans and reports identify | 2/3 |
| G6 | Comparative analysis — similar geographies for benchmarking and model identification | 3 |
| G7 | Synthesis — ecosystem map, stakeholder network, opportunity assessment, KIT answers | All |

**Primary sources:** ABS Census data (TableBuilder or pre-built profiles), Social Traders certified enterprise register, ACNC charity register with geographic filter, local government community plans and social procurement policies, Philanthropy Australia regional data, Buy Social Australia directory, inner Melbourne council websites, VCOSS and sector peak bodies, media with geographic filter.

**Output format:** Ecosystem map — institutional inventory, network relationships, funder landscape, gap analysis, opportunity assessment.

**Expert panel:** See `topic-expert-panels.md` — GEO expert set (3 personas). Invoke in Mode 1 during Phase 11 synthesis; Mode 2 on-demand during research phases; Mode 3 pre-PDF QA.

---

### Module TRD — Trend / Event Research

**Examples:** "corporate ESG reporting mandate changes 2024–2026", "impact investing in Australia 2020–2025"

**Default KITs:**
1. What drove this trend and how durable is it?
2. What has materially changed in the defined time period?
3. Who are the leaders, laggards, and emerging actors?
4. What is the regulatory/policy trajectory?
5. What does this mean for [specific context, derived from KITs]?

**Phase set:**

| Phase | Content | Tier |
|---|---|---|
| TR1 | Temporal anchoring — enforce the date scope throughout; define what "before" and "after" means for this trend | All |
| TR2 | Chronological reconstruction — build a timeline of key events, decisions, publications within the scope | All |
| TR3 | Driver analysis — what caused this trend; structural, cyclical, or event-driven | 2/3 |
| TR4 | Actor mapping — who is driving, resisting, adapting, or leading edge | 2/3 |
| TR5 | Regulatory/policy trajectory — what is legislated, consulted, or foreshadowed | 2/3 |
| TR6 | Forward-looking assessment — where is this going; scenario analysis if warranted | 2/3 |
| TR7 | Synthesis — trend brief, timeline table, implications, KIT answers | All |

**Primary sources:** Regulatory update feeds (ASIC, Treasury, relevant agencies), parliamentary announcements and Hansard, Big 4 firm publications (credible secondary commentary on regulatory trends), trade press over the defined period, academic analysis, Google Trends trajectory.

**Output format:** Trend brief — timeline table, driver analysis, actor map, regulatory trajectory, forward-looking assessment.

**Expert panel:** See `topic-expert-panels.md` — TRD expert set (3 personas). Invoke in Mode 1 during Phase 11 synthesis; Mode 2 on-demand during research phases; Mode 3 pre-PDF QA.

---

### Module PRS — Person Research (Public Figure)

**Examples:** "Laura Tingle, political journalist", "Andrew Forrest, philanthropist and businessman"

**Default KITs:**
1. What is their current role and institutional affiliations?
2. What are their publicly stated positions on [relevant topic]?
3. What is their professional track record and major contributions?
4. What networks and relationships are relevant to [our context]?
5. What is their public reputation — credibility, controversies, peer regard?

**Phase set:**

| Phase | Content | Tier |
|---|---|---|
| PR1 | Identity confirmation — confirm the correct person and distinguish from namesakes | All |
| PR2 | Career reconstruction — full professional history from public sources | All |
| PR3 | Public positions and views — stated positions on relevant topics; speeches, writing, interviews | 2/3 |
| PR4 | Network and relationship mapping — institutional affiliations, board positions, known associates | 2/3 |
| PR5 | Public record — awards, publications, media coverage, controversies | 2/3 |
| PR6 | Credibility and reputation assessment — peer regard, critical coverage, professional standing | 3 |
| PR7 | Synthesis — biographical intelligence brief, KIT answers | All |

**Primary sources:** LinkedIn (Layer 4 required — see layer-4-playwright-protocol.md), organisational websites, media archives (filtered search), published works and interviews, Wikipedia as reference map only (not primary source), AEC political donations register (for public figures in political life), AustLII (court records if relevant — public record only).

**Ethical boundaries:** Research covers only the person's PUBLIC role — career, public statements, professional record, institutional affiliations. Private life is out of scope. Privacy Act constraints apply throughout. Flag clearly if any source is self-reported (★ rating).

**Output format:** Biographical intelligence brief — career reconstruction, public positions on relevant topics, network map, professional reputation assessment, practical relevance to [context].

**Expert panel:** See `topic-expert-panels.md` — PRS expert set (3 personas). Invoke in Mode 1 during Phase 11 synthesis; Mode 2 on-demand during research phases; Mode 3 pre-PDF QA.

---

## Multi-Module Chaining Protocol

Some investigations span multiple topic types. Use this protocol when the Topic Scoping Document KITs cannot be answered by a single module.

**Examples:** "Impact investing in Australia 2020-2025" spans TRD (trend) + MKT (market) + POL (regulatory environment). "Social enterprise ecosystem in inner Melbourne" spans GEO (ecosystem) + MKT (revenue models) + POL (procurement policy).

**Rules:**
1. **Identify the primary module** — the one that best matches the user's primary KITs. This module drives the investigation structure and produces the primary deliverable.
2. **Identify secondary modules** — list only the modules needed to answer KITs that fall outside the primary type.
3. **Capture in the scoping document** — list all modules and their KIT assignments in the Topic Scoping Document before research begins. Format: `Primary module: TRD (KITs 1, 2, 4) | Secondary: MKT (KIT 3) | Secondary: POL (KIT 5)`
4. **Sequence:** Primary module phases run first. Secondary modules run after the primary investigation phase is complete, targeting only the phases relevant to their assigned KITs.
5. **Output integration:** Secondary module findings feed into the primary module's synthesis phase. Produce one unified Executive Summary; produce module-specific deliverables (e.g., Policy Brief + Trend Brief) as separate files in `00-summary/`.
6. **Limit:** Maximum 3 modules per investigation. More than 3 indicates the scope needs decomposition into separate investigations with their own scoping documents, folders, and KIT registers.

---

## Phase 11: Synthesis & Expert Review

**Goal:** Transform research into actionable intelligence. Every KIT must be explicitly answered.

### Step 1: KIT Answer Register

For each KIT, produce a structured answer:
```
KIT [N]: [question]
Answer: [direct answer — 2-5 sentences]
Confidence: [★★★/★★/★]
Key sources: [2-3 most authoritative sources used]
Gaps: [what would improve confidence if found]
```

**If KIT answers point in contradictory directions** (e.g. market is large but barriers are prohibitive, or policy is clear but implementation is absent), surface this tension explicitly in the Executive Summary. Do not resolve it artificially or weight one KIT's answer over another. Contradictory KIT answers are often the most important finding.

See `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md` Section 3 for the full KIT Answer Register format and KIT conflict rule.

### Step 2: Triangulation

Build a Triangulation Matrix per `topic-analytical-frameworks.md` — use the template and source type codes defined there. Apply to all key findings. The source type codes (PR/SI/SD/SR) and universal triangulation matrix format are also documented in `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md` Section 7.

### Step 3: Contested Claims

For any topic where genuine expert disagreement exists, produce a Contested Claims Register:
```
Claim: [the contested statement]
Position A: [who holds it, evidence, confidence]
Position B: [who holds it, evidence, confidence]
ACH verdict: [which is more consistent with all available evidence, or genuinely unresolvable]
```

Do NOT rate one side ★★★ and move on. Surface the disagreement.

**Invoking `fact-check-workflow`:** When a contested claim is identified, invoke the `fact-check-workflow` skill for structured verification. The skill provides:
- Evidence strength tiers (Strong / Medium / Weak) that map to this skill's confidence rating system (★★★ / ★★ / ★)
- A Rating Decision Template that documents supporting evidence, contradicting evidence, missing context, and confidence level — use this as the structured record for each ACH verdict
- A graduated 6-point rating scale (True / Mostly True / Half True / Mostly False / False / Pants on Fire) for use when the contested claim will be cited in a published deliverable
- Right-of-response protocol: when the contested claim involves a living public figure (PRS module only), the `fact-check-workflow` source contact template applies

**Fallback (if `fact-check-workflow` not installed):** Use the ACH template in `topic-analytical-frameworks.md` directly.

### Step 4: Critical Findings

List the 5–7 most decision-relevant findings. For each:
- The finding (one sentence)
- The evidence (sources, confidence)
- The "So What?" (direct implication for the user's purpose)

### Step 5: Information Gaps

List every significant gap in the intelligence picture. For each gap:
- What is missing
- Why it matters (which KIT it affects)
- What source would resolve it
- Whether it is obtainable (with manual action note if so)

Gaps are intelligence. Do not minimise them.

### Step 6a: Expert Panel QA

Before the Confirmation Bias Check, route the draft synthesis through the embedded expert personas for this topic type.

**Mode 1 — Synthesis Reviewer (always invoke for Full Investigations):**
1. Read `topic-expert-panels.md` and select the expert set matching the topic module
2. Select the 2-3 experts most relevant to the primary KITs
3. For each expert, apply their key questions to the draft synthesis and flag any gaps as `[QA FLAG: Expert Name — issue]`
4. Resolve all `[QA FLAG]` annotations before proceeding to deliverable generation

**Mode 3 — Pre-Output QA (invoke before PDF generation):**
Run all three experts in the module set across the complete dossier. Any unresolved `[QA FLAG]` blocks the PDF generation step.

**On-demand adversarial review:** If the user has requested a full expert panel challenge, invoke the `expert-panel` skill for an interactive multi-round panel session using the personas from `topic-expert-panels.md` as the starting point for expert selection.

**Fallback (if `expert-panel` skill not installed for on-demand):** Run the Mode 1 embedded review only using the personas from `topic-expert-panels.md` — no dependency on external skill.

See `topic-expert-panels.md` for all 7 module expert sets and full integration mode definitions.

### Step 6: Confirmation Bias Check

Before finalising, apply the Devil's Advocate check:
- What evidence contradicts the central narrative?
- List at least 3 findings that cut against the primary thesis
- Have competing hypotheses been considered for ambiguous findings?

### Step 7: Monitoring Plan

For each investigation, produce a monitoring plan:
- Google Alerts terms to set up
- Key publication feeds or regulatory update subscriptions
- Trigger events that should prompt a refresh
- Recommended refresh cadence (monthly / quarterly / on-trigger)

### Step 8: Deliverable

The following are the universal deliverables required for all Full Investigations (see `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md` Section 9 for templates):

**Always produce:**
- KIT Answer Register
- Critical Findings
- Executive Summary (purpose-oriented, 1–2 pages)
- Triangulation Matrix
- Contested Claims Register (if applicable)
- Information Gaps
- Monitoring Plan
- Assumptions and Limitations
- RAG Dashboard (per `topic-outputs.md` Section 3 — use the template matching the topic type)

**Produce based on topic type:**
- MKT: Market landscape brief + opportunity map
- POL: Policy brief + compliance timeline
- SCI: Evidence landscape map + research gap analysis
- TEC: Vendor comparison matrix + recommendation table
- GEO: Ecosystem map + stakeholder network
- TRD: Trend timeline table + forward-looking assessment
- PRS: Biographical intelligence brief

**PDF:** For Full Investigation (Tier 3), compile all outputs into a PDF at the root of the topic folder, named `full-synthesis-report-YYYY-MM-DD-HH:MM.pdf`.

**Generating the PDF (Full Investigation — Tier 3):**
Once all `00-summary/` files are written and all `[QA FLAG]` annotations from Mode 3 expert QA are resolved, invoke `document-skills:pdf` to compile the full synthesis report:
- Input: all files in `00-summary/` plus any type-specific deliverable files
- Output: `full-synthesis-report-YYYY-MM-DD-HH:MM.pdf` at the root of the `_topic-{slug}/` folder
- Naming: use 24-hour time at moment of generation

**Fallback (if `document-skills:pdf` not installed):** Note in research log; deliver markdown files only. Add to `inaccessible-sources.md`: "PDF generation — document-skills:pdf not installed."

**Generating visualisations (MKT, TEC, TRD, GEO modules only):**
After producing markdown tables for comparison matrices, trend timelines, opportunity maps, or ecosystem diagrams, invoke `data-visualization` to produce visual outputs. Invoke once per deliverable type that contains tabular data:
- MKT: opportunity map, vendor/player comparison table
- TEC: vendor comparison matrix, capability map
- TRD: trend timeline, driver analysis table
- GEO: ecosystem map, funder landscape table

Do NOT invoke `data-visualization` for POL, SCI, or PRS modules — tables and text are more appropriate for legal, academic, and biographical outputs.

**Fallback (if `data-visualization` not installed):** Deliver markdown tables only.

---

## Progress Reporting

After completing each wave, post a wave summary before beginning the next wave — this is required, not optional. See `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md` Section 6 for the standard wave summary format and Wave 2 minimum threshold rules.

---

## Folder Structure

**Naming:** Always use `_topic-{slug}/` format, where `{slug}` is the topic lowercased, spaces replaced by hyphens (e.g., `_topic-victorian-social-procurement-framework`, `_topic-australian-textile-recycling-market`).

```
_topic-{slug}/
├── full-synthesis-report-YYYY-MM-DD-HH:MM.pdf   ← Final deliverable (Tier 3 only)
├── 00-summary/
│   ├── topic-scoping.md                          ← Phase 0 output — confirmed before research
│   ├── kit-answers.md                            ← KIT Answer Register
│   ├── critical-findings.md
│   ├── executive-summary.md
│   ├── triangulation-matrix.md
│   ├── contested-claims.md                       ← if applicable
│   ├── information-gaps.md
│   ├── monitoring-plan.md
│   └── assumptions-limitations.md
├── 01-primary-sources/                           ← Official documents, legislation, data
├── 02-media-coverage/                            ← News, interviews, podcasts, video
├── 03-stakeholders-actors/                       ← People, organisations, networks
├── 04-data-statistics/                           ← Quantitative data, datasets
├── 05-analysis/                                  ← Frameworks, models, synthesis drafts
├── inaccessible-sources.md                       ← Sources that failed all four layers
└── research-log.md                               ← Running log of all research activity
```

Every subfolder gets an `INDEX.md` cataloguing its contents with source, date, confidence rating, and summary.

---

## Research Log Format

Append to `research-log.md` throughout the investigation. See `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md` Section 13 for the standard research log format.

---

## Key Principles

1. **Scope before search.** The Topic Scoping Document is confirmed before any research begins.
2. **KITs drive everything.** Every phase asks: what does this tell us about each KIT?
3. **Provenance on everything.** Every fact cites its source with a confidence rating.
4. **Source independence.** Two organisations citing the same government report = one source.
5. **Gaps are intelligence.** Interpret every gap — don't just list it.
6. **Contested knowledge.** Where genuine expert disagreement exists, surface it — don't resolve it artificially.
7. **Universal Layer 4 fallback.** If Layers 1–3 fail, always try Playwright before declaring a source inaccessible.
8. **Ethical boundaries.** All intelligence from publicly accessible sources only. Privacy Act applies. Person research covers public role only.
9. **Actionable outputs.** The reader should know what to DO, not just what to THINK.
10. **Single source of truth.** Standards shared with company-intel (STOP gate, KIT register, saturation rule, wave thresholds, progress reporting, synthesis sequence, deliverables, disambiguation) are defined in `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md`. Do not duplicate them here — reference them.
