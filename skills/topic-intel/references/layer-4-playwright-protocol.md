# Layer 4 — Playwright Universal Fallback Protocol

## Purpose

This is the canonical Layer 4 specification. Both `company-intel` and `topic-intel` reference this file. Do not duplicate this logic inline in SKILL.md files.

This document covers:
- When to escalate to Layer 4 (decision tree)
- What to execute when Layer 4 is triggered (templates)
- What to log when Layer 4 also fails (inaccessible-sources register)
- LinkedIn-specific protocol
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
   CHECK: echo $SCRAPINGBEE_API_KEY $ZENROWS_API_KEY $BRIGHTDATA_API_KEY
   YES → Route through scraping service. Got useful data? DONE.
         Service also blocked? → STEP 4.
   NO  → Log URL. → STEP 4.

STEP 4 — Layer 4 gate:
   Does this source require interactive/JS content?
   (JS-rendered SPA, infinite scroll, form submission, login wall, dynamic content)
      YES → PROCEED TO LAYER 4 EXECUTION (Section 2).
      NO  → This is a Layer 3 problem, not Layer 4.
             Declare INACCESSIBLE. Log it (Section 3). Notify user if HIGH priority.

STEP 5 — If Layer 4 also fails:
   Log as fully inaccessible (Section 3).
   Surface to user only if HIGH priority gap.
   Continue research — never halt on a single failed source.
```

### Automatic vs. pause-and-ask

**Attempt automatically** (no pause) when:
- Source is a known Layer 4 target (LinkedIn, Glassdoor, JS-rendered government portals)
- Content is publicly accessible but JS-rendered
- No login is required

**Pause and inform the user first** when:
- Source requires authentication (login wall detected)
- Investigation is flagged as sensitive

Pause message (show only on first Layer 4 trigger in a session):
```
Layer 4 (Playwright) triggered for: [URL]
Reason: [e.g. "JS-rendered content — WebFetch returned empty body"]
Action: Launching headless browser. This may take 15–30 seconds.
```

### Known JS-rendered sources — skip directly to Layer 4

For these sources, do not attempt WebFetch. Use Layer 2 WebSearch to find the URL, then go directly to Layer 4:
- `linkedin.com` — all pages
- `glassdoor.com` — all pages
- `seek.com.au` — job listing detail pages
- `productreview.com.au` — review pages
- ASIC Connect (`connectonline.asic.gov.au`) — form-based search
- Fair Work Commission online search

---

## Section 2 — Layer 4 Execution Templates

**CRITICAL:** Always use `/usr/bin/python3` (Python 3.9, system install — Playwright is installed here).
Do NOT use `python3` — it resolves to Homebrew Python 3.14 where Playwright is NOT installed.

### Pre-flight check (run once per session)

```bash
/usr/bin/python3 -c "import playwright; print('playwright ok')" 2>/dev/null || echo "playwright NOT installed"
/usr/bin/python3 -c "from playwright_stealth import Stealth; print('stealth ok')" 2>/dev/null || echo "stealth NOT installed"
```

---

### Template A — Standard page load and content extraction

Use for: JS-rendered pages, SPAs, government portals that return blank via WebFetch.

```python
/usr/bin/python3 << 'PYEOF'
from playwright.sync_api import sync_playwright
from playwright_stealth import Stealth
import sys

TARGET_URL = "REPLACE_WITH_URL"
TIMEOUT_MS = 30000  # increase to 60000 for known-slow sources (ACNC, court portals)

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    context = browser.new_context(
        user_agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) "
                   "AppleWebKit/537.36 (KHTML, like Gecko) "
                   "Chrome/120.0.0.0 Safari/537.36"
    )
    page = context.new_page()
    Stealth().apply_stealth_sync(page)
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

If body returns fewer than 200 chars, still has bot detection — escalate to Template B or C.

---

### Template B — Infinite scroll / dynamic content loading

Use for: LinkedIn post feeds, product listing pages, news archives that paginate on scroll.

```python
/usr/bin/python3 << 'PYEOF'
from playwright.sync_api import sync_playwright
from playwright_stealth import Stealth
import time, sys

TARGET_URL = "REPLACE_WITH_URL"
SCROLL_PASSES = 4
SCROLL_PAUSE_SEC = 2.5

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    context = browser.new_context(
        user_agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) "
                   "AppleWebKit/537.36 (KHTML, like Gecko) "
                   "Chrome/120.0.0.0 Safari/537.36"
    )
    page = context.new_page()
    Stealth().apply_stealth_sync(page)
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
/usr/bin/python3 << 'PYEOF'
from playwright.sync_api import sync_playwright
from playwright_stealth import Stealth
import time, sys

BASE_URL              = "REPLACE_WITH_FORM_URL"
SEARCH_TERM           = "REPLACE_WITH_SEARCH_TERM"
SEARCH_INPUT_SELECTOR = "input[name='q']"       # replace with actual selector
SUBMIT_SELECTOR       = "button[type='submit']" # replace with actual selector
RESULTS_SELECTOR      = "#results"              # replace with actual selector

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    context = browser.new_context(
        user_agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) "
                   "AppleWebKit/537.36 (KHTML, like Gecko) "
                   "Chrome/120.0.0.0 Safari/537.36"
    )
    page = context.new_page()
    Stealth().apply_stealth_sync(page)
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
/usr/bin/python3 << 'PYEOF'
from playwright.sync_api import sync_playwright
from playwright_stealth import Stealth
import os, sys

TARGET_URL            = "REPLACE_WITH_PDF_PAGE_URL"
OUTPUT_PATH           = "/tmp/playwright-download.pdf"
DOWNLOAD_LINK_SELECTOR = "a[href$='.pdf']"  # adjust to match actual link

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    context = browser.new_context(accept_downloads=True)
    page = context.new_page()
    Stealth().apply_stealth_sync(page)
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

### Template E — Login wall: headed session capture and headless reuse

**Step 1 — Headed login (run once, interactive):**

Tell the user: "This source requires authentication. I'll open a browser window — please log in normally, and I'll save the session for headless reuse."

```python
/usr/bin/python3 << 'PYEOF'
from playwright.sync_api import sync_playwright
import json, os

SERVICE             = "REPLACE_WITH_SERVICE_NAME"  # e.g. "linkedin"
LOGIN_URL           = "REPLACE_WITH_LOGIN_URL"
SUCCESS_URL_PATTERN = "REPLACE_WITH_POST_LOGIN_URL_PATTERN"  # e.g. "**/feed/**"
SESSION_DIR         = os.path.expanduser(f"~/.playwright-sessions/{SERVICE}")
os.makedirs(SESSION_DIR, exist_ok=True)

with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    context = browser.new_context()
    page    = context.new_page()
    page.goto(LOGIN_URL)
    print(f"Browser opened. Please log in to {SERVICE}.")
    print("Waiting up to 120 seconds for successful login...")
    page.wait_for_url(SUCCESS_URL_PATTERN, timeout=120000)
    cookies = context.cookies()
    cookie_path = os.path.join(SESSION_DIR, "cookies.json")
    with open(cookie_path, "w") as f:
        json.dump(cookies, f)
    print(f"Session saved: {cookie_path} ({len(cookies)} cookies)")
    browser.close()
PYEOF
```

**Step 2 — Headless reuse (all subsequent calls):**

```python
/usr/bin/python3 << 'PYEOF'
from playwright.sync_api import sync_playwright
from playwright_stealth import Stealth
import json, os, sys, time

SERVICE     = "REPLACE_WITH_SERVICE_NAME"
TARGET_URL  = "REPLACE_WITH_TARGET_URL"
COOKIE_PATH = os.path.expanduser(f"~/.playwright-sessions/{SERVICE}/cookies.json")

if not os.path.exists(COOKIE_PATH):
    print("ERROR: No saved session. Run the headed login template first.", file=sys.stderr)
    sys.exit(1)

with open(COOKIE_PATH) as f:
    cookies = json.load(f)

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    context = browser.new_context(
        user_agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) "
                   "AppleWebKit/537.36 (KHTML, like Gecko) "
                   "Chrome/120.0.0.0 Safari/537.36"
    )
    context.add_cookies(cookies)
    page = context.new_page()
    Stealth().apply_stealth_sync(page)
    page.goto(TARGET_URL, timeout=30000, wait_until="domcontentloaded")
    time.sleep(3)
    content = page.inner_text("body")
    if "sign in" in content[:500].lower() or "log in" in content[:500].lower():
        print("SESSION_EXPIRED: re-run the headed login template.", file=sys.stderr)
        sys.exit(1)
    print(content[:12000])
    browser.close()
PYEOF
```

**Saved sessions on this machine:**
- LinkedIn: `~/.playwright-sessions/linkedin/cookies.json` (saved 3 March 2026)

---

### Template F — Retry wrapper

Apply when Templates A–D return thin content on first attempt.

```python
/usr/bin/python3 << 'PYEOF'
from playwright.sync_api import sync_playwright
from playwright_stealth import Stealth
import time, sys

TARGET_URL  = "REPLACE_WITH_URL"
MAX_RETRIES = 2
TIMEOUT_MS  = 45000

def fetch_with_retry(url, max_retries):
    for attempt in range(max_retries + 1):
        try:
            with sync_playwright() as p:
                browser = p.chromium.launch(headless=True)
                context = browser.new_context(
                    user_agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) "
                               "AppleWebKit/537.36 (KHTML, like Gecko) "
                               "Chrome/120.0.0.0 Safari/537.36"
                )
                page = context.new_page()
                Stealth().apply_stealth_sync(page)
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
| 2 | https://linkedin.com/company/x | L2: login wall · L4: session expired | Recent posts, headcount | MEDIUM | Re-run Template E headed login |
```

**Priority:**
- **HIGH** — source is a KIT or creates a material gap in a deliverable section
- **MEDIUM** — adds depth; section completable without it
- **LOW** — nice to have; not material

**Continue or pause?**

Continue silently when: all inaccessible sources for a section are MEDIUM or LOW; the section has at least two other sources covering the same need.

Surface to user when: any HIGH-priority source is inaccessible; or Layer 4 itself failed to import.

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

## Section 4 — LinkedIn-Specific Protocol

### Escalation path

```
LinkedIn source encountered:

STEP 1 — Layer 2 search to find the correct URL slug.
   WebSearch: '"Company Name" site:linkedin.com/company'
   WebSearch: '"Person Name" "Job Title" site:linkedin.com/in'

STEP 2 — Skip WebFetch. LinkedIn always blocks it. Go directly to Layer 4.

STEP 3 — Check for saved session:
   ls ~/.playwright-sessions/linkedin/cookies.json 2>/dev/null
   EXISTS → Template E Step 2 (headless with cookies)
   ABSENT → Template A (stealth, no login) for PUBLIC data only

STEP 4 — If stealth returns login redirect:
   Pause. "LinkedIn requires login. Shall I open a browser for you to log in?"
   YES → Template E Step 1, then Step 2
   NO  → Log as inaccessible (authentication declined)
```

### What is accessible with and without authentication

| LinkedIn data | Unauthenticated | Authenticated |
|---|---|---|
| Company name, tagline, follower count | Sometimes | Yes |
| Company description | Sometimes | Yes |
| Recent company posts | No | Yes |
| Person's current job title | Sometimes | Yes |
| Person's full career history | No | Yes |
| Job posting details | Partial | Full |

Always note in the research document whether LinkedIn data was obtained unauthenticated or session-authenticated.

### LinkedIn company posts (authenticated)

Use Template B with `SCROLL_PASSES = 6` and `SCROLL_PAUSE_SEC = 3.0`. LinkedIn post feeds require multiple scroll cycles.

### Session expiry detection

Template E Step 2 detects session expiry: if "sign in" or "log in" appears in first 500 chars, exits with code 1. Re-run Template E Step 1 to refresh.

---

## Section 5 — Operational Security

### Why Layer 4 has a larger footprint

- Multiple rapid page views from the same IP appear in target's analytics
- LinkedIn notifies profile owners when profiles are viewed (all access methods)
- Unusual visit patterns on small sites may be noticed

### When to care

| Investigation type | Concern | Action |
|---|---|---|
| Large public organisation | Low | None |
| Small organisation (target might notice) | Medium | Space requests |
| Sensitive integrity investigation | High | Confirm with user before any Layer 4 |
| Any LinkedIn profile view | Always notify | Tell user before viewing |

### How to reduce footprint

Add between page loads in multi-page scripts:
```python
import random
time.sleep(random.uniform(2.5, 5.0))
```

Rotate user-agent strings for multi-page runs:
```python
UA_LIST = [
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 Chrome/120.0.0.0 Safari/537.36",
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 Chrome/119.0.0.0 Safari/537.36",
]
user_agent = random.choice(UA_LIST)
```

---

## Version and Machine Notes

**Canonical file:** `~/.claude/skills/company-intel/references/layer-4-playwright-protocol.md`
**Also referenced by:** `topic-intel` — reference this file rather than copying

**Machine-specific:**
- `/usr/bin/python3` = Python 3.9, Playwright installed, Chromium installed
- `python3` = Homebrew 3.14, Playwright NOT installed
- LinkedIn session: `~/.playwright-sessions/linkedin/cookies.json` (saved 3 March 2026)
