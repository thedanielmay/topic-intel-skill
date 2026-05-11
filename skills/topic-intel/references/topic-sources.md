# Topic Sources — API Endpoints and Access Guide

## Overview

This register provides the primary sources, API endpoints, access notes, and filtering heuristics for each topic-intel module. Use Layer 1 (API) sources first — they are faster, more reliable, and return structured data. Fall back to Layer 2 (search → fetch) and Layer 4 (Playwright) per the web-access-layers.md protocol.

---

## Universal Sources (All Topic Types)

These apply regardless of topic type.

**WebSearch** — Built-in. Use search operators for precision:
```
"[topic]" site:[source].gov.au            — government sources only
"[topic]" filetype:pdf                    — documents only
"[topic]" after:2024-01-01               — date-bounded
"[topic]" "[exact phrase]"               — phrase search
"[topic]" -"[exclude term]"              — exclude noise
```

**Wayback Machine CDX API** — Historical snapshots without accessing web.archive.org directly:
```bash
curl "http://web.archive.org/cdx/search/cdx?url={domain}&output=json&fl=timestamp,original,statuscode&limit=20"
curl "https://web.archive.org/web/{timestamp}/{url}"
```

**Google Trends** — Topic trajectory and seasonality:
- WebFetch: `https://trends.google.com/trends/explore?q={encoded_topic}&geo=AU`
- Note: JS-rendered. Use Layer 4 Template A.

**Wikipedia** — Use as a reference MAP to primary sources, not as a primary source (★ rating only).

---

### Additional universal sources

These sources apply across multiple modules or provide cross-cutting data not covered by module-specific registers.

**Economic and financial data:**

| Source | Access | Modules | Notes |
|---|---|---|---|
| FRED (Federal Reserve Economic Data) | Free API: `https://api.stlouisfed.org/fred/series/observations?series_id={id}&api_key={key}&file_type=json` | MKT, TRD | US-focused but widely used for macro benchmarking |
| World Bank Open Data | Free API: `https://api.worldbank.org/v2/country/all/indicator/{INDICATOR_CODE}?format=json` | MKT, TRD, GEO | Global development indicators by country |
| IMF Data | Free: `https://www.imf.org/en/Data` | MKT, TRD | Macroeconomic data; World Economic Outlook dataset |
| OECD.Stat | Free: `https://stats.oecd.org/` | MKT, POL, TRD | OECD member policy and economic data; downloadable datasets |

**Academic and research:**

| Source | Access | Modules | Notes |
|---|---|---|---|
| SSRN | Free WebFetch: `https://ssrn.com/search` | SCI, POL | Social science and economics pre-prints; search by keyword |
| OpenAlex | Free API: `https://api.openalex.org/works?search={terms}` | SCI | Open academic graph; replaces Microsoft Academic Graph |
| Web of Science | Paywalled — flag; university library access | SCI | Broad academic coverage; citation analysis |
| Scopus | Paywalled — flag; university library access | SCI | Broad academic coverage; Elsevier |
| JSTOR | Partial free: `https://www.jstor.org/` | SCI, POL | Humanities and social sciences archive; free access to 100 articles/month |
| Google Dataset Search | Free: `https://datasetsearch.research.google.com/` | All | Indexes datasets published on the web; useful for finding primary data |

**Legislative and regulatory (international):**

| Source | Access | Modules | Notes |
|---|---|---|---|
| Congress.gov API | Free API key: `https://api.congress.gov/` | POL | US federal legislation; useful for international policy comparison |
| Regulations.gov | Free: `https://www.regulations.gov/search?filter={term}` | POL | US federal rulemaking; public comments |
| EUR-Lex | Free: `https://eur-lex.europa.eu/search.html` | POL | EU legislation and case law; full text |
| UK legislation | Free: `https://www.legislation.gov.uk/search?text={term}` | POL | UK Acts and statutory instruments |

**Think tank and grey literature:**

| Source | Access | Modules | Notes |
|---|---|---|---|
| Brookings Institution | Free WebFetch: `https://www.brookings.edu/search/?s={term}` | MKT, POL, TRD | Leading US think tank; economics, governance, social policy |
| RAND Corporation | Free WebFetch: `https://www.rand.org/search.html#q={term}` | MKT, POL, TRD | Policy research; defence, health, education, economics |
| NBER Working Papers | Free (abstracts): `https://www.nber.org/papers?q={term}` | SCI, MKT | National Bureau of Economic Research; economics working papers |
| Pew Research Center | Free WebFetch: `https://www.pewresearch.org/search/{term}/` | GEO, TRD | Social research; US-focused but methodology widely cited |
| Council on Foreign Relations | Free WebFetch: `https://www.cfr.org/search#q={term}` | TRD, POL | Geopolitical analysis; international policy |
| SIPRI | Free WebFetch: `https://www.sipri.org/databases` | TRD, POL | Security and defence data; arms trade, military expenditure |

**Trend and prediction signals:**

| Source | Access | Modules | Notes |
|---|---|---|---|
| Exploding Topics | Free (limited): `https://explodingtopics.com/` | TRD, TEC | Identifies trends before mainstream coverage; keyword-based |
| Metaculus | Free WebFetch: `https://www.metaculus.com/questions/` | TRD | Prediction market; community forecasts on specific questions |
| Manifold Markets | Free WebFetch: `https://manifold.markets/browse` | TRD | Prediction market; real-money equivalent probabilities |
| Google Trends (advanced) | Layer 4: `https://trends.google.com/trends/explore?q={term}&geo=AU` | TRD | Interest trajectory over time; compare multiple terms |

---

## Module MKT — Market / Industry Sources

### Australian Statistics (API available)

**ABS — Australian Bureau of Statistics**
- TableBuilder: `https://www.abs.gov.au/statistics` — search by industry/topic
- API access: `https://api.data.abs.gov.au/` (ABS Data API, SDMX format)
- Key datasets: Industry statistics (8155.0), Labour statistics, Business characteristics

```bash
# ABS data API example — get industry data
curl "https://api.data.abs.gov.au/data/ABS,ABS_INDUSTRY,1.0.0/all?startPeriod=2020&endPeriod=2024&format=jsondata"
```

**ACCC — Market studies and reports**
- WebFetch: `https://www.accc.gov.au/about-us/publications/market-studies`
- WebSearch: `site:accc.gov.au "[industry]" market study`

**AusTender — Government contracts (what government spends in each sector)**
- API: `https://www.tenders.gov.au/api/`
- WebSearch: `site:tenders.gov.au "[sector keyword]"`

**GrantConnect — Government grants (what government funds in each sector)**
- WebFetch: `https://www.grants.gov.au/Go/Show?GoUuid=` (search by keyword)
- WebSearch: `site:grants.gov.au "[sector]"`

### Paywalled Sources (flag, do not silently fail)

| Source | Coverage | Action if needed |
|---|---|---|
| IBISWorld | Australian industry reports | Flag as paywalled; note university library access |
| Mintel / Euromonitor | Consumer market data | Flag as paywalled |
| Statista | Cross-sector statistics | Free tier has limited data; flag premium content |
| PitchBook / CB Insights | Startup and VC data | Flag as paywalled; use Crunchbase free tier instead |

When a paywalled source is relevant, add to `inaccessible-sources.md` with note: "Paywalled — no free access."

### Free Alternatives to Paywalled Sources

- **Crunchbase** (free tier): `https://www.crunchbase.com/search/organizations` — funding and acquisition data
- **ACNC register**: Financial data for NFP operators in many markets
- **Trade association websites**: Industry statistics often published publicly
- **Company annual reports**: WebSearch `"[company]" "annual report" filetype:pdf`

---

## Module POL — Policy / Regulatory Sources

### Australian Legislation (API available)

**Federal Register of Legislation**
- API: `https://www.legislation.gov.au/api/search?query={term}&series=act`
- Direct: `https://www.legislation.gov.au/` — search by title or keyword
- Returns: Acts, regulations, instruments, compilations

```bash
# Search federal legislation
curl "https://www.legislation.gov.au/api/search?query=social+procurement&series=act&format=json"
```

**AustLII — All Australian Jurisdictions**
- WebFetch: `https://www.austlii.edu.au/cgi-bin/query?mask_path=au/legis&query={term}`
- Coverage: Federal + all state/territory legislation, case law, tribunal decisions
- WebSearch: `site:austlii.edu.au "[policy name]"`

**Victorian Government Legislation**
- Direct: `https://www.legislation.vic.gov.au/` — search by title
- WebFetch works for most pages

**State Equivalents**
| State | URL |
|---|---|
| NSW | https://legislation.nsw.gov.au/ |
| QLD | https://www.legislation.qld.gov.au/ |
| WA | https://www.legislation.wa.gov.au/ |
| SA | https://www.legislation.sa.gov.au/ |
| TAS | https://www.legislation.tas.gov.au/ |
| ACT | https://www.legislation.act.gov.au/ |
| NT | https://legislation.nt.gov.au/ |

### Parliamentary Sources

**Federal Hansard**
- WebFetch: `https://parlinfo.aph.gov.au/parlInfo/search/search.w3p;query={encoded_term}`
- WebSearch: `site:parlinfo.aph.gov.au "[policy name]"`

**Victorian Parliament Hansard**
- WebFetch: `https://www.parliament.vic.gov.au/hansard/`
- WebSearch: `site:parliament.vic.gov.au "[policy name]" hansard`

**Committee submissions**
- Federal: `https://www.aph.gov.au/Parliamentary_Business/Committees`
- Victorian: `https://www.parliament.vic.gov.au/committees`
- WebSearch: `site:parliament.vic.gov.au "[policy]" submissions`

### Audit and Review Bodies

| Body | Coverage | URL |
|---|---|---|
| Victorian Auditor-General (VAGO) | VIC government program audits | https://www.audit.vic.gov.au/reports |
| Australian National Audit Office (ANAO) | Federal program audits | https://www.anao.gov.au/work/performance-audit |
| Productivity Commission | Economic and social policy | https://www.pc.gov.au/inquiries |
| Parliamentary Budget Office | Policy costings | https://www.pbo.gov.au/fiscal-analysis |

---

## Module SCI — Academic / Scientific Sources

### Free APIs with Good Coverage

**Semantic Scholar API** — Best free academic search API
```bash
# Paper search
curl "https://api.semanticscholar.org/graph/v1/paper/search?query={encoded_terms}&fields=title,authors,year,citationCount,abstract,externalIds&limit=20"

# Author search
curl "https://api.semanticscholar.org/graph/v1/author/search?query={name}&fields=name,affiliations,paperCount,citationCount"

# Paper citations (find papers citing a key paper)
curl "https://api.semanticscholar.org/graph/v1/paper/{paperId}/citations?fields=title,year,citationCount&limit=20"
```
Rate limit: 100 requests/5 minutes (no key required for basic access)

**CrossRef API** — DOI resolution and paper metadata
```bash
# Search papers
curl "https://api.crossref.org/works?query={encoded_terms}&sort=relevance&rows=20&filter=from-pub-date:2020"

# Get specific paper by DOI
curl "https://api.crossref.org/works/{DOI}"
```
No authentication required. Polite pool: add `mailto=your@email.com` to requests.

**arXiv API** — Pre-prints in STEM fields
```bash
curl "https://export.arxiv.org/api/query?search_query=all:{terms}&start=0&max_results=20&sortBy=submittedDate&sortOrder=descending"
```

**PubMed/NCBI** — Biomedical and health sciences
```bash
# Search
curl "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term={encoded_terms}&retmax=20&retmode=json"
# Fetch abstracts
curl "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=pubmed&id={pmid}&retmode=xml"
```

### Australian Research Sources

**ARC Grant Discovery Portal**
- WebFetch: `https://dataportal.arc.gov.au/NCGP/Web/Grant/Grants`
- WebSearch: `site:dataportal.arc.gov.au "{topic keywords}"`
- Coverage: All ARC-funded research projects (Discovery, Linkage, Future Fellowships)

**CSIRO Publications**
- WebFetch: `https://research.csiro.au/` — search by topic
- WebSearch: `site:csiro.au "{topic}" research publication`

**AIHW — Australian Institute of Health and Welfare** (for health/social data)
- WebFetch: `https://www.aihw.gov.au/reports-data/`
- API-like data downloads available for major datasets

### Filtering Heuristics for High-Volume Sources

When searches return large numbers of results:
1. Apply citation count threshold appropriate to field maturity:
   - Emerging field (<10 years): ≥5 citations
   - Mature field (>10 years): ≥20 citations
2. Apply date filter matching the temporal scope in the Topic Scoping Document
3. Prioritise: systematic reviews > RCTs/experimental > observational > expert opinion
4. Declare saturation when new searches return ≥50% papers already in the register

---

## Module TEC — Technology / Product Sources

### Review Platforms

**G2** — Software reviews (WebFetch works for category pages)
- Category search: `https://www.g2.com/categories/{category-slug}`
- WebSearch: `site:g2.com "{product category}"`

**Capterra**
- Category search: `https://www.capterra.com.au/directory/{category}`
- WebSearch: `site:capterra.com.au "{product category}"`

**Gartner Peer Insights** (partial WebFetch access)
- WebSearch: `site:gartner.com "peer insights" "{product category}"`

### Developer / OSS Sources

**GitHub API** — Repository activity signals
```bash
# Search repositories
curl "https://api.github.com/search/repositories?q={topic}+in:readme&sort=stars&order=desc"

# Get repository stats
curl "https://api.github.com/repos/{owner}/{repo}"
```

**npm trends** (for JavaScript packages)
- WebFetch: `https://npmtrends.com/{package1}-vs-{package2}`

### Funding and Acquisition Data

**Crunchbase** (free tier)
- WebFetch: `https://www.crunchbase.com/search/organizations`
- Note: JS-rendered, Layer 4 Template A required
- WebSearch: `site:crunchbase.com "{company or category}"`

**Product Hunt**
- WebFetch: `https://www.producthunt.com/search?q={terms}`
- Coverage: Launch announcements, upvotes, community reviews

---

## Module GEO — Geographic / Community Sources

### Australian Geographic Data

**ABS Census** — Population and community statistics
- TableBuilder: `https://www.abs.gov.au/websitedbs/censushome.nsf/home/tablebuilder`
- Pre-built community profiles: `https://www.abs.gov.au/census/find-census-data/community-profiles`
- Coverage: All Australian geographic levels (SA1 → LGA → State)

**data.gov.au** — Australian open government data
- Search: `https://data.gov.au/search?q={topic}`
- Coverage: Federal and state government datasets

**Local Government Open Data Portals**
| Council | Data Portal |
|---|---|
| City of Melbourne | https://data.melbourne.vic.gov.au/ |
| Maribyrnong | https://www.maribyrnong.vic.gov.au/Your-Council/Data-and-Statistics |
| Yarra | https://www.yarracity.vic.gov.au/services/open-data |

### Social Enterprise Registers

**Social Traders Certified Enterprise Directory**
- WebFetch: `https://www.socialtraders.com.au/find-a-social-enterprise/`
- WebSearch: `site:socialtraders.com.au "[location]"`

**ACNC Charity Register with Geographic Filter**
- WebFetch: `https://www.acnc.gov.au/charity/charities?state={state}&postcode={postcode}`
- API: `https://www.acnc.gov.au/api/entity/search?q={name}&state={state}`

**Buy Social Australia Directory**
- WebFetch: `https://buysocial.org.au/directory/`

**Kinaway Chamber of Commerce** (Aboriginal business directory)
- WebFetch: `https://www.kinaway.com.au/directory/`

### Community and Sector Sources

**VCOSS — Victorian Council of Social Service**
- WebFetch: `https://vcoss.org.au/`
- Coverage: Victorian community sector policy, research, member directory

**Pro Bono Australia**
- WebSearch: `site:probonoaustralia.com.au "[location]" "[sector]"`
- Coverage: NFP sector news, research, job listings

**Philanthropy Australia**
- WebFetch: `https://www.philanthropy.org.au/tools-resources/`
- Coverage: Funder directory, giving data

---

## Module TRD — Trend / Event Sources

**Regulatory update feeds (subscribe or search)**

| Agency | Update source |
|---|---|
| ASIC | https://asic.gov.au/about-asic/news-centre/find-a-media-release/ |
| APRA | https://www.apra.gov.au/news-and-publications |
| Treasury (Federal) | https://treasury.gov.au/consultation |
| DTF Victoria | https://www.dtf.vic.gov.au/news |
| AASB | https://aasb.gov.au/news/latest-news/ |

**Google Trends trajectory**
- Use Layer 4 Template A on: `https://trends.google.com/trends/explore?q={encoded_topic}&geo=AU`

**Big-4 accounting firm publications** (credible secondary commentary on regulatory trends)
- WebSearch: `site:deloitte.com OR site:kpmg.com OR site:pwc.com.au OR site:ey.com "[regulatory topic]" filetype:pdf`

---

## Module PRS — Person Research Sources

**LinkedIn** — Primary source for career history and role. Requires Layer 4 (see layer-4-playwright-protocol.md LinkedIn protocol).

**Organisation websites** — Current role confirmation (★★ if independently published)

**Media archives** — Statements, interviews, journalism
```
WebSearch: '"{person name}" interview OR speech OR statement [year range]'
WebSearch: '"{person name}" site:[publication].com.au'
```

**AEC Political Donations Register** (for figures in political life)
- WebFetch: `https://transparency.aec.gov.au/`

**AustLII** (for public record — court appearances, tribunal decisions)
- WebSearch: `site:austlii.edu.au "{person name}"`

**Wikipedia** — Use as reference map only; follow citations to primary sources. Never cite Wikipedia as a primary source (★ at most).

---

---

## International Sources (All Topic Types)

Use these when the topic has a global or non-Australian scope, or when Australian data is absent and international benchmarks are needed.

**OECD iLibrary** — Policy, economics, education, social data across OECD members
- Data portal: `https://stats.oecd.org/` (structured data downloads)
- Publications: `https://www.oecd-ilibrary.org/` (reports and working papers)
- WebSearch: `site:oecd.org "{topic}"`

**World Bank Open Data** — Global development indicators, country data
- API: `https://api.worldbank.org/v2/country/all/indicator/{INDICATOR_CODE}?format=json`
- Data catalogue: `https://data.worldbank.org/`
- WebSearch: `site:worldbank.org "{topic}"`

**Eurostat** — EU statistical data (useful for European policy comparisons)
- Data browser: `https://ec.europa.eu/eurostat/databrowser/`
- WebSearch: `site:ec.europa.eu/eurostat "{topic}"`

**US Legislation and Policy**
- Congress.gov: `https://www.congress.gov/search?q={term}` (federal legislation)
- Regulations.gov: `https://www.regulations.gov/search?filter={term}` (rulemaking)
- WebSearch: `site:congress.gov "{topic}"` or `site:regulations.gov "{topic}"`

**UK Government and Policy**
- GOV.UK: `https://www.gov.uk/search/all?keywords={term}` (policy, guidance, statistics)
- UK Parliament: `https://hansard.parliament.uk/search?searchTerm={term}`
- WebSearch: `site:gov.uk "{topic}"`

**UN and Multilateral Bodies**
- UN data: `https://data.un.org/` (international statistics)
- ILO: `https://ilostat.ilo.org/` (labour and employment data)
- WHO: `https://www.who.int/data/` (health data)

**Cross-jurisdiction company / entity data**
- OpenCorporates: `https://api.opencorporates.com/v0.4/companies/search?q={name}` (global company register)

**When to use international sources:**
- Topic is explicitly global or multi-jurisdictional
- Australian evidence is thin and international benchmarks add confidence
- The KIT asks about a trend, standard, or policy that originated overseas
- Comparing Australian practice to international norms

Always note the jurisdiction of any international source and assess relevance to the Australian/local context.

---

## Access Failure Logging

When any source fails all layers, add to `inaccessible-sources.md`:

```markdown
| # | URL | Layers tried | Failure mode | Data needed | Priority | Manual action |
|---|-----|---|---|---|---|---|
| 1 | [URL] | L1: N/A · L2: 403 · L3: not configured · L4: timeout | [what was needed] | HIGH/MEDIUM/LOW | [specific manual action] |
```

For paywalled sources, use failure mode: `Paywalled — no free access` and manual action: `Access via [library/institution] or purchase`.

---

## Source Effort Caps by Investigation Tier

Control web access layer usage based on investigation tier. Apply these caps when briefing agents or deciding whether to attempt Layer 3/4 access.

| Tier | Max web access layer | Playwright (Layer 4) | Scraping service (Layer 3) | Rationale |
|---|---|---|---|---|
| Tier 1 — Quick Scan | Layer 2 (search -> fetch) | Never | Never | Speed and simplicity; Quick Scans should complete in ~30 min |
| Tier 2 — Standard | Layer 3 (scraping service) | Never | Yes, if configured | Allows JS-rendered sources; preserves time budget |
| Tier 3 — Full Investigation | Layer 4 (Playwright) | Yes | Yes | All sources accessible; no layer restrictions |

**When Layer 3 is not configured:** Fall back to Layer 2 for Tier 2 investigations. Do not block the investigation — log the inaccessible source in `inaccessible-sources.md` and proceed.

**When Layer 4 is available:** Always attempt Layer 4 before declaring a source inaccessible in Tier 3 investigations. See `~/.claude/skills/shared/references/layer-4-playwright-protocol.md` for the full execution protocol.
