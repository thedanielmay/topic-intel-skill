# Topic Agent Dispatch — Wave Parallelisation Guide

## Purpose

Guidance for parallelising topic-intel investigation waves using agents. Using agents converts the wave structure from aspirational parallelism (phases described as parallelisable) to actual parallelism (phases executing simultaneously).

Agent dispatch is a performance optimisation, not a requirement. Sequential execution remains valid — all waves can be run in order without agents. Use this guide when investigation scope and available resources make parallel execution worthwhile.

**Reference:** See `superpowers:dispatching-parallel-agents` skill for Claude Code implementation details.

---

## Agent Briefing — Minimum Required Contents

Every agent dispatched from a topic-intel investigation must receive all of the following in its briefing prompt:

1. **Confirmed Topic Scoping Document** — full text of `00-summary/topic-scoping.md`
2. **KIT list with current status** — each KIT labelled: answered / partial / unanswered
3. **Research log excerpt** — sources accessed so far (prevents duplication of effort)
4. **Specific task for this agent** — named source targets, what to look for, what format to return
5. **Output instruction** — what to write back and in what format (see Handoff Format below)

Do not dispatch agents without the scoping document and research log — agents without context will duplicate work and miss KIT-specific angles.

---

## Handoff Format

Each agent writes a structured block to `research-log.md` when complete:

```markdown
## Agent task: [brief description] — [YYYY-MM-DD HH:MM]
Sources accessed: [N]
Layers used: [1/2/3/4]
KIT progress: KIT 1 [answered/partial/unanswered] | KIT 2 [...] | KIT 3 [...]
Key findings: [2-3 sentences summarising the most significant findings]
Gaps logged: [N — reference inaccessible-sources.md for details]
New sources to follow up: [any URLs or source names surfaced but not yet accessed]
```

---

## Wave-Level Parallelisation Notes

### Wave 1 — Foundation (do NOT parallelise)

Wave 1 must complete before any agents are dispatched. It confirms the topic, establishes KITs, and produces the scoping document that all agents need in their briefing. Running agents before the scoping document is confirmed is the most common failure mode in parallelised investigations.

### Wave 2 — Primary Research (parallelise across KITs and source types)

**Parallelisable tasks:**
- Primary source identification — one agent per KIT
- Stakeholder/actor mapping — one agent
- Data and statistics search — one agent

**Recommended agent type:** Explore (read-only source discovery)

**Per-agent brief structure:**
- Agent A (KIT 1 primary sources): "Search the following sources for evidence relevant to KIT 1: [source list]. Record every source accessed, confidence rating, and key finding."
- Agent B (KIT 2 primary sources): same structure for KIT 2
- Agent C (stakeholder mapping): "Identify all significant organisations, individuals, and networks relevant to [topic]. Sources: [source list]. Map relationships where evident."
- Agent D (data/statistics): "Find quantitative data relevant to [topic]. Priority: official government statistics, then industry data, then secondary estimates. Record source, date, methodology where stated."

**Wave 2 minimum threshold gate:** If any agent returns fewer than 3 substantive sources for its assigned KIT, pause and report to the user before proceeding to Wave 3. Do not build synthesis on a thin evidence base.

### Wave 3 — Secondary Research (parallelise by source type)

**Parallelisable tasks:**
- Media and community sources — one agent
- Policy/regulatory/technical depth — one agent (module-specific)
- Cross-reference — one agent

**Recommended agent type:** Explore

**Per-agent brief structure:**
- Agent E (media/community): "Search news archives, trade press, and community sources for coverage of [topic] from [date range]. Focus on: [specific angles from KIT list]."
- Agent F (policy/technical depth): "Conduct depth research on [specific policy/technical aspect identified in Wave 2]. Primary sources first, then secondary. Access via [layer guidance based on tier]."
- Agent G (cross-reference): "Cross-reference the following Wave 2 findings against independent sources. Identify any contradictions or where a single original source is being cited multiple times."

### Wave 4 — Analysis (parallelise by framework)

**Parallelisable tasks:**
- Analytical framework application — one agent per major framework required by the module
- Synthesis drafts — one agent per major deliverable section

**Recommended agent type:** General-purpose (analysis and writing)

**Per-agent brief structure:**
- Agent H (framework): "Apply [framework name] to the following evidence set. Template: [paste relevant framework from topic-analytical-frameworks.md]. Evidence: [paste relevant research log sections]."
- Agent I (synthesis draft): "Draft [KIT Answer Register / Critical Findings / Executive Summary] using the evidence in the research log. Template: [paste relevant section from topic-outputs.md]."

### Wave 5 — Synthesis and QA (partial parallelisation)

**Partially parallelisable:**
- Expert QA review — one agent per expert persona (Mode 3 pre-output QA)
- Deliverable file generation — one agent per output file

**Sequential gate:** Expert QA (Mode 3) must complete before PDF generation. Expert `[QA FLAG]` annotations must be resolved before invoking `document-skills:pdf`.

**Recommended agent type:** General-purpose

**Per-agent brief structure:**
- Agent J (Expert QA — Expert 1): "Review the attached synthesis draft as [Expert Name, role]. Identify gaps, weak analysis, or missing intelligence in your domain. Flag issues as [QA FLAG: description]. Template: [paste expert profile from topic-expert-panels.md]."
- Agent K (deliverable generation): "Write the [specific deliverable] to [file path] using this template: [paste template from topic-outputs.md]. Evidence: [paste relevant synthesis sections]."

---

## Timeout and Fallback

- **Standard research agents (Waves 2-3):** Proceed after reasonable wait. Log incomplete tasks in research log with note: "Agent timed out — [task] incomplete."
- **Analysis agents (Wave 4):** If an agent does not return, run that framework sequentially in the main session.
- **QA agents (Wave 5):** Do not skip QA. If an agent times out, run the expert review sequentially using the persona from `topic-expert-panels.md`.

Sequential execution is always a valid fallback. The wave structure works without agents — dispatch is a performance optimisation, not a dependency.

---

## Source Effort Caps by Tier

Control which web access layers agents may use based on investigation tier. Brief agents with their tier constraint:

| Tier | Max layer | Playwright (Layer 4) | Scraping service (Layer 3) |
|---|---|---|---|
| Tier 1 — Quick Scan | Layer 2 | Never | Never |
| Tier 2 — Standard | Layer 3 | Never | Yes, if configured |
| Tier 3 — Full Investigation | Layer 4 | Yes | Yes |

Include the tier constraint in every agent briefing: "This is a Tier [N] investigation. Do not use Layer [N+1] or above."
