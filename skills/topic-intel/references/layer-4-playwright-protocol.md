# Layer 4 — Playwright Opt-In Protocol

## Purpose

This is the canonical Layer 4 specification. Both `company-intel` and `topic-intel` reference this file. Do not duplicate this logic inline in SKILL.md files.

This document covers:
- When to escalate to Layer 4 (decision tree)
- What to execute after the user says yes for a source (templates)
- What to log when Layer 4 also fails (inaccessible-sources register)
- Public sources and sources behind a login
- Operational security

---

## Section 1 — Escalation Decision Tree

### When does a layer count as failed?

| Layer | Failure condition |
|---|---|
| Layer 1 (API) | Returns HTTP 4xx/5xx, empty results array, times out, or requires an unconfigured auth key |
| Layer 2 (WebFetch) | Returns HTTP 403/429/503; returns fewer than ~200 characters of real body text; returns a bot-check page; or times out |
| Layer 2 (WebSearch) | Returns zero results, or all results are irrelevant URLs that do not contain the needed data |
| Layer 3 (scraping service) | API key not configured; service returns 4xx; service returns the same blocked content as direct fetch |

A layer has **not failed** if you simply have not tried it yet. Work through layers in order.

### What counts as "no other approach defined"?

All three must be true:
- No public API exists for this source (Layer 1 inapplicable)
- WebSearch found a URL, but WebFetch of that URL failed (Layer 2 exhausted)
- No scraping service API key is configured (Layer 3 unavailable)

### The decision tree

```
FOR EACH source URL or data need:

STEP 1 — Layer 1: Is there a known public API for this source?
   YES → Call the API. Got useful data? DONE.
         API failed? → STEP 2.
   NO  → STEP 2.

STEP 2 — Layer 2a: WebSearch to find the specific page URL.
   Returns relevant URL? → WebFetch that URL.
      WebFetch returns ≥200 chars of real content? → DONE.
      WebFetch returns empty/blocked/403/timeout? → STEP 3.
   No useful URLs? → Try one alternate query. Still nothing? → STEP 3.

STEP 3 — Layer 3: Is a scraping service configured?
   CHECK: for each of SCRAPINGBEE_API_KEY, ZENROWS_API_KEY and BRIGHTDATA_API_KEY, print only "set" or "not set", never the value
   YES → Route through scraping service. Got useful data? DONE.
         Service also blocked? → STEP 4.
   NO  → Log URL. → STEP 4.

STEP 4 — Layer 4 gate:
   Does this source require interactive/JS content?
   (JS-rendered SPA, infinite scroll, form submission on public pages, dynamic content)
      YES → Name the source and the reason to the user. Ask for a yes.
             Run Layer 4 only after the user says yes for that source (Section 2).
             On a no, log the source as a gap with the reason (Section 3).
      NO  → This is a Layer 3 problem, not Layer 4.
             Declare INACCESSIBLE. Log it (Section 3). Notify user if HIGH priority.

STEP 5 — If Layer 4 also fails:
   Log as fully inaccessible (Section 3).
   Surface to user only if HIGH priority gap.
   Continue research. Never halt on a single failed source.
```

### Nothing is automatic

Layer 4 is opt in. For every source, tell the user the URL and the reason before any Layer 4 attempt. Run the browser only after the user says yes for that source. On a no, log the source as a gap with the reason. If Playwright is not installed, say so and log the gap with that reason. Do not use Layer 4 on a source behind a login.

Ask message (show for each source before Layer 4):
```
Layer 4 (Playwright) requested for: [URL]
Reason: [e.g. "JS-rendered content, WebFetch returned an empty body"]
Action: Run only after your yes for this source.
```

### Known JS-rendered public sources

For these sources, do not attempt WebFetch. Use Layer 2 WebSearch to find the URL, then ask the user before any Layer 4 attempt:
- `glassdoor.com`: all pages
- `seek.com.au`: job listing detail pages
- `productreview.com.au`: review pages
- ASIC Connect (`connectonline.asic.gov.au`): form-based search
- Fair Work Commission online search

---

## Section 2 — Layer 4 Execution Templates

Playwright runs as a normal browser. It identifies as automation. Do not use stealth tools, user-agent spoofing or other evasion. Do not save or reuse session cookies.

### Pre-flight check (run once per session)

```bash
python3 -c "import playwright" 2>/dev/null || echo "Playwright is not installed"
```

If the import fails, say Playwright is not installed and skip Layer 4. Log each affected source as a gap with that reason.

---

### Template A — Standard page load and content extraction

Use for: JS-rendered pages, SPAs, government portals that return blank via WebFetch.

```python
python3 << 'PYEOF'
from playwright.sync_api import sync_playwright
import sys

TARGET_URL = "REPLACE_WITH_URL"
TIMEOUT_MS = 30000  # increase to 60000 for known-slow sources (ACNC, court portals)

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    context = browser.new_context()
    page = context.new_page()
    try:
        page.goto(TARGET_URL, timeout=TIMEOUT_MS, wait_until="domcontentloaded")
        page.wait_for_load_state("networkidle", timeout=10000)
    except Exception as e:
        print(f"LOAD_ERROR: {e}", file=sys.stderr)
    content = page.inner_text("body")
    print(content[:12000])
    browser.close()
PYEOF
```

If the body returns fewer than 200 chars or shows a bot check, try Template B or C.

---

### Template B — Infinite scroll / dynamic content loading

Use for: public post feeds, product listing pages, news archives that paginate on scroll.

```python
python3 << 'PYEOF'
from playwright.sync_api import sync_playwright
import time

TARGET_URL = "REPLACE_WITH_URL"
SCROLL_PASSES = 4
SCROLL_PAUSE_SEC = 2.5

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    context = browser.new_context()
    page = context.new_page()
    page.goto(TARGET_URL, timeout=30000, wait_until="domcontentloaded")
    time.sleep(2)
    for i in range(SCROLL_PASSES):
        page.evaluate("window.scrollTo(0, document.body.scrollHeight)")
        time.sleep(SCROLL_PAUSE_SEC)
    content = page.inner_text("body")
    print(content[:20000])
    browser.close()
PYEOF
```

---

### Template C — Form submission / multi-step search

Use for: ASIC Connect company search, court filing portals, government tender searches.

```python
python3 << 'PYEOF'
from playwright.sync_api import sync_playwright
import time, sys

BASE_URL              = "REPLACE_WITH_FORM_URL"
SEARCH_TERM           = "REPLACE_WITH_SEARCH_TERM"
SEARCH_INPUT_SELECTOR = "input[name='q']"       # replace with actual selector
SUBMIT_SELECTOR       = "button[type='submit']" # replace with actual selector
RESULTS_SELECTOR      = "#results"              # replace with actual selector

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    context = browser.new_context()
    page = context.new_page()
    page.goto(BASE_URL, timeout=30000, wait_until="domcontentloaded")
    time.sleep(1.5)
    page.fill(SEARCH_INPUT_SELECTOR, SEARCH_TERM)
    time.sleep(0.5)
    page.click(SUBMIT_SELECTOR)
    try:
        page.wait_for_selector(RESULTS_SELECTOR, timeout=15000)
    except Exception as e:
        print(f"RESULTS_TIMEOUT: {e}", file=sys.stderr)
    content = page.inner_text("body")
    print(content[:15000])
    browser.close()
PYEOF
```

---

### Template D — PDF download via browser

Use for: Government reports gated behind a JS-driven download button.

```python
python3 << 'PYEOF'
from playwright.sync_api import sync_playwright

TARGET_URL            = "REPLACE_WITH_PDF_PAGE_URL"
OUTPUT_PATH           = "/tmp/playwright-download.pdf"
DOWNLOAD_LINK_SELECTOR = "a[href$='.pdf']"  # adjust to match actual link

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    context = browser.new_context(accept_downloads=True)
    page = context.new_page()
    page.goto(TARGET_URL, timeout=30000, wait_until="domcontentloaded")
    with page.expect_download(timeout=60000) as download_info:
        page.click(DOWNLOAD_LINK_SELECTOR)
    download = download_info.value
    download.save_as(OUTPUT_PATH)
    print(f"Downloaded to: {OUTPUT_PATH}")
    browser.close()
# Follow up: use Read tool on /tmp/playwright-download.pdf
PYEOF
```

---

### Template F — Retry wrapper

Apply when Templates A to D return thin content on first attempt.

```python
python3 << 'PYEOF'
from playwright.sync_api import sync_playwright
import time, sys

TARGET_URL  = "REPLACE_WITH_URL"
MAX_RETRIES = 2
TIMEOUT_MS  = 45000

def fetch_with_retry(url, max_retries):
    for attempt in range(max_retries + 1):
        try:
            with sync_playwright() as p:
                browser = p.chromium.launch(headless=True)
                context = browser.new_context()
                page = context.new_page()
                page.goto(url, timeout=TIMEOUT_MS, wait_until="domcontentloaded")
                time.sleep(2)
                content = page.inner_text("body")
                browser.close()
                if len(content) > 200:
                    return content
                print(f"Attempt {attempt+1}: thin content ({len(content)} chars), retrying...",
                      file=sys.stderr)
        except Exception as e:
            print(f"Attempt {attempt+1} error: {e}", file=sys.stderr)
        if attempt < max_retries:
            time.sleep(3 * (attempt + 1))
    return None

result = fetch_with_retry(TARGET_URL, MAX_RETRIES)
if result:
    print(result[:12000])
else:
    print("LAYER4_FAILED: all retries exhausted", file=sys.stderr)
    sys.exit(1)
PYEOF
```

**Retry rules:** Max 2 retries (3 attempts total). Back-off: 3s after attempt 1, 6s after attempt 2. If all retries return fewer than 200 chars: declare Layer 4 failed → Section 3 logging.

---

## Section 3 — Source Logging When Layer 4 Also Fails

### Inaccessible-sources register

Append to `inaccessible-sources.md` in the investigation folder:

```markdown
| # | URL | Layers tried | Failure mode | Data needed | Priority | Manual action |
|---|-----|---|---|---|---|---|
| 1 | https://example.com | L1: N/A · L2: 403 · L3: not configured · L4: timeout ×3 | Pricing detail | HIGH | Open in browser; copy pricing table |
| 2 | https://example.org/members | L2: login wall · L4: not attempted (login required) | Member detail | MEDIUM | User to read the page and paste content |
```

**Priority:**
- **HIGH** — source is a KIT or creates a material gap in a deliverable section
- **MEDIUM** — adds depth; section completable without it
- **LOW** — nice to have; not material

**Continue or pause?**

Continue silently when: all inaccessible sources for a section are MEDIUM or LOW; the section has at least two other sources covering the same need.

Surface to user when: any HIGH-priority source is inaccessible; or Playwright is not installed.

**User notification (pause case):**
```
Layer 4 escalation attempted and failed for [N] HIGH-priority source(s):

  1. [URL] — [what data was needed] — [failure mode]

These gaps affect the [section name] section.

Options:
  a. Continue without this data (gaps flagged clearly in deliverable)
  b. Open these URLs manually and paste relevant content here
  c. Re-run with a scraping service configured

Which would you prefer? (default: a)
```

Never halt the entire investigation for a single inaccessible source.

---

## Section 4 — Sources Behind a Login

A source that requires a login is a gap. Do not open a headed browser to log in, and do not save or reuse session cookies. Use only what is publicly visible through Layers 1 to 3.

If the user reads the page themselves and pastes the content, use what they paste and cite it as user-provided.

LinkedIn is not a Layer 4 target. Apply the public-source rule above to LinkedIn company pages, profiles, posts and job postings. Keep the Phase 4 research goals in `phase-playbook.md`. Change only how the data is collected.

---

## Section 5 — Operational Security

### Why Layer 4 has a larger footprint

- Multiple rapid page views from the same IP appear in target's analytics
- Unusual visit patterns on small sites may be noticed

### When to care

| Investigation type | Concern | Action |
|---|---|---|
| Large public organisation | Low | None |
| Small organisation (target might notice) | Medium | Space requests |
| Sensitive integrity investigation | High | Confirm with user before any Layer 4 |

### Pace page loads

Add a short pause between page loads in multi-page scripts so the site is not hit with a burst of requests:

```python
import random
import time

time.sleep(random.uniform(2.5, 5.0))
```

