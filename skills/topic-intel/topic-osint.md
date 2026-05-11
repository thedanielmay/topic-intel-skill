# Topic OSINT Reference

## Purpose

Open-source intelligence techniques scoped specifically to topic research. Complements the four-layer web access architecture in `~/.claude/skills/shared/references/web-access-layers.md`. Does NOT duplicate entity-specific techniques from `company-intel` (DNS, WHOIS, SSL, cloud footprint, stock photo detection) — those are irrelevant to topic research.

Read relevant sections when the investigation reaches the source type they cover.

---

## Section 1 — Advanced Search Operators for Topic Research

### Universal operators (all modules)

```text
# Government sources only
"[topic]" site:*.gov.au
"[topic]" site:*.gov.au filetype:pdf

# Grey literature and discussion papers
"[topic]" "discussion paper" OR "consultation paper" OR "issues paper" filetype:pdf
"[topic]" "working paper" OR "research report" filetype:pdf

# Date-bounded searches
"[topic]" after:2024-01-01
"[topic]" after:2023-01-01 before:2023-12-31

# Strip noise — find third-party coverage
"[topic]" -site:[source].gov.au -site:[source].org.au
"[topic]" -"media release" -"press release"

# Exact phrase for specific claims
"[exact claim or statistic]"
"[exact claim]" site:*.gov.au

# OR for variant terms
"[term 1]" OR "[term 2]" OR "[term 3]"
```

### MKT — Market research operators

```text
# Market sizing
"[market name]" "market size" OR "market value" filetype:pdf -site:ibisworld.com
"[market name]" ABS OR "Australian Bureau of Statistics"
"[sector]" "annual report" filetype:pdf site:*.gov.au

# Industry associations (often publish free market data)
"[market name]" "industry association" OR "peak body" site:*.org.au
"[market name]" "member survey" OR "industry survey" filetype:pdf

# Government procurement (AusTender)
"[sector keyword]" site:tenders.gov.au
"[sector keyword]" site:grants.gov.au
```

### POL — Policy research operators

```text
# Primary legislation
site:legislation.gov.au "[policy name]"
site:legislation.vic.gov.au "[policy name]"
site:austlii.edu.au "[policy name]"

# Parliamentary records
site:parlinfo.aph.gov.au "[policy name]"
site:parliament.vic.gov.au "[policy name]" hansard
site:parliament.vic.gov.au "[policy name]" submissions

# Audit and review
site:audit.vic.gov.au "[policy name]"
site:anao.gov.au "[policy name]"
site:pc.gov.au "[policy name]"

# Big-4 secondary commentary (credible, often free)
site:deloitte.com OR site:kpmg.com OR site:pwc.com.au OR site:ey.com "[regulatory topic]" filetype:pdf
```

### SCI — Academic research operators

```text
# Pre-prints and working papers (free access)
"[topic]" site:ssrn.com
"[topic]" site:arxiv.org
"[topic]" site:papers.ssrn.com
"[topic]" "working paper" filetype:pdf site:*.edu.au

# Australian research funding
site:dataportal.arc.gov.au "[topic keywords]"
"[topic]" site:nhmrc.gov.au
"[topic]" site:csiro.au research publication

# Systematic reviews (highest evidence tier)
"[topic]" "systematic review" OR "meta-analysis" site:cochranelibrary.com
"[topic]" "systematic review" filetype:pdf

# Citation search
"[author surname]" "[year]" "[journal abbreviation]"
```

### TRD — Trend research operators

```text
# Regulatory updates
site:asic.gov.au "[topic]" "media release"
site:treasury.gov.au "[topic]" consultation
site:aasb.gov.au "[topic]"

# International precursor signals (trends often arrive from US/UK first)
"[topic]" site:gov.uk
"[topic]" site:sec.gov
"[topic]" site:eur-lex.europa.eu

# Adoption signals
"[topic]" "early adopter" OR "pilot program" OR "trial"
"[topic]" "case study" filetype:pdf after:2023-01-01
```

### Google Image search parameters (PRS and evidence images)

| Parameter | Effect | Use case |
|---|---|---|
| `itp:face` | Face results only | PRS: profile photos of a subject, strip logos and banners |
| `isz:l` | Large images only | Evidence: high-resolution versions of documents or photos |
| `ic:specific,isc:green` | Dominant colour filter | Identifying branded materials by colour |
| `ic:trans` | Transparent background | Logo identification |
| `isz:lt,islt:2mp` | Greater than 2 megapixels | High-resolution evidence images |

**Usage:** Append to a standard Google Images URL:
```text
https://www.google.com/search?q=[query]&tbm=isch&tbs=itp:face
https://www.google.com/search?q=[query]&tbm=isch&tbs=isz:l
```

**Combined example — find profile photos of a subject on LinkedIn:**
```text
https://www.google.com/search?q="[Full Name]"+site:linkedin.com&tbm=isch&tbs=itp:face
```

### Query iteration strategy

1. **Start broad:** `"[topic]"` — establish baseline coverage, assess result volume and source diversity
2. **Add context:** `"[topic]" Australia` / `"[topic]" Melbourne` / `"[topic]" 2024`
3. **Subtract noise:** `-site:[dominant site].com -"press release" -jobs`
4. **Exact match for claims:** `"[specific statistic or claim]"` — confirms or fails to find evidence
5. **OR for variants:** `"[term]" OR "[synonym]" OR "[abbreviation]"`
6. **Date range exhaustion:** `after:2022-01-01 before:2022-12-31` per year systematically
7. **Site-specific:** `site:[specific source].gov.au "[topic]"` for high-value sources

---

## Section 2 — Wayback Machine Advanced Techniques

Relevant for all modules when sources delete, update, or retract content. Especially valuable for POL (policy language changes over time) and TRD (trend timeline reconstruction).

### CDX API — Programmatic archive access

```bash
# Find all archived URLs for a key government site
curl "http://web.archive.org/cdx/search/cdx?url=audit.vic.gov.au/*&output=json&fl=timestamp,original,statuscode&limit=100"

# Find specific archived page
curl "http://web.archive.org/cdx/search/cdx?url=legislation.gov.au/Details/[act]*&output=json&fl=timestamp,original,statuscode"

# Access a specific archived version
curl "https://web.archive.org/web/[YYYYMMDDHHMMSS]/[original-url]"
```

**Key government/audit domains to check via CDX:**
- `audit.vic.gov.au` — VAGO reports (sometimes removed after follow-up)
- `anao.gov.au` — ANAO performance audits
- `legislation.gov.au` — historical versions of Acts and Regulations
- `consultation.treasury.gov.au` — closed consultation documents
- `parlinfo.aph.gov.au` — parliamentary submissions

### Deleted content recovery

Policy consultation pages, retracted reports, and superseded guidance are frequently removed from government websites but preserved in the Wayback Machine.

**Workflow:**
1. Search the CDX API for the relevant domain and URL pattern
2. Find URLs that previously returned 200 but now return 404
3. Access the archived version: `https://web.archive.org/web/[timestamp]/[url]`
4. Log the archived URL in `research-log.md` with: original URL, archive date, why it was removed (if known)

### Snapshot diffing for regulatory language changes

Compare successive versions of a policy page or legislation to track how language changed over time. Useful for POL module Phase P6 (Timeline Reconstruction).

**Workflow:**
1. Use CDX API to find all snapshots of the target URL
2. Select snapshots from before and after a known amendment date
3. Fetch both versions and compare text — look for changes to obligation language, thresholds, definitions
4. Document: what changed, when it changed, and what the change implies for compliance

### `.DS_Store` directory enumeration

macOS `.DS_Store` files are sometimes pushed to production web servers accidentally. They reveal the directory structure of files the Finder has seen, often including documents not linked from the main site.

```bash
# Check if a .DS_Store file exists
curl -sO https://[target-domain]/.DS_Store 2>/dev/null
ls -la .DS_Store

# Parse the file to extract filenames
python3 -m pip install dsstore 2>/dev/null
python3 -m dsstore .DS_Store
```

**Useful for:** Finding unlisted government PDFs, consultation submissions, and internal documents on `.gov.au` sites that pushed their file system accidentally.

**Ethical note:** This accesses only publicly exposed file listings. Do not access files that require authentication.

---

## Section 3 — Social Media Signal Extraction

**Scope:** TRD (trend signals from social discourse) and PRS (person research) modules only. Not applicable to MKT, POL, SCI, TEC, or GEO.

### Twitter/X — Advanced tracking techniques

**Persistent numeric User ID:**
Every Twitter/X account has a permanent numeric ID that never changes even when the username changes or the account is deleted.

```bash
# Access account by ID (works across renames and deletions)
# https://x.com/i/user/<numeric_id>

# Find user ID from Wayback Machine archived profile
# Archived pages contain JSON-LD with "author":{"identifier":"<numeric_id>"}
curl "http://web.archive.org/cdx/search/cdx?url=twitter.com/[USERNAME]*&output=json&fl=timestamp,original,statuscode"
```

**Snowflake ID timestamp extraction:**
Any tweet ID can be converted to an exact creation timestamp.
```python
tweet_id = 1234567890123456789
timestamp_ms = (tweet_id >> 22) + 1288834974657
# Convert to datetime
import datetime
dt = datetime.datetime.fromtimestamp(timestamp_ms / 1000)
```
Useful for: precisely dating when a claim was first made publicly, timeline reconstruction in TRD investigations.

**Alternative access (no login required):**
```bash
# Nitter instance (shows tweets without login)
# https://nitter.poast.org/[USERNAME]

# Syndication API
# https://syndication.twitter.com/srv/timeline-profile/screen-name/[USERNAME]

# Username history trackers
# https://memory.lol/
```

**Username chain tracing:**
Accounts rename themselves; the same person may have 3-4 usernames over time.
1. Start with known username -> find Wayback Machine archives
2. In archived tweets, find t.co shortlinks -> these resolve to the username AT TIME OF POSTING
3. Discovered previous username -> enumerate across all platforms again
4. Repeat until no new usernames found

### Username enumeration across platforms

```bash
# Enumerate a username across 741+ platforms
# Web: https://whatsmyname.app
# Or run locally: sherlock [username]

# Cross-platform people search (paid, covers niche platforms)
# https://osint.industries/

# Username metadata — numeric suffixes often encode location/identity signals
# Pattern examples:
# LinXiayu35170  -> 35170 = postal code (Bruz, France)
# jsmith1998     -> birth year suffix
# user212nyc     -> 212 = Manhattan area code
# player44uk     -> +44 = United Kingdom country code
```

**Priority platforms for PRS username enumeration:**
Twitter/X, GitHub, Reddit, BlueSky, LinkedIn, Strava, Spotify, Steam, bio-link services (linktr.ee, about.me, bio.link)

### BlueSky — Public API (no authentication required)

```bash
# Search posts
curl -s "https://public.api.bsky.app/xrpc/app.bsky.feed.searchPosts?q=[QUERY]&sort=latest"

# Search accounts
curl -s "https://public.api.bsky.app/xrpc/app.bsky.actor.searchActors?q=[NAME]"

# Get profile
curl -s "https://public.api.bsky.app/xrpc/app.bsky.actor.getProfile?actor=[handle].bsky.social"

# Get all posts from an account
curl -s "https://public.api.bsky.app/xrpc/app.bsky.feed.getAuthorFeed?actor=[handle].bsky.social&limit=50"
```

**BlueSky search filters:**
```text
from:[username]        # Posts from specific user
since:2025-01-01      # Date range start
has:images            # Posts with images only
```

### Fitness platform GPS exposure (PRS module)

Strava, Garmin Connect, and MapMyRun public activity GPS routes expose home and work neighbourhoods. Users rarely restrict activity visibility.

**Workflow:**
1. Find target's Strava profile via username enumeration (whatsmyname.app or OSINT Industries)
2. Check public activities for GPS route maps at `https://www.strava.com/athletes/<id>`
3. Identify route start/end points or frequently visited locations
4. Verify with Google Maps and cross-reference against other known locations

**Key insight:** Even with "privacy zones" enabled, route shapes outside the hidden zone reveal the neighbourhood. Segment leaderboards expose athlete locations without requiring a follow.

### Platform false-positive table

These platforms return HTTP 200 for non-existent usernames. Check the body/title content, not just the status code.

| Platform | Returns 200 for non-existent? | How to detect a false positive |
|---|---|---|
| Telegram (`t.me/[user]`) | Yes | Page shows "Contact @[user]" not "View Profile" — "Contact" = doesn't exist |
| TikTok | Yes | Body contains "Couldn't find this account" |
| Smule | Yes | Body contains "Not Found" in page content |
| linkin.bio | Yes | Redirects to Later.com product page for unclaimed names |
| Instagram | Yes | Returns 200 but shows login wall — existence uncertain |

### Social media signal interpretation for TRD module

When researching trends, social media volume and sentiment are signals, not sources. Apply * confidence rating to all social media signal findings. Triangulate with primary sources before including in KIT answers.

**Trend signals worth recording:**
- Volume trajectory: is discussion growing, peaking, or declining?
- Adoption announcements: organisations publicly declaring adoption of the trend
- Practitioner discourse: are domain experts discussing the trend positively or sceptically?
- Resistance signals: organised opposition or criticism from credible voices

---

## Section 4 — Document Metadata for SCI and POL Modules

PDF and Office documents published by government agencies and academic institutions frequently contain metadata revealing the internal author (often different from the published author), creation timeline, and drafting software. Useful in POL module (who drafted a policy) and SCI module (confirming authorship and dating of research outputs).

### PDF metadata extraction

```bash
# Install exiftool if not present
brew install exiftool  # macOS

# Single file
exiftool document.pdf

# Batch extraction from a directory
exiftool -r -csv /path/to/downloads/ > metadata.csv

# Key fields to extract
exiftool -Author -Creator -Producer -CreateDate -ModifyDate document.pdf
```

**Key metadata fields and what they reveal:**

| Field | Reveals |
|---|---|
| `Author` | Internal drafter — often a public servant name not listed on the document cover |
| `Creator` | Application used to create (Microsoft Word, Adobe InDesign, LaTeX) — signals organisation tooling |
| `Producer` | PDF rendering engine (macOS Quartz, Adobe PDF Library, wkhtmltopdf) |
| `CreateDate` | When the document was originally created — may predate publication significantly |
| `ModifyDate` | Last edit — useful for tracking when a policy document was last substantively changed |
| `Company` | Organisation name from Office licence — confirms the originating organisation |
| `LastSavedBy` | Most recent editor — useful when Author field is generic |

**Practical workflow:**
1. Download all PDFs from a key source (government department, audit body, research institution)
2. Run batch extraction: `exiftool -r -csv /downloads/ > metadata.csv`
3. Cross-reference extracted author names against the known people list from the investigation
4. Note software/creation date patterns — a "new" policy document with a 2019 CreateDate warrants investigation

### Office document metadata

```bash
# DOCX, XLSX, PPTX — same exiftool command
exiftool document.docx

# Key additional fields for Office documents
exiftool -Author -LastSavedBy -Company -Manager -RevisionNumber -TotalEditTime document.docx
```

**Revision count and edit time** indicate how much work went into a document. A "first draft" consultation paper with 847 revisions and 40 hours of edit time has been through extensive internal review.

### Git commit author mining (TEC module)

When investigating technology vendors or open-source projects, public git repositories expose the email addresses of all contributors, including historical ones.

```bash
# Clone the target's public repo and extract all contributor emails
git clone https://github.com/<org>/<repo>.git
cd <repo>
git log --format="%an <%ae>%n%cn <%ce>" | sort -u

# GitHub-wide: list all public events from a user (extracts emails from commits)
gh api "users/<username>/events/public" --paginate \
  | python3 -c "import sys,json; [print(c['author']['email']) for e in json.load(sys.stdin) for c in e.get('payload',{}).get('commits',[]) if 'author' in c]" \
  | sort -u
```

**Use cases for TEC module:**
- Verify vendor team claims (does the claimed CTO actually have commits?)
- Assess team size and activity (how many unique contributors in the last 12 months?)
- Track developer network (who has contributed to both this project and related projects?)
- Find email addresses for professional contact verification in PRS-adjacent work

**Note:** Email addresses found in git history are public record — they were intentionally included in signed commits. Apply ** confidence rating (publicly committed by the person themselves).
