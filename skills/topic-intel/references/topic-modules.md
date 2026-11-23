# Topic Type Modules and Multi-Module Chaining

This file holds the detailed specification for each of the seven topic type modules.
It also holds the protocol for investigations that span more than one module.
The parent SKILL.md selects the module after the topic type is classified.

## Topic Type Modules

Each module defines the phase set, source priorities, KIT defaults, and output format for that topic type. Select the module matching the classified type and follow its guidance throughout the investigation.

---

### Module MKT - Market / Industry Research

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
| M1 | Market definition and sizing - anchor the scope with official statistics before any other research | All |
| M2 | Key player identification - who operates in this space, at what scale, with what model | All |
| M3 | Regulatory and policy environment - what laws, standards, or government programs shape this market | All |
| M4 | Revenue model analysis - how do organisations in this space make money | 2/3 |
| M5 | Competitive dynamics - market concentration, barriers to entry, substitutes | 2/3 |
| M6 | Stakeholder ecosystem - funders, intermediaries, advocacy bodies, regulators | 2/3 |
| M7 | Opportunity and gap analysis - what is underserved, emerging, or changing | 2/3 |
| M8 | Synthesis - market landscape brief, opportunity map, KIT answers | All |

**Primary sources:** ABS industry statistics, ACCC market studies, ACNC filings for NFP operators, IBISWorld (flag if paywalled), trade associations, GrantConnect/AusTender for government procurement, Trove for historical context, media.

**Output format:** Market landscape brief - size, segmentation, key players, regulatory environment, opportunity map, KIT answers.

**Expert panel:** See `../topic-expert-panels.md` - MKT expert set (3 personas). Invoke in Mode 1 during Phase 11 synthesis; Mode 2 on-demand during research phases; Mode 3 pre-PDF QA.

---

### Module POL - Policy / Regulatory Research

**Examples:** "Victorian Social Procurement Framework", "ISSB climate reporting standards 2024-2026"

**Default KITs:**
1. What does this policy/framework require, and of whom?
2. What are the thresholds, timelines, or triggering conditions?
3. What are the enforcement mechanisms and consequences of non-compliance?
4. How is the policy being implemented in practice vs. on paper?
5. Who benefits and who bears the costs?

**Phase set:**

| Phase | Content | Tier |
|---|---|---|
| P1 | Legislative/regulatory instrument identification - find the primary legal instrument and confirm its current version | All |
| P2 | Source authority assessment - map the hierarchy: primary legislation → subordinate legislation → ministerial guidelines → agency guidance → practice notes | All |
| P3 | Scope and obligation analysis - who must comply, what must they do, by when | All |
| P4 | Implementation landscape - how is it being applied; audit reports, case decisions, guidance documents | 2/3 |
| P5 | Stakeholder positions - who lobbied for/against, who benefits, who bears costs; parliamentary submissions | 2/3 |
| P6 | Timeline reconstruction - when was it introduced, what has changed, what is upcoming | 2/3 |
| P7 | Practical implications - what this means for a specific actor or context (derive from KITs) | All |
| P8 | Synthesis - policy brief, compliance landscape, KIT answers | All |

**POL Verbatim Provision Rule (enforced at P8 synthesis):**

Statutory provisions, thresholds, obligations, penalties, and deadlines are verbatim content. Paraphrasing a legislative provision without quoting it introduces error - the most common form being precision drift (the real threshold is $1 million; the synthesis says $1.5 million).

**Rules for P8 synthesis prose:**
- Every operative provision, threshold figure, penalty, or compliance deadline must be quoted verbatim with its section reference (e.g., `s. 47(2)(b)`) from the primary legislative instrument accessed in Phase P1
- Summary and contextual prose around the provision is permitted - but the operative element must appear in direct quotes
- When citing from subordinate legislation, ministerial guidance, or agency guidance (lower in the source authority hierarchy), the quote must be accompanied by its authority level: `[Ministerial Guideline - not primary legislation]`
- Mark any provision described from memory or secondary sources (not directly accessed) as `[PROVISION UNVERIFIED - secondary source only]` and access the primary instrument before finalising

**Common POL paraphrase hallucinations to watch for:**
- Threshold figures rounded or shifted (e.g., "$100,000" → "$100K" → "$1M" across successive paraphrases)
- Obligation scope narrowed or widened ("entities above threshold" → "all entities")
- Deadline or commencement date shifted
- Penalty stated as maximum when the source states a range

**Primary sources:** legislation.vic.gov.au or legislation.gov.au (primary instrument), AustLII (all jurisdictions, regulations, tribunal decisions), Parliament Hansard (debates, committee hearings), Victorian Auditor-General / ANAO (implementation audits), consultation submissions, Social Traders / peak body commentary for sector-specific policy, Productivity Commission reports.

**Output format:** Policy brief - scope and purpose, key obligations, implementation status, compliance landscape, advocacy positions, gaps and ambiguities, practical implications.

**Expert panel:** See `../topic-expert-panels.md` - POL expert set (3 personas). Invoke in Mode 1 during Phase 11 synthesis; Mode 2 on-demand during research phases; Mode 3 pre-PDF QA.

---

### Module SCI - Academic / Scientific Research

**Examples:** "textile recycling technology advances 2020-2026", "evidence base for work-integrated social enterprises"

**Default KITs:**
1. What is the current state of evidence/knowledge on this topic?
2. Where is the evidence strong and where is it contested or absent?
3. Who are the leading researchers and institutions?
4. What is the direction and momentum of recent research?
5. What practical applications emerge from current evidence?

**Phase set:**

| Phase | Content | Tier |
|---|---|---|
| S1 | Evidence landscape mapping - how much research exists, what databases cover it, what date range | All |
| S2 | Systematic literature search - date-bounded, keyword-controlled search of Semantic Scholar and CrossRef | All |
| S3 | Citation filtering - apply citation-count threshold appropriate to field maturity; identify most-cited and most-recent work | All |
| S4 | Key researcher and institution mapping - who is publishing, where, with what funding | 2/3 |
| S5 | Funding landscape - ARC grants database, NHMRC, international equivalents; what is government funding in this area | 2/3 |
| S6 | Contested claims register - identify areas of genuine expert disagreement; apply ACH | 2/3 |
| S7 | Practical application mapping - what emerging from the literature has real-world application | 2/3 |
| S8 | Synthesis - evidence landscape map, KIT answers, research gaps | All |

**SCI Citation Manifest (required before S8 synthesis):**

Before writing the evidence landscape map or any synthesis prose in S8, produce a citation manifest. Every academic work to be cited in synthesis must appear in this manifest:

```markdown
| Citation | DOI / URL / Semantic Scholar ID | Source of record | Verified? |
|---|---|---|---|
| Author et al. (Year). Title. Journal. | [identifier] | [API response / research log entry] | Y / N |
```

**Rule:** Any citation that cannot be given a DOI, URL, or Semantic Scholar paper ID from the research log (from Phases S1-S7) must not appear in synthesis as a citation. It can appear as a gap: "A systematic review of [topic] was not located - this is a gap in the evidence base." A citation described only as "Author et al. (year)" with no traceable identifier is not a citation - it is a fabrication risk.

Save the manifest to `05-analysis/sci-citation-manifest.md` before beginning S8 prose.

**Primary sources (access via API where possible):**
- Semantic Scholar API: `https://api.semanticscholar.org/graph/v1/paper/search?query={terms}&fields=title,authors,year,citationCount,abstract`
- CrossRef API: `https://api.crossref.org/works?query={terms}&sort=relevance&rows=20`
- Google Scholar (WebFetch - partial access, no API)
- ARC Grant Discovery: `https://dataportal.arc.gov.au/` (search by keyword)
- CSIRO publications: `https://www.csiro.au/en/research/publications`
- arXiv (for pre-prints in STEM): `https://arxiv.org/search/?query={terms}`

**Source saturation rule:** When new searches return references already in the literature register, stop searching - saturation reached. Do not expand scope to fill the word count.

**Output format:** Research brief - state of evidence, key findings with confidence levels, contested areas, major researchers, funding landscape, practical implications, identified gaps.

**Phase S2 - invoking `literature-review`:**
At Phase S2, invoke the `literature-review` skill to conduct the systematic search. The skill handles:
- Multi-database Boolean search (PubMed, arXiv, Semantic Scholar, Google Scholar, Web of Science, OpenAlex)
- Two-stage screening (title/abstract then full-text) with documented exclusion reasons
- PRISMA flow diagram generation (auditability artifact for Full Investigations)
- Study quality assessment: Cochrane Risk of Bias (RCTs), Newcastle-Ottawa Scale (observational), CASP (qualitative)

After the `literature-review` skill returns its findings, continue with S3 (citation filtering) and S4 (researcher mapping) in this skill. The saturation rule in `references/INTELLIGENCE-STANDARDS.md` Section 5 applies throughout.

**Fallback (if `literature-review` not installed):** Use the existing S2 guidance directly: Semantic Scholar API + CrossRef API + arXiv API as documented in `topic-sources.md` Module SCI section.

**Expert panel:** See `../topic-expert-panels.md` - SCI expert set (3 personas). Invoke in Mode 1 during Phase 11 synthesis; Mode 2 on-demand during research phases; Mode 3 pre-PDF QA.

---

### Module TEC - Technology / Product Research

**Examples:** "circular economy ERP platforms", "AI tools for NFP fundraising"

**Default KITs:**
1. What platforms/solutions exist in this category?
2. How do they compare on capability, usability, and pricing?
3. Which are being adopted by organisations like ours, and with what outcomes?
4. What are the implementation and integration considerations?
5. What is the vendor landscape - which are stable vs. at risk?

**Phase set:**

| Phase | Content | Tier |
|---|---|---|
| T1 | Category definition - establish what is and is not in scope for this technology category | All |
| T2 | Vendor/solution landscape - identify all significant players via G2, Capterra, GitHub, search | All |
| T3 | Feature and capability comparison - build a comparison matrix from public sources | 2/3 |
| T4 | Pricing and commercial model analysis - published pricing, free tier, enterprise negotiation signals | 2/3 |
| T5 | Adoption evidence - case studies, customer reviews, Australian/sector-specific users | 2/3 |
| T6 | Vendor viability - funding status (Crunchbase), team size (LinkedIn), support quality (reviews) | 2/3 |
| T7 | Integration landscape - what does each solution connect to, what are the technical dependencies | 3 |
| T8 | Synthesis - vendor comparison matrix, recommendation matrix, KIT answers | All |

**Primary sources:** G2 / Capterra / Gartner Peer Insights, GitHub (OSS activity - stars, commits, issues), Crunchbase (funding and acquisition), vendor websites (feature lists, pricing pages, case studies), conference talks and product demos (YouTube), trade press, Australian user communities.

**Output format:** Technology landscape - capability map, vendor comparison matrix, maturity/viability assessment, pricing summary, adoption evidence, recommendation by use case.

**Expert panel:** See `../topic-expert-panels.md` - TEC expert set (3 personas). Invoke in Mode 1 during Phase 11 synthesis; Mode 2 on-demand during research phases; Mode 3 pre-PDF QA.

---

### Module GEO - Geographic / Community Research

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
| G1 | Geographic boundary definition - confirm the specific area, what is and is not included | All |
| G2 | Anchor institution mapping - major organisations, peak bodies, government bodies in the geography | All |
| G3 | Network and intermediary mapping - who connects whom; events, convening bodies, shared platforms | 2/3 |
| G4 | Funder landscape - philanthropy, government grants, community foundations with local focus | 2/3 |
| G5 | Needs and gaps analysis - what is underserved, what do community plans and reports identify | 2/3 |
| G6 | Comparative analysis - similar geographies for benchmarking and model identification | 3 |
| G7 | Synthesis - ecosystem map, stakeholder network, opportunity assessment, KIT answers | All |

**Primary sources:** ABS Census data (TableBuilder or pre-built profiles), Social Traders certified enterprise register, ACNC charity register with geographic filter, local government community plans and social procurement policies, Philanthropy Australia regional data, Buy Social Australia directory, inner Melbourne council websites, VCOSS and sector peak bodies, media with geographic filter.

**Output format:** Ecosystem map - institutional inventory, network relationships, funder landscape, gap analysis, opportunity assessment.

**Expert panel:** See `../topic-expert-panels.md` - GEO expert set (3 personas). Invoke in Mode 1 during Phase 11 synthesis; Mode 2 on-demand during research phases; Mode 3 pre-PDF QA.

---

### Module TRD - Trend / Event Research

**Examples:** "corporate ESG reporting mandate changes 2024-2026", "impact investing in Australia 2020-2025"

**Default KITs:**
1. What drove this trend and how durable is it?
2. What has materially changed in the defined time period?
3. Who are the leaders, laggards, and emerging actors?
4. What is the regulatory/policy trajectory?
5. What does this mean for [specific context, derived from KITs]?

**Phase set:**

| Phase | Content | Tier |
|---|---|---|
| TR1 | Temporal anchoring - enforce the date scope throughout; define what "before" and "after" means for this trend | All |
| TR2 | Chronological reconstruction - build a timeline of key events, decisions, publications within the scope | All |
| TR3 | Driver analysis - what caused this trend; structural, cyclical, or event-driven | 2/3 |
| TR4 | Actor mapping - who is driving, resisting, adapting, or leading edge | 2/3 |
| TR5 | Regulatory/policy trajectory - what is legislated, consulted, or foreshadowed | 2/3 |
| TR6 | Forward-looking assessment - where is this going; scenario analysis if warranted | 2/3 |
| TR7 | Synthesis - trend brief, timeline table, implications, KIT answers | All |

**Primary sources:** Regulatory update feeds (ASIC, Treasury, relevant agencies), parliamentary announcements and Hansard, Big 4 firm publications (credible secondary commentary on regulatory trends), trade press over the defined period, academic analysis, Google Trends trajectory.

**Output format:** Trend brief - timeline table, driver analysis, actor map, regulatory trajectory, forward-looking assessment.

**Expert panel:** See `../topic-expert-panels.md` - TRD expert set (3 personas). Invoke in Mode 1 during Phase 11 synthesis; Mode 2 on-demand during research phases; Mode 3 pre-PDF QA.

---

### Module PRS - Person Research (Public Figure)

**Examples:** "Laura Tingle, political journalist", "Andrew Forrest, philanthropist and businessman"

**Default KITs:**
1. What is their current role and institutional affiliations?
2. What are their publicly stated positions on [relevant topic]?
3. What is their professional track record and major contributions?
4. What networks and relationships are relevant to [our context]?
5. What is their public reputation - credibility, controversies, peer regard?

**Phase set:**

| Phase | Content | Tier |
|---|---|---|
| PR1 | Identity confirmation - confirm the correct person and distinguish from namesakes | All |
| PR2 | Career reconstruction - full professional history from public sources | All |
| PR3 | Public positions and views - stated positions on relevant topics; speeches, writing, interviews | 2/3 |
| PR4 | Network and relationship mapping - institutional affiliations, board positions, known associates | 2/3 |
| PR5 | Public record - awards, publications, media coverage, controversies | 2/3 |
| PR6 | Credibility and reputation assessment - peer regard, critical coverage, professional standing | 3 |
| PR7 | Synthesis - biographical intelligence brief, KIT answers | All |

**Primary sources:** LinkedIn (Layer 4 required - see layer-4-playwright-protocol.md), organisational websites, media archives (filtered search), published works and interviews, Wikipedia as reference map only (not primary source), AEC political donations register (for public figures in political life), AustLII (court records if relevant - public record only).

**Ethical boundaries:** Research covers only the person's PUBLIC role - career, public statements, professional record, institutional affiliations. Private life is out of scope. Privacy Act constraints apply throughout. Flag clearly if any source is self-reported (★ rating).

**Output format:** Biographical intelligence brief - career reconstruction, public positions on relevant topics, network map, professional reputation assessment, practical relevance to [context].

**Expert panel:** See `../topic-expert-panels.md` - PRS expert set (3 personas). Invoke in Mode 1 during Phase 11 synthesis; Mode 2 on-demand during research phases; Mode 3 pre-PDF QA.

---

## Multi-Module Chaining Protocol

Some investigations span multiple topic types. Use this protocol when the Topic Scoping Document KITs cannot be answered by a single module.

**Examples:** "Impact investing in Australia 2020-2025" spans TRD (trend) + MKT (market) + POL (regulatory environment). "Social enterprise ecosystem in inner Melbourne" spans GEO (ecosystem) + MKT (revenue models) + POL (procurement policy).

**Rules:**
1. **Identify the primary module** - the one that best matches the user's primary KITs. This module drives the investigation structure and produces the primary deliverable.
2. **Identify secondary modules** - list only the modules needed to answer KITs that fall outside the primary type.
3. **Capture in the scoping document** - list all modules and their KIT assignments in the Topic Scoping Document before research begins. Format: `Primary module: TRD (KITs 1, 2, 4) | Secondary: MKT (KIT 3) | Secondary: POL (KIT 5)`
4. **Sequence:** Primary module phases run first. Secondary modules run after the primary investigation phase is complete, targeting only the phases relevant to their assigned KITs.
4b. **Module Contamination Firewall:** Each module writes its complete synthesis to a module-specific synthesis file before Phase 11 begins. When Phase 11 integrates findings across modules, it must cite module synthesis files by file reference - not by memory of the module's findings.

Format for cross-module citation in Phase 11 synthesis prose:
`[Source: module-MKT-synthesis.md §Market Sizing - "exact quote or ≤15 word paraphrase"]`

**Rule:** No finding from a secondary module can enter primary module synthesis prose without a `[Source: module-X-synthesis.md]` tag. Untagged cross-module claims are treated as unsourced and must be resolved before the Fabrication Detection Pass (INTELLIGENCE-STANDARDS.md Section 8 Step 6b).

Module synthesis files are saved to:
- Primary module: `05-analysis/synthesis-[MODULE-CODE]-primary.md`
- Secondary modules: `05-analysis/synthesis-[MODULE-CODE]-secondary.md`
5. **Output integration:** Secondary module findings feed into the primary module's synthesis phase. Produce one unified Executive Summary; produce module-specific deliverables (e.g., Policy Brief + Trend Brief) as separate files in `00-summary/`.
6. **Limit:** Maximum 3 modules per investigation. More than 3 indicates the scope needs decomposition into separate investigations with their own scoping documents, folders, and KIT registers.

---
