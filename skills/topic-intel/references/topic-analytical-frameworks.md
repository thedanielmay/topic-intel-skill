# Topic Analytical Frameworks

## Overview

This file contains analytical frameworks adapted for topic-intel investigations. Unlike company-intel, where Porter's Five Forces and SWOT are applied consistently, topic-intel frameworks vary by topic type. Select frameworks based on the topic module and KITs.

---

## Universal Frameworks (All Topic Types)

### ACH — Analysis of Competing Hypotheses

Apply whenever two credible sources contradict each other, or when there is genuine uncertainty about the correct interpretation of evidence.

**Process:**
1. State the competing hypotheses explicitly (H1, H2, H3...)
2. List all evidence relevant to any hypothesis
3. For each piece of evidence, assess: consistent with / inconsistent with / neutral to each hypothesis
4. Count: which hypothesis has the fewest inconsistencies with the evidence?
5. The most defensible hypothesis is the one with the fewest inconsistencies — not the one with the most supporting evidence

**Template:**

| Evidence | H1: [hypothesis] | H2: [hypothesis] | H3: [hypothesis] |
|---|---|---|---|
| [evidence item] | Consistent / Inconsistent / Neutral | C / I / N | C / I / N |
| [evidence item] | | | |
| **Inconsistency count** | [N] | [N] | [N] |

**Verdict:** H[N] is most consistent with the evidence. [Confidence statement.]

---

### Triangulation Matrix

Apply to all key findings to assess source independence and confidence.

| Claim | Source 1 | Source 2 | Source 3 | Independent? | Confidence |
|---|---|---|---|---|---|
| [claim] | [source, type] | [source, type] | [source, type] | Yes/No/Partial | ★★★/★★/★ |

**Source type codes:**
- PR = Primary (official document, legislation, audited data)
- SI = Secondary Independent (news, think-tank, peer-reviewed analysis)
- SD = Secondary Dependent (sources that all cite the same primary source)
- SR = Self-reported (organisation or person's own claims)

Two SD sources = one source for triangulation purposes.

---

### Narrative Consistency Audit

Apply to any synthesis document before finalising.

Check:
1. Do all quantitative claims in the document use consistent figures? (No two sections citing different revenue figures for the same organisation/market)
2. Do temporal references cohere? (No document that says something is "current" when it cites a 2019 source)
3. Are all confidence ratings applied consistently? (No ★★★ rating on a LinkedIn self-reported claim)
4. Does the executive summary accurately reflect the findings in the body? (No claims in the summary that are not supported in the body)
5. Are all gaps acknowledged? (No finding presented as certain when the underlying source is ★)

---

### Inversion Test

Apply to the 3 strongest positive findings before finalising.

For each finding: "What would the evidence look like if the opposite of this finding were true?"

If the answer is "the evidence would look exactly the same," the finding is not well-supported — it may be confirmation bias. If the answer is "the evidence would look materially different," the finding is robust.

---

### Devil's Advocate Check

Required before any synthesis is finalised.

Identify at least 3 findings that cut against the primary narrative. If you cannot find 3, the research may have been confirmation-biased toward the dominant conclusion.

Document them explicitly in the Critical Findings output, even if they are ultimately outweighed by contrary evidence.

---

## Module-Specific Frameworks

### MKT — Market Frameworks

#### Porter's Five Forces (adapted for topic-intel)

| Force | Assessment | Evidence | Implication |
|---|---|---|---|
| Threat of new entrants | High/Medium/Low | [evidence] | [so what] |
| Bargaining power of suppliers | High/Medium/Low | [evidence] | [so what] |
| Bargaining power of buyers | High/Medium/Low | [evidence] | [so what] |
| Threat of substitutes | High/Medium/Low | [evidence] | [so what] |
| Competitive rivalry | High/Medium/Low | [evidence] | [so what] |

**Overall market attractiveness:** [assessment + reasoning]

#### Revenue Model Sustainability Scorecard

For each revenue model identified in the market:

| Model | Revenue source | Customer dependency risk | Margin profile | Scalability | Replicability | Overall rating |
|---|---|---|---|---|---|---|
| [model] | [who pays] | [low/med/high] | [high/med/low] | [high/med/low] | [easy/hard] | [★★★/★★/★] |

#### Market Opportunity Matrix

| Opportunity | Size | Accessibility | Competition | Urgency | Priority |
|---|---|---|---|---|---|
| [opportunity] | [S/M/L] | [easy/medium/hard] | [low/med/high] | [now/12mo/3yr] | [1-5] |

---

### POL — Policy Frameworks

#### Source Authority Pyramid

Apply to confirm every factual claim is sourced from the highest available authority.

```
        ┌─────────────────────────────┐
        │    Primary legislation      │ ★★★
        │   (Acts of Parliament)      │
        ├─────────────────────────────┤
        │  Subordinate legislation    │ ★★★
        │ (Regulations, Instruments)  │
        ├─────────────────────────────┤
        │  Ministerial guidelines     │ ★★★
        │  and policy documents       │
        ├─────────────────────────────┤
        │   Agency guidance /         │ ★★
        │   practice notes / FAQs     │
        ├─────────────────────────────┤
        │  Independent commentary     │ ★★
        │  (Productivity Commission,  │
        │   academic analysis)        │
        ├─────────────────────────────┤
        │  Media / advocacy /         │ ★
        │  self-reported              │
        └─────────────────────────────┘
```

Claims citing only sources from the lower tiers must be flagged (★) and noted as requiring primary source confirmation.

#### Compliance Risk Matrix

| Obligation | Applicability to our context | Compliance status | Risk if non-compliant | Priority |
|---|---|---|---|---|
| [obligation] | [Yes/No/Partial] | [Compliant/Gap/Unknown] | [penalty/consequence] | [High/Med/Low] |

#### Policy Change Scenario Analysis

| Scenario | Probability | Trigger | Impact on us | Response |
|---|---|---|---|---|
| [e.g. "Policy strengthened"] | [High/Med/Low] | [what would cause it] | [positive/negative/neutral + detail] | [recommended response] |
| [e.g. "Policy repealed"] | | | | |
| [e.g. "No change"] | | | | |

---

### SCI — Scientific / Academic Frameworks

#### Evidence Hierarchy (generic)

Apply to classify sources consistently:

| Level | Type | Confidence | Examples |
|---|---|---|---|
| 1 | Systematic review / meta-analysis | ★★★ | Cochrane review, PRISMA-compliant synthesis |
| 2 | Randomised controlled trial | ★★★ | Peer-reviewed RCT |
| 3 | Cohort / observational study | ★★ | Peer-reviewed longitudinal study |
| 4 | Case study / case series | ★★ | Published case study |
| 5 | Expert opinion / editorial | ★ | Expert commentary, opinion piece |
| 6 | Grey literature / pre-print | ★ | Conference paper, working paper, arXiv |

**Mapping to the standard three-star confidence system:**
- Levels 1–2 (systematic reviews, RCTs) → ★★★
- Levels 3–4 (cohort studies, case studies) → ★★
- Levels 5–6 (expert opinion, grey literature, pre-prints) → ★

Apply this mapping when assigning confidence ratings in the KIT Answer Register and Triangulation Matrix so all outputs use a consistent rating system.

#### Research Gap Matrix

| Sub-topic | Evidence quality | Volume of research | Practical application | Research priority |
|---|---|---|---|---|
| [sub-topic] | [strong/moderate/weak] | [high/medium/low] | [clear/unclear/none] | [high/medium/low] |

#### Knowledge Frontier Mapping

Identify where the research frontier currently sits:

| Area | Current consensus | Open questions | Active debates | Direction of travel |
|---|---|---|---|---|
| [area] | [what is settled] | [what is unknown] | [what is contested] | [where is research heading] |

---

### TEC — Technology Frameworks

#### Technology Readiness Assessment

| Vendor / Solution | Maturity level | Adoption evidence | Australian presence | Risk rating |
|---|---|---|---|---|
| [vendor] | [Concept/Beta/Early/Growth/Mature] | [none/limited/strong] | [yes/no/planned] | [low/med/high] |

#### Build / Buy / Partner Decision Matrix

| Option | Capability match | Cost | Time to value | Control | Risk | Recommendation |
|---|---|---|---|---|---|---|
| Build internally | [low/med/high] | [high] | [12-18mo] | [full] | [high] | |
| Buy [Vendor A] | [high] | [$X/mo] | [1-3mo] | [medium] | [low] | |
| Partner / integrate | [medium] | [low] | [3-6mo] | [low] | [medium] | |

#### Total Cost of Ownership Estimator

| Cost category | Year 1 | Year 2 | Year 3 | Notes |
|---|---|---|---|---|
| Licence / subscription | [$] | [$] | [$] | |
| Implementation / setup | [$] | — | — | One-time |
| Training | [$] | [$] | — | |
| Integration / development | [$] | — | — | |
| Ongoing support | [$] | [$] | [$] | |
| **Total** | **[$]** | **[$]** | **[$]** | |

---

### GEO — Geographic / Ecosystem Frameworks

#### Ecosystem Health Assessment

| Dimension | Rating | Evidence | Trend |
|---|---|---|---|
| Diversity (types of actors) | Green/Amber/Red | [evidence] | [improving/stable/declining] |
| Connectivity (how well networked) | Green/Amber/Red | [evidence] | |
| Resource availability (funding, talent) | Green/Amber/Red | [evidence] | |
| Adaptive capacity (how well it responds to change) | Green/Amber/Red | [evidence] | |
| Leadership and coordination | Green/Amber/Red | [evidence] | |

#### Stakeholder Power / Interest Matrix

Plot key actors on a 2x2:
- X axis: Interest in the topic / our work (low to high)
- Y axis: Power / influence (low to high)

| Quadrant | Actors | Strategy |
|---|---|---|
| High power, high interest | [names] | Engage deeply, keep close |
| High power, low interest | [names] | Keep satisfied, don't ignore |
| Low power, high interest | [names] | Keep informed, potential allies |
| Low power, low interest | [names] | Monitor only |

---

### TRD — Trend Frameworks

#### Trend Durability Assessment

| Driver | Type | Durability | Evidence | Reversal triggers |
|---|---|---|---|---|
| [driver] | [Structural/Cyclical/Event] | [High/Med/Low] | [evidence] | [what would reverse it] |

**Overall durability verdict:** [High/Medium/Low] — [reasoning]

#### Scenario Analysis (3 scenarios)

| Scenario | Probability | Key assumptions | Impact on [topic] | Impact on us | Time horizon |
|---|---|---|---|---|---|
| Bull case | [%] | [what must be true] | [positive developments] | [implication] | [1-3yr] |
| Base case | [%] | [current trajectory] | [expected developments] | [implication] | [1-3yr] |
| Bear case | [%] | [what must go wrong] | [negative developments] | [implication] | [1-3yr] |

#### Adoption Curve Positioning

Where does this trend sit on the adoption curve, and what does that mean for action timing?

```
Innovators --> Early Adopters --> Early Majority --> Late Majority --> Laggards
    2%              13%               34%               34%            16%
```

**Current position:** [where the trend sits]
**Implication for timing:** [act now / prepare / watch / wait]

---

## Selecting Frameworks for an Investigation

Use this guide to select the right frameworks based on topic type and KITs:

| If your KITs ask about... | Use these frameworks |
|---|---|
| Market size and competitive dynamics | Porter's Five Forces, Market Opportunity Matrix |
| Revenue sustainability | Revenue Model Sustainability Scorecard |
| Whether to comply and at what cost | Compliance Risk Matrix, Source Authority Pyramid |
| What a regulation might do in future | Policy Change Scenario Analysis |
| How strong the evidence is | Evidence Hierarchy, Research Gap Matrix |
| Whether to build, buy, or partner | Build/Buy/Partner Matrix, TCO Estimator |
| How healthy an ecosystem is | Ecosystem Health Assessment, Stakeholder Power/Interest Matrix |
| How durable a trend is | Trend Durability Assessment, Adoption Curve Positioning |
| Contradictory evidence | ACH — Analysis of Competing Hypotheses |
| Any finding you want to stress-test | Inversion Test, Devil's Advocate Check |

All frameworks should include a "So What?" paragraph connecting the framework output to the user's stated purpose and KITs. A framework without a So What is decoration.
