# Topic Expert Panels

## Purpose

Provides embedded QA expert personas for Phase 11 synthesis review. Distinct from the interactive `expert-panel` skill — these are structured reviewers applied during synthesis, not a full adversarial panel session.

Read this file at the start of Phase 11 (Synthesis) for every Full Investigation (Tier 3).

---

## Integration Modes

### Mode 1 — Synthesis Reviewer (default, always invoke)

During Phase 11, before generating any deliverable, route the draft synthesis through the 2-3 most relevant experts for the topic type. Each expert reviews their domain and flags gaps, weak analysis, or missing intelligence. Select experts based on the investigation's primary KITs.

**When:** Always, during Phase 11 synthesis. Select experts matching the topic module.

### Mode 2 — On-Demand Consultant

Available during any research phase when a phase produces unexpected findings, thin results, or contradictory information in a specific expert's domain. Invoke by asking: "What would [Expert Name] look for here?"

**When:** When a phase produces unexpected, thin, or contradictory results in a specific domain.

### Mode 3 — Pre-Output QA (Full Investigation only)

Before generating the final PDF synthesis, route the complete dossier through ALL three experts for the topic type. Each reviews the full output from their perspective and flags critical omissions as `[QA FLAG]` annotations.

**When:** Always, as the final step before invoking `document-skills:pdf`. Reviews outputs only — does not re-run research phases.

---

## Expert Sets by Module

### MKT — Market / Industry

**Expert 1: Market Economist**
- Persona: Senior economist, 15+ years in market analysis at institutions such as the Productivity Commission, RBA, or a Big 4 economics practice
- Domain: Market sizing, growth trajectories, competitive dynamics, revenue model sustainability
- Key questions:
  - "What is the primary evidence for the market size figure — is it primary data or a cited estimate of an estimate?"
  - "Have substitutes been properly accounted for in the competitive analysis?"
  - "Is the revenue model analysis based on publicly disclosed financials or inference?"
  - "What is the confidence interval on the growth rate — is this a point estimate presented as fact?"
  - "Does the opportunity map account for barriers to entry, or does it treat the market as open?"
- Will challenge: Data sources for market sizing, conflation of correlation with causation in driver analysis, revenue model viability claims without financial evidence
- Invoke in Mode 2 when: Market size or growth data is from a single source, or revenue model conclusions feel unsupported

**Expert 2: Industry Analyst**
- Persona: Senior industry analyst, 12+ years covering Australian markets at a research firm or trade association
- Domain: Competitive landscape, key player identification, industry structure, sector-specific dynamics
- Key questions:
  - "Are the key players identified by revenue/scale, or by prominence in search results — these often differ?"
  - "Has market concentration been quantified — is this a fragmented or concentrated market and does the analysis say so?"
  - "Are barriers to entry assessed from the perspective of a new entrant, or assumed from current player behaviour?"
  - "Have the relationships between players (supplier, customer, competitor, partner) been mapped?"
  - "Is there a distinction between organisations that are in the market and organisations that are adjacent to it?"
- Will challenge: Incomplete player mapping, conflation of market adjacency with market participation, assumed barriers without evidence
- Invoke in Mode 2 when: Player identification phase returns fewer than 5 named organisations, or competitive dynamics analysis is thin

**Expert 3: Regulatory Economist**
- Persona: Regulatory economist or policy advisor, 10+ years working at or advising government regulators (ACCC, ASIC, sector-specific)
- Domain: Regulatory environment, policy drivers, government program impacts on market structure
- Key questions:
  - "Is the regulatory environment described from primary legislative sources, or from industry commentary about those sources?"
  - "Have upcoming regulatory changes (in consultation, foreshadowed) been distinguished from current requirements?"
  - "Has government procurement spend been checked via AusTender — this is often a significant market driver not captured in commercial sources?"
  - "Are the compliance costs estimated or sourced — and from whose perspective?"
  - "Does the analysis distinguish between federal and state regulatory environments?"
- Will challenge: Regulatory analysis sourced from industry advocacy rather than primary instruments, missing government procurement angle, conflation of proposed and enacted policy
- Invoke in Mode 2 when: Regulatory environment section relies primarily on industry body commentary rather than primary legislative sources

---

### POL — Policy / Regulatory

**Expert 1: Constitutional and Administrative Lawyer**
- Persona: Senior partner or academic specialising in administrative law and regulatory compliance, 15+ years Australian practice
- Domain: Legislative basis, source authority hierarchy, legal validity of policy instruments
- Key questions:
  - "Has the primary enabling legislation been identified and confirmed as current — not a superseded version?"
  - "Is the instrument being analysed delegated legislation, ministerial guidance, or non-binding policy — this affects its legal force?"
  - "Have all amendments since original commencement been captured?"
  - "Is the compliance obligation derived from the Act itself or from subordinate instruments — and have those instruments been checked?"
  - "Have any successful legal challenges to this policy been found via AustLII?"
- Will challenge: Policy brief built from guidance documents rather than primary legislation, missing source authority hierarchy, unconfirmed compliance obligations
- Invoke in Mode 2 when: Primary legislative instrument has not been directly accessed, or obligations are described from agency guidance only

**Expert 2: Policy Implementation Specialist**
- Persona: Senior public servant or implementation advisor, 12+ years across federal and state government program delivery
- Domain: Implementation landscape, compliance in practice vs. on paper, audit findings, operational gaps
- Key questions:
  - "Has an audit body (VAGO, ANAO) reviewed implementation of this policy — and if so, what did they find?"
  - "What is the reported compliance rate, and how is compliance measured?"
  - "Are there known implementation failures or gaps documented in parliamentary committee reports or submissions?"
  - "Has the policy been amended in response to implementation problems — and is that history captured?"
  - "What are the enforcement consequences documented in actual cases, not just the stated maximum penalties?"
- Will challenge: Policy described as implemented when audits show significant gaps, compliance described from policy text rather than evidence of practice
- Invoke in Mode 2 when: Implementation landscape phase (P4) returned thin results or only positive government sources

**Expert 3: Stakeholder Engagement Specialist**
- Persona: Senior stakeholder analyst or advocacy strategist, 10+ years working on complex policy consultations
- Domain: Stakeholder positions, advocacy landscape, who wins and loses from the policy
- Key questions:
  - "Have the parliamentary submission databases been searched for stakeholder positions — not just the final guidance?"
  - "Are there organised interests opposing this policy that are not represented in the current analysis?"
  - "Have the indirect costs and benefits been identified — who bears costs that are not captured in the compliance framework?"
  - "Is the timeline of stakeholder engagement captured — what was the consultation process and who participated?"
  - "Have peak bodies, unions, and consumer groups been distinguished from individual organisations in the stakeholder map?"
- Will challenge: Stakeholder map that only includes supporters, missing organised opposition, no analysis of distributional effects
- Invoke in Mode 2 when: Stakeholder positions section is thin or sources only government and industry peak body positions

---

### SCI — Academic / Scientific

**Expert 1: Domain Scientist**
- Persona: Senior researcher or professor with 15+ years in the specific field being investigated
- Domain: Substantive findings, consensus vs. contested claims, direction of research
- Key questions:
  - "Does the literature review include the most-cited papers in this field — are the foundational works captured?"
  - "Have the contested claims been identified with the competing positions named — not just noted as 'mixed evidence'?"
  - "Is the field's consensus accurately characterised, or has a minority position been given equal weight?"
  - "Have pre-prints been distinguished from peer-reviewed publications in the confidence ratings?"
  - "Are there known replication failures or retracted papers in this area that affect the evidence picture?"
- Will challenge: Overstatement of consensus, undifferentiated treatment of pre-prints and peer-reviewed work, missing foundational citations
- Invoke in Mode 2 when: Literature search returns fewer than 10 papers for a mature field, or contested claims are described without naming the competing positions

**Expert 2: Research Methodologist / Statistician**
- Persona: Senior methodologist, 12+ years reviewing research designs and statistical approaches across disciplines
- Domain: Study design quality, statistical validity, evidence hierarchy application, bias identification
- Key questions:
  - "Has the evidence hierarchy been correctly applied — are observational studies being treated with appropriate caution relative to RCTs?"
  - "Have sample sizes been noted — are the cited studies adequately powered for their conclusions?"
  - "Is publication bias acknowledged — is there a file drawer problem in this literature?"
  - "Are causal claims distinguished from correlational findings?"
  - "Have the methodological limitations noted by the authors themselves been captured?"
- Will challenge: Conflation of association with causation, treating small-sample studies as definitive, applying confidence ratings without reference to study design quality
- Invoke in Mode 2 when: Critical Findings section makes causal claims or presents correlational studies as evidence of effect

**Expert 3: Research-to-Practice Practitioner**
- Persona: Senior practitioner (clinician, engineer, policy officer, educator) who applies research in their field, 10+ years bridging research and practice
- Domain: Practical applicability, implementation barriers, gap between research findings and real-world use
- Key questions:
  - "Do the practical application recommendations follow from the evidence, or are they inferred beyond what the studies support?"
  - "Are the implementation barriers to applying this research in practice identified?"
  - "Is there a gap between what the literature finds and what practitioners actually do — and is that gap documented?"
  - "Have the conditions required for findings to hold been stated — does this evidence generalise to the user's context?"
  - "Are the research gaps identified by the investigators themselves captured alongside the gaps identified by this investigation?"
- Will challenge: Research-to-practice leap that overstates applicability, missing implementation conditions, practical gap not acknowledged
- Invoke in Mode 2 when: Practical Applications section (S7) draws conclusions not directly supported by cited studies

---

### TEC — Technology / Product

**Expert 1: Software Architect**
- Persona: Principal or Staff Engineer with 15+ years building and evaluating enterprise software
- Domain: Technical capability assessment, integration complexity, architecture patterns, build/buy/partner decisions
- Key questions:
  - "Have the integration requirements been assessed from primary technical documentation — not just marketing claims?"
  - "Is the vendor comparison matrix based on documented capabilities or assumed from product naming?"
  - "Have the security and compliance implications of each solution been assessed?"
  - "Is the maturity of each solution's API documented — REST/GraphQL, versioning, rate limits?"
  - "Has the total cost of ownership been estimated across 3 years, not just headline licence cost?"
- Will challenge: Vendor comparison built from marketing pages rather than technical documentation or user reviews, missing integration complexity assessment
- Invoke in Mode 2 when: Vendor comparison matrix is sourced primarily from vendor websites rather than G2/Capterra/independent reviews

**Expert 2: Enterprise Procurement Specialist**
- Persona: Senior procurement officer or technology buyer, 12+ years evaluating and contracting enterprise software
- Domain: Pricing models, negotiation signals, vendor viability, procurement risk
- Key questions:
  - "Has pricing been verified against published pricing pages or user-reported prices on G2/Capterra — not just 'contact sales'?"
  - "Has vendor financial stability been checked — funding status, burn rate signals, acquisition history?"
  - "Are the contract terms known — minimum commitment periods, exit clauses, data portability?"
  - "Have the hidden costs been identified — implementation, training, integration, ongoing support?"
  - "Is there evidence of Australian or sector-specific adoption, or is this only international evidence?"
- Will challenge: Pricing described as unknown without attempting to find user-reported prices, missing vendor viability assessment, Australian/sector adoption not verified
- Invoke in Mode 2 when: Vendor Viability Assessment (T6) is thin or Pricing Summary contains multiple "contact sales" entries without user-reported data

**Expert 3: Security and Privacy Expert**
- Persona: Senior security architect or privacy advisor, 10+ years evaluating software for enterprise security and regulatory compliance
- Domain: Data handling practices, privacy compliance, security architecture, regulatory fit
- Key questions:
  - "Has data residency been confirmed for each vendor — where is data stored and processed?"
  - "Have the privacy compliance certifications been verified (ISO 27001, SOC 2, GDPR, Privacy Act 1988)?"
  - "Is there a known breach or security incident history for any vendor?"
  - "Are the access control and authentication capabilities documented?"
  - "Has the regulatory fit for the user's context been assessed — does this solution meet their compliance obligations?"
- Will challenge: Vendor recommendations without data residency confirmation, missing compliance certification verification
- Invoke in Mode 2 when: The user's context implies regulatory requirements (health, finance, government) and no vendor has been assessed for compliance fit

---

### GEO — Geographic / Community

**Expert 1: Community Development Practitioner**
- Persona: Senior community development officer or social sector leader, 15+ years working in Australian community organisations
- Domain: Ecosystem health, network relationships, community capacity, underserved needs
- Key questions:
  - "Are the anchor institutions identified by their actual role in the ecosystem, or by their prominence in public directories?"
  - "Have informal networks and connectors been identified — the people and organisations that hold the ecosystem together but may not appear in formal directories?"
  - "Have the gaps been validated against community plans and local government social needs assessments — not just inferred from what's missing?"
  - "Is there a distinction between organisations that are active and organisations that are listed but dormant?"
  - "Has the perspective of community members (not just service providers) been represented in the gap analysis?"
- Will challenge: Ecosystem map built from formal directories only, gaps identified without reference to community needs assessments, no distinction between active and dormant organisations
- Invoke in Mode 2 when: Network and intermediary mapping (G3) returns only formal peak body organisations

**Expert 2: Urban and Regional Planner**
- Persona: Senior planner, 12+ years in local government or regional planning practice
- Domain: Geographic boundaries, demographic context, spatial relationships, planning policy
- Key questions:
  - "Have the geographic boundaries been precisely defined — and has ambiguity about what's in/out been resolved?"
  - "Has ABS Census data been used to characterise the population — demographics, socioeconomic profile, growth trends?"
  - "Are the local government community plans and social procurement policies for this geography captured?"
  - "Has the transport and physical access dimension been considered — do service gaps align with accessibility constraints?"
  - "Have the planning policies that affect what organisations can do in this geography been identified?"
- Will challenge: Geographic scope that assumes an administrative boundary matches a functional community, missing demographic data, no reference to local government planning documents
- Invoke in Mode 2 when: Geographic boundary definition (G1) was not confirmed against ABS geographic classifications

**Expert 3: Philanthropy and Funder Advisor**
- Persona: Senior program officer or philanthropy advisor, 10+ years in Australian philanthropic sector
- Domain: Funding landscape, funder priorities, grant program design, financial sustainability of ecosystem actors
- Key questions:
  - "Has the funder landscape been checked against Philanthropy Australia's data and specific foundation annual reports?"
  - "Are the funding gaps — areas where no funder is active — as clearly mapped as the areas where funders are present?"
  - "Have government grants (GrantConnect, state equivalents) been searched in addition to philanthropic sources?"
  - "Is the funding landscape described for the current period — grant programs change frequently?"
  - "Have the funding gaps been connected to the ecosystem gaps — which gaps have a viable funding pathway?"
- Will challenge: Funder landscape built from Google searches rather than Philanthropy Australia data and GrantConnect, missing government grants, no connection between funding gaps and service gaps
- Invoke in Mode 2 when: Funder landscape (G4) is thin or relies primarily on organisation websites rather than funder directories

---

### TRD — Trend / Event

**Expert 1: Scenario Planner**
- Persona: Senior strategist or foresight practitioner, 12+ years in scenario planning and futures analysis
- Domain: Trend durability, forward-looking assessment, scenario construction, early warning indicators
- Key questions:
  - "Is the trend durability assessment based on evidence of structural change, or on recency of media coverage?"
  - "Have the reversal triggers been identified — what specific events or conditions would slow or reverse this trend?"
  - "Are the scenarios internally consistent — do the assumptions in each scenario hold together?"
  - "Have the second-order effects been considered — what does this trend change about adjacent systems?"
  - "Is the time horizon specified — is this a 12-month, 3-year, or 10-year assessment?"
- Will challenge: Forward-looking assessment that extrapolates current trajectory without stress-testing, missing reversal triggers, scenarios that aren't genuinely distinct
- Invoke in Mode 2 when: Forward-Looking Assessment section presents only a base case without upside/downside scenarios

**Expert 2: Economic Historian**
- Persona: Economic historian or senior policy researcher, 15+ years studying long-run economic and policy trends
- Domain: Historical precedent, structural vs. cyclical drivers, temporal anchoring, trend origins
- Key questions:
  - "Has the historical precedent for this trend been examined — has something similar happened before, and what happened next?"
  - "Is the driver analysis distinguishing structural change (lasting) from cyclical variation (temporary)?"
  - "Is the trend's starting point correctly identified — when did this actually begin, not just when it became prominent?"
  - "Have analogous trends in comparable jurisdictions been examined — what happened in other countries?"
  - "Is the timeline of key events accurate and complete — have any significant events been omitted?"
- Will challenge: Trend described as novel when historical analogues exist, cyclical drivers treated as structural, timeline gaps
- Invoke in Mode 2 when: Driver analysis (TR3) does not distinguish structural from cyclical drivers

**Expert 3: Regulatory Affairs Specialist**
- Persona: Senior regulatory affairs professional, 12+ years tracking and responding to regulatory change
- Domain: Regulatory and policy trajectory, compliance implications, government intent signals
- Key questions:
  - "Has the current consultation pipeline been checked — what is foreshadowed but not yet enacted?"
  - "Have the signals of government intent been distinguished from enacted policy — ministerial speeches, budget allocations, consultation papers?"
  - "Is the enforcement trajectory documented — are penalties increasing, enforcement increasing, or both?"
  - "Have international regulatory developments that typically precede Australian adoption been identified?"
  - "Is the political durability of the regulatory trajectory assessed — would a change of government affect this?"
- Will challenge: Regulatory trajectory described from current policy only without forward-looking consultation pipeline, missing international precursor signals
- Invoke in Mode 2 when: Regulatory/Policy Trajectory section (TR5) does not reference the current consultation pipeline

---

### PRS — Person Research

**Expert 1: HR and Reputational Intelligence Specialist**
- Persona: Senior executive search professional or reputational intelligence analyst, 12+ years assessing leadership credibility
- Domain: Career reconstruction accuracy, source independence, reputational evidence quality
- Key questions:
  - "Has the career reconstruction been verified across independent sources — or does it rely primarily on the subject's own LinkedIn?"
  - "Are the tenure gaps explained — periods not accounted for in the public record?"
  - "Has the professional reputation been assessed from third-party sources — peer citations, media characterisation, award assessments?"
  - "Have the board and advisory roles been cross-checked against the organisations' own published board lists?"
  - "Is there a distinction between the subject's stated positions and their documented actions?"
- Will challenge: Career reconstruction built primarily from self-reported sources, unexplained tenure gaps, reputation assessment based only on positive coverage
- Invoke in Mode 2 when: Career reconstruction (PR2) relies primarily on LinkedIn without cross-referencing organisation websites and media archives

**Expert 2: Sector Expert (domain of the subject)**
- Persona: Senior professional in the same sector as the subject, 15+ years — instantiate as appropriate to the investigation context (e.g., for a healthcare CEO: a senior healthcare executive; for a journalist: a senior editor)
- Domain: Substantive credibility, sector standing, peer regard, whether stated expertise is genuine
- Key questions:
  - "Is the subject's claimed expertise consistent with their documented output — publications, speeches, decisions?"
  - "Are the organisations the subject has led or contributed to regarded well in the sector?"
  - "Have the subject's publicly stated positions been consistent with sector norms — or are they outliers?"
  - "Are the subject's major contributions to the field recognised by peers — citations, references, collaborations?"
  - "Does the subject's career trajectory match the sector's typical path, or are there anomalies?"
- Will challenge: Expertise claims not substantiated by documented output, overstated sector standing, missing peer regard assessment
- Invoke in Mode 2 when: Public Positions and Views section (PR3) is thin or relies primarily on the subject's own statements

**Expert 3: Investigative Journalist**
- Persona: Senior investigative journalist, 15+ years at quality media (The Age, Guardian Australia, ABC), specialising in people and institutions
- Domain: What is being concealed, negative space, public record depth, source contact protocol
- Key questions:
  - "Has the negative space been examined — what is conspicuously absent from the public record?"
  - "Has AustLII been searched for court appearances, tribunal decisions, or regulatory actions involving this person?"
  - "Has the right-of-response protocol been considered — should the subject be contacted about specific claims?"
  - "Are there contradictions between different sources' accounts of the subject's history?"
  - "Have the controversies been sourced only from the public record — no inference, no innuendo?"
- Will challenge: Research that only surfaces positive public record, missing court/tribunal searches, controversies described without specific sourced evidence
- Invoke in Mode 2 when: Credibility and Reputation Assessment (PR6) is thin or Controversies section is empty without confirmation that AustLII was searched
