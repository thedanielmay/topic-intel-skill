---
name: topic-intel
description: "Deep, multi-source intelligence research on a named non-company topic: a market, industry, policy or regulation, technology, trend, community, academic field or public figure. Use this skill when the user asks for research, due diligence, a dossier, a deep dive, a brief or intelligence on a named subject, or names this skill. Do NOT use for research on a specific named company or organisation. Use company-intel instead. If the subject is ambiguous between a company and a topic, show the disambiguation prompt."
license: MIT
metadata:
  version: "2.6.0"
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

## Dependency Check

Before doing anything else, check whether the following skills appear in the available skill list for this session:

- `fact-check-workflow`
- `literature-review`
- `document-skills:pdf`
- `data-visualization`
- `expert-panel`

If ANY are missing, say:

> "topic-intel works best with its full skill set. The following skills are missing from this session:
> [list the missing ones]
>
> Install them using either method below, then restart your session:
>
> **`fact-check-workflow`**
> - `npx skills add jamditis/claude-skills-journalism@fact-check-workflow` (third-party repo)
>
> **`literature-review`**
> - `npx skills add eyadsibai/ltk@literature-review` (third-party repo)
>
> **`document-skills:pdf`**
> - `npx skills add anthropics/skills@pdf`
> - In Claude Code: `/plugin marketplace add anthropics/skills`, then `/plugin install document-skills@anthropic-agent-skills`
>
> **`data-visualization`**
> - `npx skills add anthropics/knowledge-work-plugins@data-visualization`
>
> **`expert-panel`**
> - `npx skills add thedanielmay/expert-panel-skill`
> - `/plugin install expert-panel@thedanielmay`
>
> Or type **'continue anyway'** to proceed — steps that need missing skills will be skipped."

If the user types 'continue anyway', note which skills are unavailable and proceed. When a missing skill would normally be invoked, skip that step and note it inline: "(skipped — `literature-review` not available)".

If all skills are present, proceed silently without mentioning the check.

---

**Reference files. Read each one at the time named below.**

- `references/INTELLIGENCE-STANDARDS.md`: read first, at the start of every investigation. It holds the STOP gate, scoping document, KIT register, confidence ratings, saturation rule, wave rules, progress reporting, synthesis sequence, deliverables, disambiguation routing, tool check and ethical boundaries.
- `references/web-access-layers.md`: read at the start of every investigation. It describes the four web access layers.
- `references/layer-4-playwright-protocol.md`: read when Layer 4 is triggered.
- `references/topic-sources.md`: read when identifying sources. It holds the source register by topic type, with API endpoints and access notes.
- `references/topic-outputs.md`: read when producing outputs. It holds the output templates for each topic type.
- `references/topic-analytical-frameworks.md`: read during analysis. It holds the analytical frameworks for topic research.
- `references/topic-modules.md`: read after classifying the topic type. It holds the KIT defaults, phase sets, sources, output formats and chaining protocol for all seven modules.
- `references/topic-synthesis.md`: read at Phase 11. It holds the full synthesis, expert QA and deliverable procedure.
- `topic-expert-panels.md`: read at Phase 11 QA. It holds seven module-specific expert sets and three integration modes.
- `topic-osint.md`: read when using OSINT. It holds search operators, Wayback techniques, social media signals and document metadata methods.
- `topic-agent-dispatch.md`: read when dispatching parallel agents. It holds briefing templates, handoff format, per-wave notes and timeout fallback.

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

Full routing logic and natural hand-off points between skills are documented in `references/INTELLIGENCE-STANDARDS.md` Section 14.

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

If the user named a depth in the request, use that depth and skip this step.

```
INVESTIGATION DEPTH
───────────────────
1. Quick Scan:          Core sources, key facts, recent developments. ~30 min.
                        Produces: 1-page briefing note.
2. Standard:            All of Quick Scan + stakeholder map, data sources,
                        analysis, synthesis. ~2 hrs.
                        Produces: Multi-section dossier + executive summary.
3. Full Investigation:  Everything. All sources, cross-reference, expert QA
                        review, PDF synthesis report. ~4 to 6 hrs.
                        Produces: Complete dossier + PDF.

Which depth? (1 to 3; suggested: 1 Quick Scan)
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

Read `references/web-access-layers.md` for the full protocol.

**Summary:**
- **Layer 1 — API-first:** Use public APIs where available (ABS, legislation.gov.au, Semantic Scholar, CrossRef, ACNC, etc.)
- **Layer 2 — Search as router:** Use WebSearch to find the exact URL, then fetch only that URL. Never crawl blind.
- **Layer 3 — Scraping service:** If WebFetch fails, route through ScrapingBee/ZenRows/BrightData if configured.
- **Layer 4 - Playwright:** Opt-in only. Run it for a named source after the user says yes for that source. It is not an automatic fallback.

**Layer 4 opt-in rule:** If Layers 1, 2, and 3 have failed or are inapplicable, and the source has interactive or JS-rendered content, tell the user the exact source and why Layer 4 is needed. Run Layer 4 only after the user says yes for that source. If the user says no, or Playwright is not installed, log the source as a gap with that reason in `inaccessible-sources.md` and move on. Layer 4 runs as a normal browser that identifies as automation. LinkedIn is not a Layer 4 target. Use only LinkedIn content that is publicly visible without login through Layers 1 to 3, or content the user pastes. Read `references/layer-4-playwright-protocol.md` for the execution protocol and source-logging format.

**Pre-investigation tool check** (run once at the start of every investigation):
```bash
python3 -c "import playwright" 2>/dev/null && echo "playwright ok" || echo "playwright NOT installed; skip Layer 4"
echo "ScrapingBee: ${SCRAPINGBEE_API_KEY:+configured}"
echo "BrightData:  ${BRIGHTDATA_API_KEY:+configured}"
echo "ZenRows:     ${ZENROWS_API_KEY:+configured}"
```

Run the check and record the actual results in `research-log.md`. Note any tools that are unavailable before proceeding.

If the Playwright check fails, say Playwright is not installed, skip Layer 4 for this investigation, and log each affected source as a gap with that reason.

See `references/INTELLIGENCE-STANDARDS.md` Section 15 for the standard tool check format.

---

## Confidence Rating System

See `references/INTELLIGENCE-STANDARDS.md` Section 4 for the full grading system and source independence rule. Apply two grades to every piece of information recorded: source reliability A to F and information credibility 1 to 6, written together (for example B2). Keep the ⚠ flag for claims contradicted by another source.

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
- Apply tier effort caps: Tier 1 = Layer 2 max; Tier 2 = Layer 3 max; Tier 3 = Layer 3 by default, with Layer 4 only after the user approves that source
- After each agent completes, it returns a structured research-log block in its final message. The orchestrator appends the block to `research-log.md` (see `topic-agent-dispatch.md`)
- Sequential execution remains valid — dispatch is a performance optimisation, not a requirement

See `topic-agent-dispatch.md` for full guidance: agent briefing templates, per-wave notes, handoff format, and timeout fallback. See `superpowers:dispatching-parallel-agents` skill for Claude Code implementation.

**Wave 2 minimum threshold:** If primary source identification returns fewer than 3 substantive sources for any KIT, pause and report to the user before proceeding to Wave 3. Do not proceed to synthesis on a thin evidence base.

See `references/INTELLIGENCE-STANDARDS.md` Section 6 for the full wave execution rules.

---

## Source Saturation Rule

See `references/INTELLIGENCE-STANDARDS.md` Section 5 for the source saturation rule. Module SCI's additional threshold (≥50% overlap) applies to academic searches and takes precedence over the global rule.

---

## Topic Type Modules

Select the module matching the classified topic type and follow its guidance throughout the investigation. Read `references/topic-modules.md` for all seven modules, their KIT defaults, phase sets, sources, output formats and the multi-module chaining protocol.

---

## Phase 11: Synthesis and Expert Review

Every KIT must be answered explicitly. Read `references/topic-synthesis.md` for the full synthesis procedure, expert QA, deliverables and PDF generation rules.

---

## Progress Reporting

After completing each wave, post a wave summary before beginning the next wave — this is required, not optional. See `references/INTELLIGENCE-STANDARDS.md` Section 6 for the standard wave summary format and Wave 2 minimum threshold rules.

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
│   ├── assumptions-limitations.md
│   └── synthesis-source-register.md          ← claim-to-source linkage map (required before synthesis prose)
├── 01-primary-sources/                           ← Official documents, legislation, data
├── 02-media-coverage/                            ← News, interviews, podcasts, video
├── 03-stakeholders-actors/                       ← People, organisations, networks
├── 04-data-statistics/                           ← Quantitative data, datasets
├── 05-analysis/                                  ← Frameworks, models, synthesis drafts
│   ├── sci-citation-manifest.md              ← SCI module only: citation identifier verification
│   ├── synthesis-[MODULE-CODE]-primary.md    ← module synthesis before Phase 11 integration
│   └── synthesis-[MODULE-CODE]-secondary.md  ← secondary module synthesis files (if chaining)
├── inaccessible-sources.md                       ← Sources that failed all four layers
└── research-log.md                               ← Running log of all research activity
```

Every subfolder gets an `INDEX.md` cataloguing its contents with source, date, confidence rating, and summary.

---

## Research Log Format

Append to `research-log.md` throughout the investigation. See `references/INTELLIGENCE-STANDARDS.md` Section 13 for the standard research log format.

---

## Key Principles

1. **Scope before search.** The Topic Scoping Document is confirmed before any research begins.
2. **KITs drive everything.** Every phase asks: what does this tell us about each KIT?
3. **Provenance on everything.** Every fact cites its source with a confidence rating.
4. **Source independence.** Two organisations citing the same government report = one source.
5. **Gaps are intelligence.** Interpret every gap — don't just list it.
6. **Contested knowledge.** Where genuine expert disagreement exists, surface it — don't resolve it artificially.
7. **Layer 4 is opt-in.** If Layers 1-3 fail, name the source and why, then run Playwright only after the user says yes for that source. Log a gap when the user says no or Playwright is not installed.
8. **Ethical boundaries.** All intelligence comes from publicly accessible sources. Respect each site's terms and robots rules. Privacy Act applies. Person research covers public role only.
9. **Actionable outputs.** The reader should know what to DO, not just what to THINK.
10. **Single source of truth.** Standards shared with company-intel (STOP gate, KIT register, saturation rule, wave thresholds, progress reporting, synthesis sequence, deliverables, disambiguation) are defined in `references/INTELLIGENCE-STANDARDS.md`. Do not duplicate them here — reference them.
