# Intelligence Standards — Shared Reference

## Purpose

This document defines the universal standards that govern all intelligence work across the `company-intel` and `topic-intel` skills. Both skills reference this file. When a standard is updated here, both skills pick it up automatically.

**Read this at the start of every investigation.**

Skill-specific methodology (phases, modules, source registers, output templates) lives in each skill's own files. This document covers only the patterns that must be consistent across all intelligence work.

---

## 1. Investigation Lifecycle

Every investigation — regardless of skill or topic type — follows this lifecycle:

```
Phase 0: Requirements & Scoping   ← CONFIRM before any research
Wave 1:  Foundation               ← Entity/topic confirmation only
Wave 2:  Primary research         ← Parallel; threshold gate before Wave 3
Wave 3:  Secondary research       ← Parallel; builds on Wave 2
Wave 4:  Analysis                 ← After all data gathered
Wave 5:  Synthesis & QA           ← Deliverable generation
```

**The STOP gate rule:** Research does not begin until the user has confirmed the Scoping Document. Do not run any searches, WebFetch calls, or tool checks while awaiting confirmation. The temptation to begin research during scoping is the single most common failure mode. Resist it.

---

## 2. Scoping Document

Every investigation begins with a Scoping Document confirmed by the user. The exact format varies by skill (see each skill's Phase 0), but every Scoping Document must include:

| Field | Purpose |
|---|---|
| Subject | The entity or topic being investigated |
| Purpose / use case | What decision this intelligence supports |
| Investigation depth | Tier 1 (Quick Scan) / Tier 2 (Standard) / Tier 3 (Full) |
| Key Intelligence Topics (KITs) | 2–5 specific questions to answer |
| Scope inclusions | What is explicitly in scope |
| Scope exclusions | What is explicitly out of scope — prevents drift |
| End consumer | Who receives this intelligence |
| Output location | Where files will be saved |

**Research does not begin until the user confirms this document.**

Save to: `[investigation-folder]/00-summary/[scoping-document].md`

---

## 3. Key Intelligence Topics (KITs)

KITs are the 2–5 questions the investigation must answer. They drive every phase.

**KIT elicitation:** If the user has not specified KITs, propose a set based on the stated purpose and ask for confirmation before proceeding.

**KIT Answer Register:** At the synthesis phase, produce a structured answer for each KIT:

```
KIT [N]: [question]
Answer:      [direct answer — 2–5 sentences. Lead with the answer, not the caveats.]
Confidence:  [★★★ / ★★ / ★]
Key sources: [2–3 most authoritative sources used]
Gaps:        [what would improve confidence if found]
```

Save to: `[investigation-folder]/00-summary/kit-answers.md`

**KIT conflict rule:** If KIT answers point in contradictory directions (e.g. opportunity is large but barriers are prohibitive, or policy is clear but implementation is absent), surface this tension explicitly in the Executive Summary. Do not resolve it artificially. Contradictory KIT answers are often the most important finding.

---

## 4. Confidence Rating System

Apply to every piece of information recorded:

| Rating | Meaning | Examples |
|---|---|---|
| ★★★ | Official, audited, or primary source | Government statistics, audited financials, primary legislation, peer-reviewed systematic review, ASIC filing, court record |
| ★★ | Credible independent source | Quality journalism, credible think-tank, industry body data with disclosed methodology, analyst report |
| ★ | Informal or unverified | Organisation self-reported claims, social media posts, blog posts, advocacy, pre-prints, forum comments |
| ⚠ | Contradicted by another source | Flag, note the contradiction, apply ACH |

**Source independence rule:** Two sources that both cite the same original document = one source, not two. Mark as Secondary Dependent (SD) in the Triangulation Matrix.

**SCI module note:** The six-level academic Evidence Hierarchy maps to the three-star system as follows: Levels 1–2 (systematic reviews, RCTs) → ★★★; Levels 3–4 (cohort/observational studies, case studies) → ★★; Levels 5–6 (expert opinion, grey literature, pre-prints) → ★.

---

## 5. Source Saturation Rule

When two consecutive search queries on the same subject area or KIT return no sources not already in the research log, declare saturation for that area and move on.

Saturation is a valid finding — it means public information on this subject is genuinely limited, or the area is well-covered by existing sources. Note it explicitly in the relevant phase file and in the Information Gaps output.

Do not invent new search angles or expand scope to fill a word count.

This rule applies globally. Module SCI (academic research) has an additional saturation threshold: when new searches return ≥50% papers already in the literature register, stop searching.

---

## 6. Wave Execution and Progress Reporting

### Wave structure

Run research in waves as defined by each skill. The universal rules:

- **Wave 1** must complete before Wave 2 begins (foundation/identity confirmation)
- **Wave 2** runs in parallel across multiple source types
- **Wave 2 threshold gate:** If primary source identification returns fewer than the minimum substantive sources for any KIT (company-intel: 2 independent sources; topic-intel: 3 substantive sources), pause and report to the user before proceeding to Wave 3. Do not build synthesis on a thin evidence base.
- Waves 3–5 may run in parallel within their wave

### Progress reporting (mandatory)

After completing each wave, post a wave summary before beginning the next wave. Do not proceed without posting this update:

```
Wave [N] complete — [phases/modules covered]
  Sources accessed: [N total] | New this wave: [N] | Gaps logged: [N]
  KIT status: KIT 1 ✓  KIT 2 partial  KIT 3 ✗
  Notable: [1–2 sentence highlight of most significant finding so far]
  Proceeding to Wave [N+1] →
```

This is not optional. It keeps the user informed and serves as a forcing function to check KIT progress before committing further research effort.

---

## 7. Triangulation Matrix

Apply to all key findings before finalising synthesis. For each major claim:

| Claim | Source 1 | Source 2 | Source 3 | Independent? | Confidence |
|---|---|---|---|---|---|
| [claim] | [source, type] | [source, type] | [source, type] | Yes/No/Partial | ★★★/★★/★ |

**Source type codes:**
- **PR** = Primary (official document, legislation, audited data, court record)
- **SI** = Secondary Independent (news, think-tank, peer-reviewed analysis — independently arrived at)
- **SD** = Secondary Dependent (sources that all cite the same primary source)
- **SR** = Self-reported (organisation or person's own claims)

Two SD sources = one source for triangulation purposes.

### Numeric Verification Protocol

Numbers are the highest hallucination-risk element in synthesis. Before finalising any deliverable, extract every numeric token from the draft synthesis and verify each against its originating research-phase file.

**What counts as a numeric claim:** Any digit, percentage, currency amount, year, date, threshold, count, or ratio in a factual sentence.

**Verification steps:**
1. For each numeric value in the synthesis, identify the specific research-phase file and passage it came from
2. Confirm the number matches — exact match required, not approximation
3. If a source says "approximately $200M" and the synthesis says "$214M", this is a paraphrase hallucination — revert to the source's language
4. Flag any number that cannot be traced to a specific file with: `[NUMERIC GAP — not sourced]` and rewrite as a range or omit

**Add to the Triangulation Matrix:** A `Numeric verified?` column:

| Claim | Source 1 | Source 2 | Source 3 | Independent? | Confidence | Numeric verified? |
|---|---|---|---|---|---|---|
| [claim] | [source, type] | [source, type] | [source, type] | Yes/No/Partial | ★★★/★★/★ | Y / N / ⚠ / N/A |

Any `⚠` in the Numeric verified column must be resolved before PDF generation.

---

## 8. Synthesis Phase — Universal Steps

Regardless of skill, the synthesis phase follows this sequence. **Steps 0, 6b, and 7b are anti-hallucination enforcement points — do not skip them.**

**Step 0: Cite-Before-You-Write (synthesis source register)**

Before drafting any synthesis prose, produce a flat claim-to-source register in `00-summary/synthesis-source-register.md`. Format:

```markdown
# Synthesis Source Register
**Subject:** [entity or topic]
**Date:** [YYYY-MM-DD]

| Claim (max 20 words) | Source file | Passage (quote or paraphrase ≤15 words) | Confidence |
|---|---|---|---|
| [claim] | [phase-file.md] | [exact text or short paraphrase] | ★★★/★★/★ |
```

**Rule:** Every factual claim in synthesis prose must appear in this register before the prose is written. If a claim cannot be placed in the register (no traceable source file), it must be written as a gap statement or uncertainty — not as a fact. Prose is written from this register, not from memory of prior phases.

1. **KIT Answer Register** — answer every KIT explicitly before doing anything else
2. **Triangulation** — build the Triangulation Matrix for all key findings, including the Numeric verified? column (Section 7)
3. **Contested Claims** — for any genuine expert/source disagreement, produce a Contested Claims Register using ACH
4. **Critical Findings** — 5–7 most decision-relevant findings, each with evidence and "So What?". Every finding must cite its source file.
5. **Confirmation Bias Check** — apply Devil's Advocate before producing deliverables:
   - What evidence contradicts the central narrative?
   - List at least 3 findings that cut against the primary thesis
   - Have competing hypotheses been considered for ambiguous findings?
6. **Expert QA** (Full Investigation tier) — route through relevant expert panels

**Step 6b: Fabrication Detection Pass**

After Expert QA and before deliverable generation, re-read the full draft synthesis as a sceptic. Apply the Fabrication Red Flags checklist (Section 17). For each flag triggered:
- If the claim can be sourced retroactively: add to synthesis-source-register.md and downrate confidence if needed
- If the claim cannot be sourced: rewrite as a gap statement or remove
- Log all triggered flags and their resolutions in a `FABRICATION AUDIT — [date]` block at the end of `synthesis.md`

**The Fabrication Detection Pass must complete before deliverable generation.** Any unresolved triggered flag blocks PDF generation — same gate as unresolved `[QA FLAG]` annotations.

7. **Deliverables** — generate outputs per skill-specific templates

**Step 7b: Numeric Quarantine**

After deliverables are drafted and before PDF generation, run the Numeric Verification Protocol (Section 7). Extract every numeric token from all deliverable files. For each:
- Trace to source file — exact match required
- Any `[NUMERIC GAP — not sourced]` flags must be resolved before PDF generation
- Numbers sourced only from the subject's own materials (SR source type) must be marked ★ and hedged: "per [subject]'s own reporting"

8. **Information Gaps Register** — list every significant gap with KIT impact and resolution path
9. **Monitoring Plan** — alerts, feeds, trigger events, refresh cadence
10. **Assumptions and Limitations** — temporal limitations, data limitations, source independence note

**PDF generation gate:** The PDF may not be generated until ALL of the following are complete:
- synthesis-source-register.md is complete (all claims have source entries)
- Fabrication Detection Pass complete with no unresolved flags
- Numeric Quarantine complete with no `[NUMERIC GAP]` flags
- All `[QA FLAG]` annotations resolved
- Triangulation Matrix Numeric verified? column complete with no `⚠`

**The Confirmation Bias Check (step 5) must come before deliverable generation (step 7).** Applying it after deliverables are built means it either goes unused or forces a rebuild.

---

## 9. Universal Deliverables

Every Full Investigation (Tier 3) produces these outputs regardless of skill:

| Deliverable | File | Description |
|---|---|---|
| KIT Answer Register | `00-summary/kit-answers.md` | Structured per-KIT answers |
| Critical Findings | `00-summary/critical-findings.md` | 5–7 most decision-relevant findings |
| Executive Summary | `00-summary/executive-summary.md` | Purpose-oriented, 1–2 pages |
| RAG Dashboard | `00-summary/rag-dashboard.md` | Traffic-light summary — always the first page |
| Triangulation Matrix | `00-summary/triangulation-matrix.md` | Source independence verification |
| Contested Claims | `00-summary/contested-claims.md` | If applicable |
| Information Gaps | `00-summary/information-gaps.md` | Gaps as intelligence |
| Monitoring Plan | `00-summary/monitoring-plan.md` | Alerts, feeds, triggers |
| Assumptions & Limitations | `00-summary/assumptions-limitations.md` | Temporal, data, source independence |
| Synthesis Source Register | `00-summary/synthesis-source-register.md` | Claim-to-source linkage map — required before any synthesis prose is written |
| Full Synthesis PDF | `[root]/full-synthesis-report-YYYY-MM-DD-HH:MM.pdf` | Final polished deliverable |

**PDF naming convention:** `full-synthesis-report-YYYY-MM-DD-HH:MM.pdf` — date and 24-hour time of investigation completion. The time component allows multiple runs on the same day to coexist.

Skill-specific deliverables (Market Landscape Brief, Policy Brief, Engagement Playbook, etc.) are defined in each skill's output templates.

---

## 10. Information Gaps Register

Gaps are intelligence. The absence of a source, or the failure to find data, is itself a finding — it tells us something about the subject's documentation, opacity, or contested status.

Every investigation produces an Information Gaps Register:

```markdown
# Information Gaps
**Subject:** [entity or topic]
**Date:** [YYYY-MM-DD]

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

[Reference inaccessible-sources.md at the root of the investigation folder]

---

## Interpretation

[1–2 paragraphs: what do these gaps, taken together, tell us? Is this a well-documented subject or an opaque one? Does opacity itself signal something?]
```

---

## 11. Monitoring Plan

Every Full Investigation produces a Monitoring Plan:

```markdown
# Monitoring Plan
**Subject:** [entity or topic]
**Date:** [YYYY-MM-DD]
**Recommended refresh:** [monthly / quarterly / on-trigger / one-off]

---

## Alerts to Set Up

| Alert term | Frequency | Why |
|---|---|---|
| "[exact subject phrase]" | Weekly | Core monitoring |
| "[key person or organisation]" | Weekly | Key actor monitoring |
| "[policy / regulation name] amendment OR update" | Weekly | Regulatory changes |

---

## Key Publication and Filing Feeds

| Source | What to watch for | How to monitor |
|---|---|---|
| [source] | [type of update] | [URL or subscribe link] |

---

## Trigger Events

Events that should prompt an immediate refresh:
- [Trigger 1]
- [Trigger 2]
- [Trigger 3]

---

## Refresh Cadence

**Next scheduled refresh:** [date, or "on trigger only"]
**Refresh scope:** [which sections need updating vs. which can be carried forward]
```

---

## 12. Assumptions and Limitations

Every Full Investigation documents its assumptions and limitations:

```markdown
# Assumptions and Limitations
**Subject:** [entity or topic]
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
| [e.g. "ACNC financials have 12–18 month lag"] | [Financial analysis] | [Medium — figures may not reflect current position] |
| [e.g. "LinkedIn data obtained without authentication"] | [People profiles] | [Low — confirmed via other sources] |

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

## 13. Research Log Format

Append to `research-log.md` throughout every investigation:

```markdown
## Session [YYYY-MM-DD]

### Tool check
[Output of pre-investigation tool check — paste actual results, do not assume]

### Wave [N] — [phases/modules covered]
- Source: [name/URL] | Layer used: [1/2/3/4] | Result: [summary or FAILED]
- Source: [name/URL] | Layer used: [1/2/3/4] | Result: [summary or FAILED]

### KIT progress
KIT 1: [answered / partial / unanswered] | KIT 2: [status] | KIT 3: [status]

### Wave summary posted: [yes/no]
```

---

## 14. Disambiguation — Routing Between Skills

The two intelligence skills are complementary and must route to each other at natural hand-off points.

### Subject classification table

| Subject type | Use |
|---|---|
| Named legal entity (company, charity, trust, government agency) | company-intel |
| Industry, market, or sector (not one specific company) | topic-intel |
| Policy, framework, standard, regulation | topic-intel |
| Technology category or product class | topic-intel |
| Geographic area or community | topic-intel |
| Trend, movement, or time-bounded phenomenon | topic-intel |
| Public figure (not a company they run) | topic-intel |
| Scientific or academic field | topic-intel |
| Ambiguous — could be company or topic | Show disambiguation prompt |

### Disambiguation prompt (show when ambiguous)

```
Is this research about the organisation itself, or the broader topic/landscape?

  C — The organisation [Name]       → company-intel (dossier, people, financials)
  T — The broader topic or sector   → topic-intel (market, policy, ecosystem)
  B — Both                          → company-intel first, then topic-intel for context

(C / T / B)
```

**If C:** Use company-intel.
**If T:** Use topic-intel.
**If B:** Run company-intel for the entity first. When reaching Phase 10 (Industry & Market), invoke topic-intel (Module MKT or POL) for sector-level work rather than running a lightweight market analysis inside company-intel.

### Natural hand-off points

- **company-intel → topic-intel:** Phase 10 (Industry & Market Context) — when the sector itself is a primary KIT, not just background context
- **topic-intel → company-intel:** Module MKT Phase M2 (Key Player Identification) — when a specific organisation warrants deep entity research
- **topic-intel → company-intel:** Module GEO Phase G2 (Anchor Institution Mapping) — when a named anchor institution warrants full entity research

---

## 15. Pre-Investigation Tool Check

Run once at the start of every investigation. Record actual results. Do not assume any tool is available:

```bash
python3 -c "import playwright" 2>/dev/null || echo "Playwright is not installed"
echo "ScrapingBee: ${SCRAPINGBEE_API_KEY:+configured}"
echo "BrightData:  ${BRIGHTDATA_API_KEY:+configured}"
echo "ZenRows:     ${ZENROWS_API_KEY:+configured}"
```

If the import fails, say Playwright is not installed and skip Layer 4. Log each affected source as a gap with that reason.

**Layer 4 protocol:** Layer 4 is opt in. Tell the user the source and the reason, then run the browser only after the user says yes for that source. The full Layer 4 execution protocol lives in `references/layer-4-playwright-protocol.md`. Read it before executing any Playwright scripts. It covers the opt-in gate, decision templates, operational security, and source logging format. Both company-intel and topic-intel reference this file.

Record results in `research-log.md`. Note any unavailable tools before proceeding. Unavailable tools affect which research paths are viable.

---

## 16. Ethical Boundaries

All intelligence work across both skills operates within these boundaries:

- All intelligence from publicly accessible sources only
- No misrepresentation of identity or purpose to access information
- No access to authenticated systems. A source behind a login is a gap unless the user reads it and pastes what they want used.
- Do not use stealth tools or session cookies. When the user approves Layer 4, Playwright runs as a normal browser that identifies as automation.
- Respect each site's terms of service and robots rules
- Privacy Act and applicable legislation apply throughout
- Person research covers public role only: career, public statements, professional record, institutional affiliations. Private life is out of scope.
- LinkedIn profile views notify the profile owner. Inform the user before viewing.
- For every Layer 4 (Playwright) attempt, tell the user the source and the reason. Run it only after a yes for that source. On a no, log the source as a gap with the reason.

---

## 17. Fabrication Red Flags Checklist

Apply this checklist during the Fabrication Detection Pass (Phase 11 Step 6b). A triggered flag requires sourcing or removal — not rewording.

**Applies to all modules and both skills.** Module-specific flags are noted where relevant.

| # | Flag | What to do when triggered |
|---|---|---|
| 1 | **Orphaned number** — a digit, percentage, currency amount, or date appears in synthesis with no entry in synthesis-source-register.md | Trace to source file or rewrite as gap statement |
| 2 | **Precision inflation** — synthesis is more specific than its source (source says "approximately $200M", synthesis says "$214M") | Revert to source's exact language |
| 3 | **[MKT] Market size without methodology** — a market size figure appears with no stated data source or methodology | Add source and methodology note, or downrate to ★ and flag as estimate |
| 4 | **[SCI] Citation without identifier** — an academic reference appears as "Author et al. (year)" with no DOI, URL, or Semantic Scholar ID from the research log | Remove or replace with a traceable citation |
| 5 | **[POL] Paraphrased provision** — a statutory obligation, threshold, or penalty is described in indirect speech without a section number | Replace with verbatim quote + section number, or flag as unverified |
| 6 | **Unsourced comparator** — a comparative claim ("larger than", "faster than", "below average") appears without a source for the comparison point | Source the comparator or rewrite as directional ("appears larger, though no benchmark data was found") |
| 7 | **Confidence inflation** — a source uses hedged language ("preliminary findings suggest") but synthesis uses assertive language ("evidence shows") | Revert to source's confidence level |
| 8 | **New proper noun** — a name, organisation, or entity appears in synthesis that does not appear in any research-phase file | Verify and add to source register, or remove |
| 9 | **[Multi-module] Untagged cross-module claim** — a finding from a secondary module appears in primary synthesis without a `[Source: module-X-synthesis.md]` tag | Add source tag or re-verify against original research-phase file |
| 10 | **[TRD] Unanchored start date** — a trend is described as beginning in a specific year with no document source for that date | Source the date or rewrite as "the trend became visible around [period]" |
| 11 | **Summary-only claim** — the executive summary contains a specific claim not found in any body section or KIT Answer Register | Trace to source or remove from summary |
| 12 | **Invented probability** — a forward-looking scenario contains a specific probability estimate ("70% likely") with no stated basis | Remove the figure; express as qualitative likelihood ("appears more likely than not") |

**How to run this checklist:**
1. Read the draft synthesis from start to finish as a sceptic, not as the author
2. For each sentence containing a factual claim: check against the synthesis-source-register.md
3. For each sentence containing a number: apply flags 1 and 2, plus any module-specific flag
4. Record outcomes in the `FABRICATION AUDIT — [date]` block in synthesis.md:

```markdown
## FABRICATION AUDIT — [YYYY-MM-DD]
Flags checked: [N flags from checklist]
Flags triggered: [N]
Resolutions:
  - Flag [N], [claim]: [sourced / rewritten / removed]
Status: CLEAR / ISSUES RESOLVED / ISSUES OUTSTANDING
```

**A status of ISSUES OUTSTANDING blocks PDF generation.**

---

## Maintenance

This document is the single source of truth for shared intelligence standards. When updating:

- Changes here apply to both company-intel and topic-intel immediately
- Do not duplicate these standards in the individual SKILL.md files — reference this document instead
- When a standard needs to be skill-specific (e.g. different wave thresholds), note the variation here rather than creating separate copies
- Review this document when either skill undergoes major revision
