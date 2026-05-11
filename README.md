# topic-intel skill

Deep, multi-source intelligence research on any non-company topic.

## What it researches

Markets, industries, policies, regulations, standards, technology categories, geographic communities, trends, movements, academic fields, and public figures.

## Seven topic-type modules

| Code | Type | Examples |
|---|---|---|
| MKT | Market / industry | "Australian textile recycling market" |
| POL | Policy / regulatory | "Victorian Social Procurement Framework" |
| SCI | Academic / scientific | "microplastics in marine environments" |
| TEC | Technology / product | "circular economy ERP platforms" |
| GEO | Geographic / community | "social enterprise ecosystem, inner Melbourne" |
| TRD | Trend / event | "impact investing in Australia, 2023-2025" |
| PRS | Person (public figure) | Research on a named public figure |

## v2.0.0 - Anti-Hallucination Protocol

- **Cite-Before-You-Write** - every synthesis claim mapped to a source file before prose is written
- **Numeric Quarantine** - every number verified against its source file before PDF generation
- **Fabrication Detection Pass** - 12-flag checklist applied to all synthesis output
- **SCI Citation Manifest** - academic citations require DOI/URL/identifier from research log
- **POL Verbatim Provision Rule** - statutory provisions quoted verbatim with section numbers
- **Module Contamination Firewall** - cross-module claims tagged with source file references

## Install

```bash
npx skills add thedanielmay/topic-intel-skill
```

## Features

- Three investigation tiers: Quick Scan, Standard, Full Investigation
- Five-wave parallel research execution with agent dispatch templates
- Module-specific expert panels (3 experts x 7 modules) with 3 integration modes
- PDF synthesis report with timestamped filename
- Refresh Investigation protocol
- Integrates: `fact-check-workflow`, `literature-review`, `document-skills:pdf`, `data-visualization`, `expert-panel`
- Shared standards with company-intel via `INTELLIGENCE-STANDARDS.md`
