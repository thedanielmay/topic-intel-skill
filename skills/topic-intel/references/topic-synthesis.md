# Phase 11 Synthesis and Expert Review

This file holds the full synthesis and expert review procedure.
The parent SKILL.md points here when the investigation reaches Phase 11.

## Phase 11: Synthesis & Expert Review

**Goal:** Transform research into actionable intelligence. Every KIT must be explicitly answered.

### Step 1: KIT Answer Register

For each KIT, produce a structured answer:
```
KIT [N]: [question]
Answer: [direct answer - 2-5 sentences]
Confidence: [★★★/★★/★]
Key sources: [2-3 most authoritative sources used]
Gaps: [what would improve confidence if found]
```

**If KIT answers point in contradictory directions** (e.g. market is large but barriers are prohibitive, or policy is clear but implementation is absent), surface this tension explicitly in the Executive Summary. Do not resolve it artificially or weight one KIT's answer over another. Contradictory KIT answers are often the most important finding.

See `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md` Section 3 for the full KIT Answer Register format and KIT conflict rule.

### Step 2: Triangulation

Build a Triangulation Matrix per `topic-analytical-frameworks.md` - use the template and source type codes defined there. Apply to all key findings. The source type codes (PR/SI/SD/SR) and universal triangulation matrix format are also documented in `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md` Section 7.

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
- A Rating Decision Template that documents supporting evidence, contradicting evidence, missing context, and confidence level - use this as the structured record for each ACH verdict
- A graduated 6-point rating scale (True / Mostly True / Half True / Mostly False / False / Pants on Fire) for use when the contested claim will be cited in a published deliverable
- Right-of-response protocol: when the contested claim involves a living public figure (PRS module only), the `fact-check-workflow` source contact template applies

**Fallback (if `fact-check-workflow` not installed):** Use the ACH template in `topic-analytical-frameworks.md` directly.

### Step 4: Critical Findings

List the 5-7 most decision-relevant findings. For each:
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

**Mode 1 - Synthesis Reviewer (always invoke for Full Investigations):**
1. Read `../topic-expert-panels.md` and select the expert set matching the topic module
2. Select the 2-3 experts most relevant to the primary KITs
3. For each expert, apply their key questions to the draft synthesis and flag any gaps as `[QA FLAG: Expert Name - issue]`
4. Resolve all `[QA FLAG]` annotations before proceeding to deliverable generation

**Mode 3 - Pre-Output QA (invoke before PDF generation):**
Run all three experts in the module set across the complete dossier. Any unresolved `[QA FLAG]` blocks the PDF generation step.

**On-demand adversarial review:** If the user has requested a full expert panel challenge, invoke the `expert-panel` skill for an interactive multi-round panel session using the personas from `../topic-expert-panels.md` as the starting point for expert selection.

**Fallback (if `expert-panel` skill not installed for on-demand):** Run the Mode 1 embedded review only using the personas from `../topic-expert-panels.md` - no dependency on external skill.

See `../topic-expert-panels.md` for all 7 module expert sets and full integration mode definitions.

### Step 6: Confirmation Bias Check

Before finalising, apply the Devil's Advocate check:
- What evidence contradicts the central narrative?
- List at least 3 findings that cut against the primary thesis
- Have competing hypotheses been considered for ambiguous findings?

### Step 7: Monitoring Plan

For each investigation, produce a monitoring plan:
- Google Alerts terms to set up
- Key publication feeds or regulatory update subscriptions
- Trigger events that prompt a refresh
- Recommended refresh cadence (monthly / quarterly / on-trigger)

### Step 8: Deliverable

The following are the universal deliverables required for all Full Investigations (see `~/.claude/skills/shared/references/INTELLIGENCE-STANDARDS.md` Section 9 for templates):

**Always produce:**
- KIT Answer Register
- Critical Findings
- Executive Summary (purpose-oriented, 1-2 pages)
- Triangulation Matrix
- Contested Claims Register (if applicable)
- Information Gaps
- Monitoring Plan
- Assumptions and Limitations
- RAG Dashboard (per `topic-outputs.md` Section 3 - use the template matching the topic type)

**Produce based on topic type:**
- MKT: Market landscape brief + opportunity map
- POL: Policy brief + compliance timeline
- SCI: Evidence landscape map + research gap analysis
- TEC: Vendor comparison matrix + recommendation table
- GEO: Ecosystem map + stakeholder network
- TRD: Trend timeline table + forward-looking assessment
- PRS: Biographical intelligence brief

**PDF:** For Full Investigation (Tier 3), compile all outputs into a PDF at the root of the topic folder, named `full-synthesis-report-YYYY-MM-DD-HH:MM.pdf`.

**Generating the PDF (Full Investigation - Tier 3):**
Once all `00-summary/` files are written and all `[QA FLAG]` annotations from Mode 3 expert QA are resolved, invoke `document-skills:pdf` to compile the full synthesis report:
- Input: all files in `00-summary/` plus any type-specific deliverable files
- Output: `full-synthesis-report-YYYY-MM-DD-HH:MM.pdf` at the root of the `_topic-{slug}/` folder
- Naming: use 24-hour time at moment of generation

**Fallback (if `document-skills:pdf` not installed):** Note in research log; deliver markdown files only. Add to `inaccessible-sources.md`: "PDF generation - document-skills:pdf not installed."

**Generating visualisations (MKT, TEC, TRD, GEO modules only):**
After producing markdown tables for comparison matrices, trend timelines, opportunity maps, or ecosystem diagrams, invoke `data-visualization` to produce visual outputs. Invoke once per deliverable type that contains tabular data:
- MKT: opportunity map, vendor/player comparison table
- TEC: vendor comparison matrix, capability map
- TRD: trend timeline, driver analysis table
- GEO: ecosystem map, funder landscape table

Do NOT invoke `data-visualization` for POL, SCI, or PRS modules - tables and text are more appropriate for legal, academic, and biographical outputs.

**Fallback (if `data-visualization` not installed):** Deliver markdown tables only.

---
