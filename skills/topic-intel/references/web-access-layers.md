# Web Access Layers — Shared Reference

## Overview

Direct web fetching (WebFetch) fails for a significant proportion of intelligence targets:
- JS-rendered pages return blank or minimal content
- Some sites (Facebook, LinkedIn, web.archive.org) are blocked at the Claude Code network level
- Sites with bot detection return 403s or misleading content
- Large pages time out at the 60-second WebFetch limit

This document describes a four-layer escalation architecture. Work through layers in order — stop as soon as you have enough data from a layer. Only escalate when the current layer fails.

Skill-specific API endpoints are documented in each skill's own source register, not here.

---

## Layer 1 — API-First

For any data source with a public API, use it. APIs return clean structured data with no rendering issues, and are faster and more reliable than scraping.

See the skill's source register for specific API endpoints relevant to the investigation type.

### Universal APIs

**News / search**
- Use WebSearch (built-in) — this is effectively a search API and covers most news use cases
- For comprehensive media monitoring, SerpAPI or Brave Search (if configured by user)

**Wayback Machine CDX API** — Historical snapshots without accessing web.archive.org directly:
```bash
# Get all snapshots of a URL
curl "http://web.archive.org/cdx/search/cdx?url={domain}&output=json&fl=timestamp,original,statuscode&limit=50"

# Get snapshots within a date range
curl "http://web.archive.org/cdx/search/cdx?url={domain}&output=json&from=20200101&to=20231231&limit=20"

# Fetch a specific snapshot
curl "https://web.archive.org/web/{timestamp}/{url}"
```
web.archive.org itself is blocked. The CDX API uses different infrastructure and works fine.

---

## Layer 2 — Search API as Router

Instead of guessing page URLs and fetching them blind, use WebSearch to find the exact pages that contain the information you need, then fetch only those specific URLs.

This is the most important layer to apply consistently. It prevents:
- Blind crawling that hits 404s and timeouts
- Fetching JavaScript-rendered pages that return no content
- Cascade failures when parallel WebFetch calls time out

### How to use it

**Step 1: Search for the specific information you need**
```
WebSearch: "site:example.org pricing"
→ returns the exact URL of the pricing page
→ fetch only that URL
```

**Step 2: Use search operators to find structured data**
```
WebSearch: '"Organisation Name" annual report filetype:pdf'
→ returns direct PDF links
→ read the PDF with the Read tool (not WebFetch)

WebSearch: '"Organisation Name" grants 2024 site:acnc.gov.au'
→ finds the specific ACNC document page
→ fetch only that URL
```

**Step 3: Use search to verify a page exists before fetching it**
```
WebSearch: '"example.org" "supplier" OR "vendor" procurement
→ confirms which pages contain procurement information
→ fetch those specific pages rather than crawling the site map
```

### High-value search patterns

```
# Financial / regulatory
site:acnc.gov.au "{organisation name}"
"{organisation name}" filetype:pdf "annual report"

# People
"{person name}" "{organisation}" linkedin.com
"{person name}" CEO OR founder "{organisation}"

# News with date range
"{subject}" after:2023-01-01 before:2025-01-01

# Negative / risk
"{subject}" complaint OR lawsuit OR investigation

# Government sources
"{topic}" site:gov.au
"{topic}" filetype:pdf site:vic.gov.au

# Documents
"{topic}" filetype:pdf after:2022-01-01
```

---

## Layer 3 — Scraping Service for Blocked Sites

When WebFetch returns an error, is blocked, or returns empty/meaningless content from a JS-rendered page, route the request through a scraping service.

### Check if a scraping service is available

Before attempting Layer 3, check whether the user has configured a scraping service:

```bash
# Check for ScrapingBee
echo $SCRAPINGBEE_API_KEY

# Check for BrightData
echo $BRIGHTDATA_API_KEY

# Check for ZenRows
echo $ZENROWS_API_KEY

# Check for ScraperAPI
echo $SCRAPERAPI_KEY
```

If none are configured, skip Layer 3 and note the gap in `research-log.md` with the specific URLs that need scraping. Prompt the user:

> "The following pages are blocked for direct access and would benefit from a scraping service. If you have a ScrapingBee, ZenRows, or similar API key, add it to your environment and I can retry these. Alternatively, these can be opened manually in a browser:
> - [URL 1] — [what it contains]
> - [URL 2] — [what it contains]"

### Using ScrapingBee

```bash
curl -s "https://app.scrapingbee.com/api/v1/?api_key=$SCRAPINGBEE_API_KEY&url={ENCODED_URL}&render_js=true" > /tmp/scraped.html
```

Use `render_js=true` for JS-rendered pages (LinkedIn, Facebook, Instagram, modern SPAs).
Use `render_js=false` for simple HTML pages (faster, cheaper).

Parse the result with Bash or Python as needed.

### Sites that commonly require Layer 3

| Site | Issue | Layer 3 fix |
|------|-------|-------------|
| LinkedIn company/profile pages | Bot detection | `render_js=true` |
| Facebook / Meta Ad Library | Blocked at network level | `render_js=true` |
| Instagram | JS-rendered, login required | `render_js=true` |
| Glassdoor | Bot detection | `render_js=true` |
| ProductReview.com.au | JS-rendered | `render_js=true` |
| Seek.com.au | Bot detection | `render_js=true` |
| web.archive.org | Blocked at network level | Avoid — use CDX API instead |

---

## Layer 4 — Playwright for Interactive Flows

Layer 4 is opt in. It is not an automatic fallback. Before any Layer 4 attempt, tell the user the source and the reason. Run the browser only after the user says yes for that source. On a no, or when Playwright is not installed, log the source as a gap with the reason. Do not use Layer 4 on a source behind a login.

Use Playwright only when you need to:
- Navigate multi-page flows (e.g., multi-step search forms)
- Fill and submit forms programmatically
- Handle dynamic content that changes based on user interaction
- Scrape content that only loads after user events (scroll, click, hover)

**Full Layer 4 protocol:** Read `references/layer-4-playwright-protocol.md`.

### When to use Playwright vs Layer 3

- **Use Layer 3** when a page loads all its content on initial render (even if JS-rendered) — scraping services handle this well and are faster
- **Use Layer 4** when content requires user interaction to load (infinite scroll, button clicks, form submission on public pages), and only after the user says yes

---

## Decision Tree — Which Layer to Use

```
For each URL/source you need to access:

1. Does it have a public API?
   → YES: Use Layer 1 (API). Done.
   → NO: Continue.

2. Can WebSearch find the specific page URL containing the data?
   → YES: Use Layer 2 (fetch that specific URL with WebFetch).
          If WebFetch succeeds: Done.
          If WebFetch times out or returns empty: Continue.
   → NO: Try a different search query. If still nothing: Continue.

3. Is a scraping service configured ($SCRAPINGBEE_API_KEY etc.)?
   → YES: Use Layer 3. If succeeds: Done.
   → NO: Log the URL as a manual review item. Prompt the user.

4. Does the data require interactive navigation (forms, clicks, scroll)?
   → YES: Name the source and the reason to the user. Use Layer 4 (Playwright)
          only after the user says yes for that source. If no, or Playwright is
          not installed, log the gap with the reason.
   → NO: This is a Layer 3 problem, not Layer 4.

5. None of the above worked?
   → Log as gap in research-log.md
   → Note what the gap means for the intelligence picture
   → Flag for manual review with instructions
```

---

## Handling Cascade Failures

When running parallel WebFetch calls, a timeout in one call can cause sibling calls to fail with "Sibling tool call errored."

**Prevention:**
- Run slow/risky fetches (ACNC, Facebook, web.archive.org) in separate sequential calls, not in parallel batches
- Limit parallel WebFetch batches to 3-4 calls maximum
- Put known-slow sources last in any batch

**When cascade occurs:**
- Re-run the failed calls individually, not in a batch
- If a source consistently fails, escalate to Layer 2/3 rather than retrying WebFetch

---

## Research Log — Access Failures Template

When a source is inaccessible at all layers, log it with this format in `research-log.md`:

```
| URL | Layer tried | Failure mode | Data needed | Manual action |
|-----|-------------|--------------|-------------|---------------|
| acnc.gov.au/... | WebFetch | Timeout (60s) | Financial statements | Download PDF from ACNC profile and place in _inbox/ |
| facebook.com/ads/library | WebFetch | Blocked | Ad creative/spend | Open in browser: [URL] |
```

This creates an actionable list for the user rather than a silent gap.
