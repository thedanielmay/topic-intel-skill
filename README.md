# topic-intel-skill

Deep, multi-source intelligence research on any non-company topic — markets, industries, policies, regulations, academic fields, technology categories, geographic communities, trends, and public figures.

## Install

```bash
npx skills add thedanielmay/topic-intel-skill@topic-intel -g
```

## What it does

A structured intelligence-gathering skill that performs deep, multi-source research on any non-company topic. Produces a comprehensive dossier with provenance-tracked findings, organised into a navigable folder structure.

**Triggers on:** `research [topic]`, `deep dive on [topic]`, `brief me on [topic]`, `landscape of [topic]`, `state of [topic]`, `market analysis of [topic]`, `intelligence on [topic]`, or any request to understand a non-company subject in depth.

**Does NOT cover:** named companies or organisations — use [company-intel](https://github.com/thedanielmay/company-intel-skill) for those.

## Topic types

| Code | Type | Examples |
|---|---|---|
| MKT | Market / Industry | "Australian textile recycling market", "B2B stationery sector" |
| POL | Policy / Regulatory | "Victorian Social Procurement Framework", "ISSB climate standards" |
| SCI | Academic / Scientific | "microplastics in marine environments", "social impact measurement" |
| TEC | Technology / Product | "circular economy ERP platforms", "AI tools for NFP fundraising" |
| GEO | Geographic / Community | "social enterprise ecosystem in inner Melbourne" |
| TRD | Trend / Event | "impact investing trends 2023-2025", "post-COVID supply chain shifts" |
| PRS | Person (public figure) | Research on a named public figure's career, views, record |

## Investigation tiers

1. **Quick Scan** — Core sources, key facts, 1-page briefing note. ~30 min.
2. **Standard** — Multi-section dossier + executive summary. ~2 hrs.
3. **Full Investigation** — Complete dossier + PDF synthesis report. ~4-6 hrs.

## What's included

- **7 topic type modules** with phase sets, source priorities, KIT defaults, and output templates
- **Expert panels** — 3 domain-specific expert personas per module for Phase 11 QA
- **OSINT techniques** — advanced search operators, Wayback Machine, social media signals, document metadata
- **Agent dispatch guidance** — wave-level parallelisation with briefing templates and handoff format
- **Expanded source register** — 279+ sources including FRED, World Bank, SSRN, OpenAlex, Brookings, RAND, Metaculus, and more
- **Skill integrations** — `fact-check-workflow`, `literature-review`, `document-skills:pdf`, `data-visualization`
- **Multi-module chaining** — protocol for investigations spanning multiple topic types
- **Refresh Investigation** — protocol for updating existing dossiers

## Key principles

- Scope before search — the Topic Scoping Document is confirmed before any research begins
- KITs drive everything — Key Intelligence Topics orient every phase
- Provenance on everything — every fact cites its source with a confidence rating (★★★/★★/★)
- Gaps are intelligence — interpret every gap, don't just list it
- Actionable outputs — the reader should know what to DO, not just what to THINK

## Reference files

All reference files are included in the skill package:

- `SKILL.md` — main skill file with all phases, modules, and protocols
- `topic-expert-panels.md` — 7 module expert sets (21 personas), 3 integration modes
- `topic-osint.md` — OSINT techniques scoped to topic research
- `topic-agent-dispatch.md` — wave-level parallelisation guidance
- `references/topic-sources.md` — source register by topic type with API endpoints
- `references/topic-outputs.md` — output templates for each topic type
- `references/topic-analytical-frameworks.md` — analytical frameworks per module

## Author

Daniel May — [thedanielmay.com](https://thedanielmay.com)
