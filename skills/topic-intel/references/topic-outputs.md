# Topic Output Templates

## Overview

This file contains all output templates for topic-intel investigations, organised by topic type. Every investigation produces the universal outputs (Section 1) plus the type-specific output for its module (Section 2).

---

## Section 0 — Quick Scan Briefing Note (Tier 1 only)

The Quick Scan produces a single 1-page briefing note. It does not produce a RAG Dashboard, Triangulation Matrix, or Contested Claims Register. KIT answers are 2-sentence summaries, not full structured entries.

Save to: `_topic-{slug}/quick-scan-briefing-YYYY-MM-DD.md`

```markdown
# Quick Scan Briefing Note
**Topic:** [topic as stated and refined]
**Type:** [topic type code — MKT / POL / SCI / TEC / GEO / TRD / PRS]
**Date:** [YYYY-MM-DD]
**Prepared by:** topic-intel Quick Scan (Tier 1)

---

## Key Findings

1. [Finding — one sentence] [★★★ / ★★ / ★]
2. [Finding] [confidence]
3. [Finding] [confidence]
4. [Finding] [confidence]
5. [Finding] [confidence]

---

## KIT Answers

**KIT 1: [question]**
[2-sentence direct answer. Lead with the answer.] [★★★ / ★★ / ★]

**KIT 2: [question]**
[2-sentence direct answer.] [confidence]

[repeat for each KIT — maximum 5]

---

## Top Information Gaps

1. [Most significant missing piece and which KIT it affects]
2. [Second gap]
3. [Third gap]

---

## Recommended Next Steps

[1-2 sentences: what would a Standard or Full Investigation add? What specific trigger should prompt an upgrade from Quick Scan to Standard?]

---

*Sources accessed: [N] | Layers used: [1 / 2] | Approximate research time: ~30 min*
*Note: Quick Scan uses Layer 2 maximum. No Playwright or scraping service access.*
```

---

## Section 1 — Universal Outputs (All Topic Types, All Tiers)

### 1.1 KIT Answer Register

```markdown
# KIT Answer Register
**Topic:** [topic]
**Date:** [YYYY-MM-DD]

---

## KIT 1: [question]

**Answer:** [Direct answer — 2-5 sentences. Lead with the answer, not the caveats.]

**Confidence:** ★★★ / ★★ / ★

**Key sources:**
- [Source 1 — name, type, confidence]
- [Source 2 — name, type, confidence]

**Gaps that would improve confidence:**
- [What is missing and where it might be found]

---

## KIT 2: [question]
[same structure]

[repeat for each KIT]

---

## Overall assessment

[2-3 sentences: How well did the research answer the KITs overall? What remains uncertain?]
```

---

### 1.2 Critical Findings

```markdown
# Critical Findings
**Topic:** [topic]
**Date:** [YYYY-MM-DD]

These are the [N] findings most relevant to the stated purpose. Each is directly actionable.

---

## Finding 1: [headline — one sentence]

**Evidence:** [What was found, from which sources, with what confidence]

**So What?** [Direct implication for the user's purpose — what should change as a result of knowing this]

**Source(s):** [primary sources, confidence ratings]

---

## Finding 2: [headline]
[same structure]

[5–7 findings total]

---

## Devil's Advocate — Findings That Cut Against the Main Narrative

1. [Finding that contradicts or complicates the primary thesis]
2. [Finding that contradicts or complicates the primary thesis]
3. [Finding that contradicts or complicates the primary thesis]
```

---

### 1.3 Executive Summary

```markdown
# Executive Summary
**Topic:** [topic]
**Type:** [topic type]
**Investigation depth:** [tier]
**Date:** [YYYY-MM-DD]
**Prepared for:** [context — e.g. "GreenCollect turnaround, March 2026"]

---

## Purpose

[One sentence: what decision or purpose this intelligence serves]

---

## Key Intelligence Topics

[List the KITs as stated at the start — 2-5 items]

---

## What We Found

[3-5 paragraphs. Each paragraph addresses one KIT or one major theme. Lead with findings, not methodology. Every claim has a confidence indicator inline: ★★★, ★★, or ★.]

---

## What We Don't Know

[1-2 paragraphs: the most significant gaps and what they mean for the reliability of the overall picture]

---

## Recommended Actions

[3-5 specific, actionable steps that follow from the findings. Addressed to the relevant person by first name where appropriate.]

---

## Confidence Assessment

Overall confidence in this intelligence picture: [HIGH / MEDIUM / LOW]

[2-3 sentences explaining why: e.g. "Three KITs answered at ★★★ from official government sources. KIT 4 relies on ★ sources only — treat with caution."]
```

---

### 1.4 Information Gaps Register

```markdown
# Information Gaps
**Topic:** [topic]
**Date:** [YYYY-MM-DD]

Gaps are intelligence. The absence of a source, or the failure to find data, is itself a finding — it tells us something about the topic's documentation, opacity, or contested status.

---

## HIGH Priority Gaps (affect KIT answers)

| Gap | Which KIT | Why it matters | What would resolve it | Obtainable? |
|---|---|---|---|---|
| [what's missing] | KIT [N] | [implication] | [specific source] | Yes (manual) / No |

---

## MEDIUM Priority Gaps (would add depth)

| Gap | Section affected | What would resolve it |
|---|---|---|
| [what's missing] | [section] | [source] |

---

## Inaccessible Sources

[Reference `inaccessible-sources.md` at the root of the `_topic-{slug}/` folder — it sits alongside `research-log.md`, not inside `00-summary/`]

---

## Interpretation

[1-2 paragraphs: What do these gaps, taken together, tell us? Is this a well-documented topic with good data availability, or a poorly-documented one? Does opacity itself signal something?]
```

---

### 1.5 Monitoring Plan

```markdown
# Monitoring Plan
**Topic:** [topic]
**Date:** [YYYY-MM-DD]
**Recommended refresh:** [monthly / quarterly / on-trigger / one-off]

---

## Google Alerts to Set Up

| Alert term | Frequency | Why |
|---|---|---|
| "[exact topic phrase]" | Weekly | Core topic monitoring |
| "[key organisation]" | Weekly | Key actor monitoring |
| "[policy name] amendment OR update OR review" | Weekly | Regulatory changes |

---

## Regulatory / Publication Feeds

| Source | What to watch for | How to monitor |
|---|---|---|
| [agency] | [type of update] | [URL or subscribe link] |

---

## Trigger Events

Events that should prompt an immediate refresh of this intelligence:

- [Trigger event 1 — e.g. "New government budget released"]
- [Trigger event 2 — e.g. "Named organisation announces major change"]
- [Trigger event 3]

---

## Refresh Cadence

**Next scheduled refresh:** [date, or "on trigger only"]

**Refresh scope:** [what sections need updating at refresh vs. what can be carried forward]
```

---

### 1.6 Assumptions and Limitations

```markdown
# Assumptions and Limitations
**Topic:** [topic]
**Date:** [YYYY-MM-DD]

---

## Key Assumptions

| Assumption | Basis | Risk if wrong |
|---|---|---|
| [assumption] | [why it was made] | [what changes if false] |

---

## Data Limitations

| Limitation | Sections affected | Impact on confidence |
|---|---|---|
| [e.g. "IBISWorld market data paywalled — used ABS as proxy"] | [section] | [medium — ABS less granular] |
| [e.g. "LinkedIn data obtained without authentication — career histories incomplete"] | [section] | [low — confirmed via other sources] |

---

## Temporal Limitations

This research reflects the state of intelligence as of [date]. The following elements are most likely to have changed since then:
- [element most likely to become stale]
- [element most likely to become stale]

---

## Source Independence Note

[Note any cases where multiple citations refer back to the same original source — warn against treating these as independent confirmation]
```

---

## Section 2 — Type-Specific Output Templates

### 2.1 MKT — Market Landscape Brief

```markdown
# Market Landscape Brief: [Market Name]
**Date:** [YYYY-MM-DD]
**Scope:** [geographic, temporal, sectoral boundaries]

---

## Market at a Glance

| Dimension | Finding | Confidence |
|---|---|---|
| Market size (revenue) | [figure or range] | ★★★/★★/★ |
| Market size (volume) | [figure or range] | ★★★/★★/★ |
| Growth rate (CAGR) | [figure] | ★★★/★★/★ |
| Number of operators | [figure or range] | ★★/★ |
| Market concentration | [e.g. "fragmented — top 5 players hold <30%"] | ★★/★ |

---

## Key Players

| Organisation | Type | Scale | Model | Positioning | Confidence |
|---|---|---|---|---|---|
| [name] | [NFP/commercial/hybrid] | [revenue/size] | [how they make money] | [competitive position] | ★★/★ |

---

## Market Segmentation

[Description of how the market breaks down — by geography, customer type, product, price point, or channel — with data where available]

---

## Revenue Models in Use

| Model | Who uses it | Viability | Notes |
|---|---|---|---|
| [e.g. "Fee for service"] | [organisations] | [high/medium/low] | [context] |

---

## Regulatory Environment

[Summary of the key laws, standards, government programs, and procurement policies shaping this market]

---

## Market Drivers and Constraints

**Drivers:**
- [driver 1 — evidence]
- [driver 2 — evidence]

**Constraints:**
- [constraint 1 — evidence]
- [constraint 2 — evidence]

---

## Opportunity Map

| Opportunity | Description | Addressable by whom | Timing | Confidence |
|---|---|---|---|---|
| [opportunity] | [detail] | [who is positioned] | [now/12mo/3yr] | ★★/★ |

---

## KIT Answers

[Reference KIT Answer Register or embed here]
```

---

### 2.2 POL — Policy Brief

```markdown
# Policy Brief: [Policy Name]
**Date:** [YYYY-MM-DD]
**Current version:** [instrument name, version/date]
**Status:** [In force / Pending / Under review / Repealed]

---

## Policy at a Glance

| Element | Detail | Source |
|---|---|---|
| Full name | [official name] | ★★★ |
| Administering body | [department/agency] | ★★★ |
| Legislative basis | [Act/Regulation] | ★★★ |
| Commenced | [date] | ★★★ |
| Last amended | [date] | ★★★ |
| Next review | [date or "not scheduled"] | ★★/★ |

---

## Scope — Who Must Comply and With What

| Obligation | Who it applies to | Threshold | Effective date |
|---|---|---|---|
| [obligation] | [entity type] | [contract value/size/other] | [date] |

---

## Source Authority Hierarchy

1. [Primary legislation — name and link]
2. [Subordinate legislation / regulations]
3. [Ministerial guidelines]
4. [Agency guidance documents]
5. [Practice notes / FAQs]

---

## Implementation Landscape

[How is the policy being applied in practice? Audit findings, case decisions, reported compliance rates, known implementation challenges]

---

## Stakeholder Positions

| Stakeholder | Position | Basis |
|---|---|---|
| [organisation] | [supports/opposes/neutral] | [evidence] |

---

## Compliance Implications

[Practical implications for the user's specific context — derived from KITs. What must they do, by when, and what happens if they don't?]

---

## Timeline

| Date | Event |
|---|---|
| [date] | [policy introduced / amended / reviewed] |

---

## KIT Answers

[Reference KIT Answer Register or embed here]
```

---

### 2.3 SCI — Research Brief

```markdown
# Research Brief: [Topic]
**Date:** [YYYY-MM-DD]
**Literature search scope:** [date range, databases, search terms used]
**Papers identified:** [N] | **Papers reviewed:** [N] | **Saturation reached:** [Yes/No]

---

## State of the Evidence

**Overall assessment:** [Strong / Moderate / Weak / Emerging / Contested]

[2-3 paragraphs: What is the current state of knowledge? Where is evidence strong, where is it thin, where is it contested?]

---

## Key Findings with Confidence

| Finding | Evidence basis | Confidence | Key citations |
|---|---|---|---|
| [finding] | [study type, N] | ★★★/★★/★ | [author year] |

---

## Evidence Landscape Map

| Sub-topic | Evidence strength | Key papers | Gaps |
|---|---|---|---|
| [sub-topic] | [Strong/Moderate/Weak/None] | [citations] | [what's missing] |

---

## Contested Areas

[For each contested claim, show the competing positions and ACH assessment]

**Contested claim:** [statement]
- Position A: [who holds it, evidence, confidence]
- Position B: [who holds it, evidence, confidence]
- ACH verdict: [which is more consistent with all evidence, or "genuinely unresolved"]

---

## Leading Researchers and Institutions

| Researcher | Institution | Focus | Notable work |
|---|---|---|---|
| [name] | [institution] | [sub-topic] | [key paper/contribution] |

---

## Funding Landscape

[What is government and philanthropic funding in this research area? ARC grants, NHMRC, etc.]

---

## Practical Applications

[What emerges from the literature that has real-world application? What can practitioners do with this evidence?]

---

## Research Gaps

[What does the literature itself identify as needing more research? What did the search fail to find?]

---

## KIT Answers

[Reference KIT Answer Register or embed here]
```

---

### 2.4 TEC — Technology Landscape

```markdown
# Technology Landscape: [Category]
**Date:** [YYYY-MM-DD]
**Scope:** [what's included and excluded from this category]

---

## Market Overview

[Brief description of the technology category — what it does, who uses it, maturity stage]

---

## Vendor Comparison Matrix

| Vendor | Product | Pricing model | Price range | Key capabilities | Target size | Australian users | Viability | Confidence |
|---|---|---|---|---|---|---|---|---|
| [name] | [product name] | [SaaS/perpetual/usage] | [$X/month] | [top 3 capabilities] | [SMB/Enterprise] | [yes/no/unknown] | [high/medium/low] | ★★/★ |

---

## Capability Map

[What capabilities exist across the category — which vendors offer what, and where are the gaps]

---

## Pricing Summary

| Vendor | Entry tier | Mid tier | Enterprise | Free trial? |
|---|---|---|---|---|
| [vendor] | [price] | [price] | [negotiated] | [Yes/No] |

---

## Adoption Evidence (Australian / Sector-Specific)

[Case studies, user reviews, known Australian adopters — especially in the relevant sector]

---

## Integration Landscape

[What do these platforms connect to? What are the technical dependencies?]

---

## Vendor Viability Assessment

| Vendor | Funding status | Team size | Support quality | Risk rating |
|---|---|---|---|---|
| [vendor] | [bootstrapped/VC/public] | [approx] | [from reviews] | [low/medium/high] |

---

## Recommendation by Use Case

| Use case | Recommended solution | Runner-up | Avoid | Reasoning |
|---|---|---|---|---|
| [use case] | [vendor] | [vendor] | [vendor if applicable] | [brief reason] |

---

## KIT Answers

[Reference KIT Answer Register or embed here]
```

---

### 2.5 GEO — Ecosystem Map

```markdown
# Ecosystem Map: [Geography / Community]
**Date:** [YYYY-MM-DD]
**Scope:** [exact geographic boundaries, sector scope, what is and is not included]

---

## Ecosystem at a Glance

| Dimension | Finding | Confidence |
|---|---|---|
| Significant organisations identified | [N] | ★★/★ |
| Active networks / intermediaries | [N] | ★★/★ |
| Known funders | [N] | ★★/★ |
| Identified gaps | [N] | ★/★★ |

---

## Anchor Institutions

| Organisation | Type | Scale | Role in ecosystem | Key contact(s) | Confidence |
|---|---|---|---|---|---|
| [name] | [type] | [size/budget] | [what they do for the ecosystem] | [name, role] | ★★/★ |

---

## Networks and Intermediaries

| Network | Purpose | Members/reach | Key connector | Relevant to us? |
|---|---|---|---|---|
| [name] | [what it does] | [scale] | [key person] | [Yes/No/Potentially] |

---

## Funder Landscape

| Funder | Type | Focus areas | Geographic scope | Relevant programs | Contact |
|---|---|---|---|---|---|
| [funder] | [govt/philanthropy/corporate] | [what they fund] | [local/state/national] | [specific programs] | [contact info] |

---

## Gaps and Unmet Needs

| Gap | Evidence | Who is positioned to fill it | Opportunity rating |
|---|---|---|---|
| [identified gap] | [source] | [who could address it] | [high/medium/low] |

---

## Positioning of [Named Organisation] in Ecosystem

[How the named organisation (e.g. GreenCollect) sits relative to peers — strengths, gaps, competitive position, collaboration opportunities]

---

## KIT Answers

[Reference KIT Answer Register or embed here]
```

---

### 2.6 TRD — Trend Brief

```markdown
# Trend Brief: [Trend Name]
**Date:** [YYYY-MM-DD]
**Time scope:** [start date] to [end date]
**Geographic scope:** [Australia / Global / specific]

---

## Trend at a Glance

| Dimension | Assessment | Confidence |
|---|---|---|
| Trend strength | [Strong / Moderate / Weak / Reversing] | ★★/★ |
| Durability | [Structural / Cyclical / Event-driven] | ★★/★ |
| Maturity | [Emerging / Growing / Mature / Declining] | ★★/★ |
| Regulatory trajectory | [Accelerating / Stable / Uncertain / Reversing] | ★★/★ |

---

## Timeline

| Date | Event | Significance | Source |
|---|---|---|---|
| [date] | [event] | [why it matters] | [source, confidence] |

---

## Driver Analysis

**What caused this trend:**
- [Structural driver — long-term, likely durable]
- [Cyclical driver — likely to reverse]
- [Event-driven driver — tied to a specific event]

---

## Actor Map

| Actor | Role in trend | Position | Direction |
|---|---|---|---|
| [actor] | [driver/adapter/resister/leader] | [leading/following/lagging] | [accelerating/slowing] |

---

## Regulatory and Policy Trajectory

[What is legislated, consulted, foreshadowed, or at risk of reversal — with dates where available]

---

## Forward-Looking Assessment

**Most likely scenario (next 12–24 months):**
[Description]

**Upside scenario:**
[What would accelerate or amplify the trend]

**Downside / reversal scenario:**
[What would slow or reverse the trend]

---

## Implications for [Specific Context]

[Direct implications for the user's context — derived from KITs. What should they do, stop doing, or watch?]

---

## KIT Answers

[Reference KIT Answer Register or embed here]
```

---

### 2.7 PRS — Biographical Intelligence Brief

```markdown
# Biographical Intelligence Brief: [Person Name]
**Date:** [YYYY-MM-DD]
**Scope:** Public role only — career, public statements, professional record, institutional affiliations

---

## Identity Confirmation

| Field | Detail | Source | Confidence |
|---|---|---|---|
| Full name | [name] | [source] | ★★★ |
| Current role | [title, organisation] | [source] | ★★/★ |
| Location | [city, country] | [source] | ★★/★ |
| LinkedIn | [URL if public] | Direct | ★★ |

---

## Career Reconstruction

| Period | Role | Organisation | Notes | Confidence |
|---|---|---|---|---|
| [dates] | [title] | [org] | [notable achievements/context] | ★★/★ |

---

## Public Positions and Views

| Topic | Position | Stated where | Date | Confidence |
|---|---|---|---|---|
| [topic relevant to KITs] | [stated position] | [speech/article/interview] | [date] | ★★/★ |

---

## Network and Affiliations

| Organisation | Role | Active? | Relevance |
|---|---|---|---|
| [org] | [board/advisor/member] | [yes/no/unknown] | [why it matters] |

---

## Public Record

**Awards and recognition:**
[List with dates and sources]

**Publications:**
[List of significant published work]

**Media coverage:**
[Key media appearances, themes, tone]

**Controversies:**
[Any public controversies — state factually from sourced public record only]

---

## Credibility and Reputation Assessment

[Assessment of professional standing, peer regard, reliability as a source — from third-party coverage, citations, and public record only]

---

## Practical Relevance

[How this person is relevant to the user's stated purpose — derived from KITs. Approach vectors, shared connections, topics to raise or avoid]

---

## KIT Answers

[Reference KIT Answer Register or embed here]
```

---

## Section 3 — RAG Dashboard (Adapted for Topics)

The RAG (Red/Amber/Green) dashboard provides a one-page traffic-light summary. Row labels vary by topic type.

**RAG Key:**
- Green — favourable, low risk, strong evidence
- Amber — mixed, uncertain, or moderate risk
- Red — unfavourable, high risk, or critical gap

### MKT — Market RAG

| Dimension | RAG | Summary |
|---|---|---|
| Market size and growth | Green/Amber/Red | [one sentence] |
| Competitive intensity | Green/Amber/Red | [one sentence] |
| Regulatory environment | Green/Amber/Red | [one sentence] |
| Revenue model viability | Green/Amber/Red | [one sentence] |
| Entry barriers | Green/Amber/Red | [one sentence] |
| Data quality | Green/Amber/Red | [one sentence] |

### POL — Policy RAG

| Dimension | RAG | Summary |
|---|---|---|
| Compliance clarity | Green/Amber/Red | [one sentence] |
| Implementation status | Green/Amber/Red | [one sentence] |
| Enforcement risk | Green/Amber/Red | [one sentence] |
| Political durability | Green/Amber/Red | [one sentence] |
| Stakeholder support | Green/Amber/Red | [one sentence] |
| Evidence quality | Green/Amber/Red | [one sentence] |

### SCI — Research RAG

| Dimension | RAG | Summary |
|---|---|---|
| Evidence strength | Green/Amber/Red | [one sentence] |
| Consensus level | Green/Amber/Red | [one sentence] |
| Recency of evidence | Green/Amber/Red | [one sentence] |
| Practical applicability | Green/Amber/Red | [one sentence] |
| Research momentum | Green/Amber/Red | [one sentence] |
| Data availability | Green/Amber/Red | [one sentence] |

### TEC — Technology RAG

| Dimension | RAG | Summary |
|---|---|---|
| Solution maturity | Green/Amber/Red | [one sentence] |
| Vendor viability | Green/Amber/Red | [one sentence] |
| Pricing accessibility | Green/Amber/Red | [one sentence] |
| Australian availability | Green/Amber/Red | [one sentence] |
| Integration complexity | Green/Amber/Red | [one sentence] |
| Evidence of adoption | Green/Amber/Red | [one sentence] |

### GEO — Ecosystem RAG

| Dimension | RAG | Summary |
|---|---|---|
| Ecosystem richness | Green/Amber/Red | [one sentence] |
| Network connectivity | Green/Amber/Red | [one sentence] |
| Funder availability | Green/Amber/Red | [one sentence] |
| Gap / opportunity | Green/Amber/Red | [one sentence] |
| Our positioning | Green/Amber/Red | [one sentence] |
| Data quality | Green/Amber/Red | [one sentence] |

### TRD — Trend RAG

| Dimension | RAG | Summary |
|---|---|---|
| Trend strength | Green/Amber/Red | [one sentence] |
| Durability | Green/Amber/Red | [one sentence] |
| Regulatory trajectory | Green/Amber/Red | [one sentence] |
| Our exposure / opportunity | Green/Amber/Red | [one sentence] |
| Time sensitivity | Green/Amber/Red | [one sentence] |
| Evidence quality | Green/Amber/Red | [one sentence] |

### PRS — Person RAG

| Dimension | RAG | Summary |
|---|---|---|
| Identity confidence | Green/Amber/Red | [one sentence] |
| Alignment with our purpose | Green/Amber/Red | [one sentence] |
| Accessibility / approach | Green/Amber/Red | [one sentence] |
| Reputation and credibility | Green/Amber/Red | [one sentence] |
| Known risks | Green/Amber/Red | [one sentence] |
| Intelligence quality | Green/Amber/Red | [one sentence] |
