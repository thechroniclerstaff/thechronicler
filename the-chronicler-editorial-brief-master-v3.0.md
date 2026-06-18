# The Chronicler — Editorial Brief
**Version:** 3.0 (Master)
**Effective from:** June 17, 2026 — SPA + monthly-JSON architecture
**Supersedes:** Editorial Brief v2.1 (archived)
**Publisher/Editor:** Anand
**Production Platform:** Claude (Anthropic) + Cloudflare Pages
**Canonical URL:** https://thechronicler.ca
**Instagram:** @the.chronicler.news
**Contact:** thechroniclerstaff@proton.me

---

## HOW TO USE THIS DOCUMENT

This is the single authoritative reference for all production standards. It is injected at the start of every production session, alongside the Production Log, the current `index.html`, and the current month's `articles-YYYY-MM.json` file. Those files govern everything in that session.

Standards are numbered I–XII and ordered to mirror the production sequence — from the first action taken before any drafting begins, through to the final check before publication. Group 3 in this version (Standard IX) replaces what was Groups 3 and the old Standard XIII in v2.1: it now describes the site's data architecture rather than an HTML build process, because there is no longer a per-edition HTML file to assemble.

The production log (separate document) records session-specific data: research slates, publisher approvals, market figures, corrections, and source audits. It references standards by number but does not restate them.

**Archive note:** Editorial Brief v2.1 and all prior versions, along with production logs from the per-edition era, are preserved for archival reference. They describe a publishing model (discrete daily HTML editions, Vol./No. numbering, GTA as a separate desk, This Week in History, Upcoming Events) that no longer reflects the live site as of this version. Consult them only to understand historical decisions, never as current standards.

---

## CHANGELOG — v2.1 → v3.0

- **Architecture.** No more per-edition HTML files. The site is a single `index.html` (a small JS application) that loads articles from monthly JSON files (`articles-YYYY-MM.json`). Production now means adding article objects to a JSON file, not assembling a document.
- **No edition numbering.** Vol./No. is gone from the UI, replaced by date + a "Live Edition" indicator. The "daily edition as artefact" feeling is retained as an editorial framing, not a literal once-a-day publishing schedule — see Standard I.
- **GTA is becoming a tag, not a desk.** The multi-tag system is not yet built into the JSON schema (no `tags` field exists as of this version). Until it ships, GTA-relevant Canada-desk stories should be flagged in the slate and production log so they can be retroactively tagged — see Standard I and Standard IX-B.
- **This Week in History and Upcoming Events are permanently discontinued**, not paused. They do not appear anywhere in the new workflow.
- **Sport is a single unified section**, not a per-desk subsection (no more "Canada Sports" vs. "India Sports" as separate research categories).
- **Weather and currency are fully automated.** Both fetch live, client-side, on every page load (Open-Meteo for weather/AQI, Frankfurter/ECB for currency). Neither requires any manual sourcing, fetching, or verification during production anymore. This removes a meaningful chunk of session-time work that existed under v2.1.
- **World Cup live scores are automated** via a Cloudflare Worker proxying football-data.org, refreshing every 3 minutes. The homepage strip needs no manual updates. The standalone Specials page (`Specials/world-cup-2026.html`) may still need manual handling — confirm with the publisher each tournament.
- **Market indices remain manually sourced and verified** each session — this is the one data category in the old Standard IX-C/D that survives unchanged in spirit, just relocated to a JSON block instead of HTML cards.
- **Crunch, Word Web, Crossbar, and Conquest have no permanent home in the new design.** Crunch and Word Web are still produced daily; until a delivery mechanism exists, log them in the production log under "Puzzles — Pending Integration" rather than losing them.
- **Standard XIII (Index File Requirements) is retired.** Its surviving relevant content is folded into Standard IX-G through IX-J.

---

## PUBLICATION IDENTITY

| Field | Value |
|---|---|
| Name | The Chronicler |
| URL | https://thechronicler.ca |
| Instagram | @the.chronicler.news |
| Contact | thechroniclerstaff@proton.me |
| Style | Independent AI-assisted daily news site |
| Aesthetic | Clean broadsheet, white canvas — UnifrakturMaguntia masthead, ink/rust/gold accent system |

### CSS Variables (required in `index.html`)

```css
:root {
  --ink:   #141008;
  --rust:  #8b2513;
  --gold:  #a07800;
  --grey:  #635d4e;
  --rule:  #e8e2d8;
  --warm:  #faf8f4;
  --text:  #222222;
  --muted: #888888;
}
```

### Required Google Fonts
- UnifrakturMaguntia — masthead only
- Playfair Display — headlines, values, expanded-article headline
- Source Sans 3 — body text, UI labels, buttons, captions

Libre Baskerville and Caveat are no longer loaded. There is currently no comic strip (Flatland News) in the live site — see Standard IX-G.

---

# GROUP 1 — PRE-PRODUCTION GATES

These standards must be satisfied before any article copy is drafted. Violation of any Group 1 standard invalidates the draft.

---

## Standard I — Article-by-Article Production Workflow (Mandatory Gate)

### Purpose

The "research everything, draft everything, generate one file" workflow allows training-data memory contamination to pass automated audit checks undetected. In Vol. I No. 23, 35 of 51 source URLs were fabricated — articles drafted from background knowledge with plausible URLs constructed post-hoc. This standard eliminates that failure mode by making publisher verification a mandatory gate at every article. This gate has not changed with the architecture; what changed is only what happens after drafting.

### The Two-Session Structure

**Session 1 (Research and Slate)**

Claude conducts all live searches for the day's articles. Research slates are presented one section at a time, in this fixed sequence:

1. Canada — Current Events
2. Canada — Politics
3. Canada — Economy and Business
4. India — Current Events
5. India — Politics
6. India — Economy and Business
7. World — Current Events
8. World — Politics
9. World — Economy and Business
10. Sport — unified across all desks (not split by nation)
11. World Cup — only while the tournament is active; omitted entirely otherwise

This Week in History and Upcoming Events are permanently discontinued and do not appear in this sequence under any circumstances.

Each section slate shows per article: proposed headline, source domain, confirmed specific URL, and search query used. Where a story is specifically about the Greater Toronto Area (or another locality likely to become a future tag), flag it inline in the slate — e.g. "(GTA-local)" — even though the JSON schema cannot yet store this. This flag should also be carried into the production log so it can be retroactively applied once the tag system ships (Standard IX-B). Publisher approves or rejects each section slate before the next is presented.

Session 1 closes only when all section slates for that day (10, or 11 with World Cup active) are approved.

**Session 2 (Production)**

Claude drafts all articles from approved URLs, gathers and verifies market index/commodity figures (weather and currency require no action — see Standard IX-D and IX-E), appends the new article objects to the current month's JSON file, overwrites the `markets` block with the session's verified figures, and presents the result for publisher review. Publisher reads the new articles, batches any corrections, corrections are applied, and the file is committed/pushed.

### The Four Phases — No Phase May Be Skipped

**Phase I — Research Slate (Session 1)**

The publisher supplies the current month's `articles-YYYY-MM.json` (and the previous month's file if early in a new month) at the start of Session 1. Claude reads it before conducting any searches, to enforce the no-repetition rule below.

Target article counts per slate: Current Events 3, Politics 3, Economy and Business 3 (each, per desk: Canada/India/World), Sport 3 (unified, not per desk), World Cup 2–3 (when active). These are targets, not hard minimums. If Claude cannot identify sufficient qualifying stories, it flags the shortfall with count and reason; the publisher decides whether to accept it. A section may not be padded with weak stories to hit the target.

No-repetition rule: No proposed headline may cover a topic already covered in the last several days of published articles (as shown in the supplied JSON) unless a confirmed new development has occurred and is evidenced by the live search result. No proposed headline may cover a topic already covered elsewhere in the current session's slates unless there is a distinct, uncovered aspect. There is no longer a single discrete "previous edition" to check against — judge repetition against recent publication history, generally the last 2–3 days, unless the story is genuinely evergreen and recurring (e.g. daily market commentary, which is exempt).

Exception: a sports result may be covered with a national angle under Sport even if a Canada- or India-desk Current Events story touches the same event, provided the framing is demonstrably distinct.

**Phase II — Article-by-Article Drafting (Session 2)**

All articles are drafted from Session 1 approved URLs. If a source has been materially updated or superseded overnight, Claude stops, flags the original and new information, and waits for publisher direction before proceeding with that article.

**Phase III — Data Verification (Session 2)**

Only market indices and commodities require verification — see Standard IX-C. Weather and currency are fully automated and need no session-time action. Indian market data may be provided directly by the publisher from a live screen if search results are lagging.

**Phase IV — Data Assembly (Session 2)**

Once every article and the markets block has explicit publisher approval, Claude:
1. Constructs each new article object per the schema in Standard IX-B, with lowercase `section` and `id` values.
2. Appends the new objects to the `articles` array in the current month's JSON file.
3. Overwrites (does not append to) the `markets` object with the session's verified figures.
4. Validates the resulting file is syntactically valid JSON before presenting it.

No HTML file is generated or assembled. Post-publication errors are corrected by editing the specific article object (matched by `id`) directly, and documented in the corrections log (Standard XII).

### Mandatory Gate Conditions

All must be true before the JSON file is updated:

- Current month's JSON (and previous month's, if applicable) supplied and read at Session 1 start
- All section slates for the day approved by publisher in Session 1
- No topic repeated from recent publication history without a confirmed new development
- No topic repeated across this session's slates without a distinct angle
- All section article counts meet targets or shortfalls flagged and approved
- Every article drafted from a Session 1 approved URL
- Any overnight source change flag resolved by publisher before drafting proceeds
- All market index/commodity figures verified in Session 2
- Crunch puzzle solution computationally verified via Python
- Word Web tiles and groups confirmed as topically accurate
- New article objects use lowercase `section` and `id` values exclusively
- Resulting JSON file validated as syntactically correct before delivery

---

## Standard II — Source-First Rule and Fabrication Prevention

*(Unchanged from v2.1.)*

### The Core Rule

No article may be drafted without a confirmed specific source URL found by a live web search during the current production session. No article may ever be drafted from training-data knowledge alone. The sequence is fixed:

1. Search for the story
2. Confirm the specific article URL is live and returns the expected article
3. Draft the article from that source
4. Record the URL and the search query that found it in the research slate

No URL may ever be constructed to match an article. Every URL must be found, not built.

Before proposing any source domain in a research slate, confirm the outlet is in the Standard III Approved Source Registry. Unlisted outlets must be flagged before presenting the slate to the publisher.

### Prohibited Actions

- Drafting an article from training-data knowledge
- Constructing or fabricating a URL to match an article
- Presenting a search result's domain without confirming the specific article URL exists
- Treating familiarity with a topic as a substitute for a confirmed live source

### Mandatory Warning Signs — Apply Additional Scrutiny When

1. The article reads like background context or analysis rather than a specific recent event
2. The URL was not retrieved from a search result in this session
3. The article mentions a specific venue, institution, person, entity, or proper noun that could be confused with a similar one (e.g. Rogers Centre vs. BMO Field)
4. No search was conducted for that specific article during the session

### What the Source Audit Can and Cannot Catch

| The audit reliably catches | The audit cannot catch |
|---|---|
| Homepage / section-root URLs | Fabricated article-style URLs |
| Standard III registry violations | URLs that look real but do not exist |
| Stated recency violations | Stale stories with current byline dates |
| Duplicate sources across sections | Training-data articles dressed as current news |

### Publisher Action

If any article reads like background context rather than breaking news, run a live search against the stated source URL before accepting the draft. This is the only reliable check.

---

## Standard III — Approved Source Registry

Any outlet not in this registry must be flagged in the source audit. At the end of every source audit, explicitly list any unlisted source and ask the publisher whether to add it to this registry.

### Canada Desk

CBC News (cbc.ca) · Globe and Mail (theglobeandmail.com) · CTV News (ctvnews.ca) · Global News (globalnews.ca) · National Post (nationalpost.com) · BNN Bloomberg (bnnbloomberg.ca) · Financial Post (financialpost.com) · TSN (tsn.ca) · Sportsnet (sportsnet.ca) · Radio-Canada / RCI (ici.radio-canada.ca) · CP24 (cp24.com) · The Canadian Press · Statistics Canada · Bank of Canada · Government of Canada (canada.ca) · Maclean's · iPolitics · The Walrus (thewalrus.ca) · Montreal CityNews (montreal.citynews.ca) · Al Jazeera English (aljazeera.com — also approved for World Desk)

**Canada Desk — GTA-area sources** (for stories likely to carry a future GTA tag): Toronto Star (thestar.com) · Toronto Sun (torontosun.com) · CP24 (cp24.com) · Metrolinx News (metrolinx.com/en/news) · Toronto FC (torontofc.ca) · BlogTO (blogto.com — current events only) · City of Toronto Newsroom · Brampton Guardian · Markham Economist and Sun · Oakville News · Whitby This Week (durhamregion.com/whitby-this-week) · The Local (thelocal.to) · Region of Peel / York Region / Durham Region official sites

### India Desk

The Hindu (thehindu.com) · The Indian Express (indianexpress.com) · Hindustan Times (hindustantimes.com — network-blocked, use search-confirmation per Standard II) · Business Standard (business-standard.com) · LiveMint (livemint.com) · Financial Express (financialexpress.com) · Economic Times (economictimes.indiatimes.com) · NDTV (ndtv.com) · The Tribune (tribuneindia.com — paywalled, search-confirm and draft from snippets) · Deccan Herald (deccanherald.com) · India Today (indiatoday.in) · Frontline (frontline.thehindu.com) · Times of India (timesofindia.indiatimes.com — network-blocked, search-confirm) · Outlook India (outlookindia.com) · Outlook Business (outlookbusiness.com) · ETV Bharat (etvbharat.com) · ANI (aninews.in — primary newswire) · PTI (ptinews.com — national newswire) · PM India (pmindia.gov.in) · ESPNcricinfo (espncricinfo.com) · IMD (imd.gov.in) · BSE India (bseindia.com) · NSE India (nseindia.com)

### World Desk

Reuters (reuters.com) · Associated Press (apnews.com — network-blocked, search-confirm with original AP URL cited) · BBC News (bbc.com/news) · Al Jazeera English (aljazeera.com) · CNN (cnn.com) · The Guardian (theguardian.com) · Bloomberg (bloomberg.com) · CNBC (cnbc.com) · Financial Times (ft.com) · The Economist (economist.com) · NPR (npr.org) · PBS NewsHour (pbs.org/newshour) · France24 (france24.com) · Deutsche Welle (dw.com) · South China Morning Post (scmp.com) · Foreign Policy (foreignpolicy.com) · Time Magazine (time.com) · The Atlantic (theatlantic.com) · WSJ (wsj.com) · MarketWatch (marketwatch.com) · AFP · UN OCHA (unocha.org) · Times of Israel (timesofisrael.com) · Xinhua (english.news.cn) · Kyodo News (english.kyodonews.net — network-blocked; topic must be supplied by Anand, not searched independently) · Formula 1 (formula1.com) · ESPN (espn.com) · FIFA (fifa.com) · UEFA (uefa.com) · Sky Sports (skysports.com) · BBC Sport (bbc.com/sport)

### Market and Financial Data (for the `markets` JSON block only — not for editorial articles)

Yahoo Finance · Kitco · GoldPrice.org · Bloomberg Markets · CNBC Markets · Trading Economics · Investing.com · MCX India · ICE · S&P Global (spglobal.com) · CME Group · BSE India · NSE India · IBJA Rates · Goodreturns · NASDAQ · LSEG / FTSE Russell · Hang Seng Index (hsi.com.hk) · Nikkei (asia.nikkei.com)

Currency sources (XE.com, Exchange-Rates.org) are no longer part of this registry. Currency conversion is fully automated client-side via Frankfurter/ECB — see Standard IX-D. Do not manually source currency figures.

### Additional Approved Sources

Wikipedia — verifiable factual timeline events only, corroborated by a primary source · biographi.ca (Dictionary of Canadian Biography) — background/historical context, all desks · Government primary sources (PM India, Ontario Legislature, etc.) · ABC Sports Australia · Ole Sports · Asia Sponsorship News · CBC Archives (cbc.ca/archives) — background/historical context

Environment Canada and IMD are no longer part of this registry as production sources. Weather is fully automated client-side via Open-Meteo — see Standard IX-E. Do not manually source weather data.

### Registry Enforcement Rules

1. Confirm outlet is in registry before drafting any article
2. Flag unlisted outlets in the source audit with a recommendation
3. Prediction/preview sites (e.g. Dailysports.net) are never approved
4. Market data sources above are approved for the `markets` JSON block only, not editorial articles
5. At end of every source audit, list any unlisted source used and ask the publisher: "Should this be added to Standard III?"

---

## Standard IV — Recency Requirements

| Section | Maximum Age from Publication Date |
|---|---|
| Current Events | 48 hours |
| Politics | 7 days |
| Economy and Business | 7 days |
| Sport | 7 days (match-report articles tied to a specific event: 48 hours) |
| World Cup | 48 hours for match reports; standings/fixture commentary as relevant |
| Market data | Latest available close or intraday; staleness must be noted in the `markets` block |

This Week in History and Upcoming Events recency rules no longer apply — both are discontinued. Recency is measured from the publication date of the article, not from the time of production.

---

# GROUP 2 — CONTENT STANDARDS

These standards govern the drafting of each individual article and puzzle.

---

## Standard V — Article Integrity

- Every article must be accurate, verified, and representative of confirmed reporting
- No fabricated quotes, invented details, or composite attributions
- Every named person's action or statement must be verified against a live source before inclusion
- Venues, institutions, and proper nouns are high-risk for memory errors — verify each one explicitly (e.g. Rogers Centre is the baseball stadium; BMO Field is the football/soccer stadium)
- If a factual claim cannot be verified against the confirmed source URL, it must be removed or flagged
- The article body is stored as an array of paragraph strings (one string per paragraph, no inline HTML tags) — see Standard IX-B for the exact field

---

## Standard VI — Byline Format

All articles use this exact format: `The Chronicler [Desk Name] Desk · [Day, Month Date, Year]`

Approved desk names: Canada Desk · India Desk · World Desk · Sport Desk · World Cup Desk (while the tournament is active)

GTA Desk is retired as a byline value. GTA-relevant stories run under Canada Desk.

---

## Standard VII — Crunch Puzzle

### Purpose

The Crunch puzzle is a daily number puzzle. It must be solvable, mathematically clean, and computationally verified before inclusion. It may never be included on the basis of manual calculation alone.

### Puzzle Format

- Four single-digit numbers are given alongside a target number
- The player must use all four numbers exactly once with +, -, x, / and brackets to reach the target
- All intermediate steps must produce clean integers — no fractions or decimals at any point in the working

### Verification Requirement

The solution must be computationally verified using Python before inclusion. The script must iterate all permutations and operator combinations across all bracket structures. If no clean solution exists for a chosen set of numbers, new numbers must be selected and verified before proceeding.

### Python Verification Template

```python
from itertools import permutations, product
from fractions import Fraction

def verify_crunch(numbers, target):
    ops = ['+', '-', '*', '/']
    solutions = []
    for perm in permutations(numbers):
        a, b, c, d = [Fraction(x) for x in perm]
        for op1, op2, op3 in product(ops, repeat=3):
            try:
                candidates = [
                    eval(f"(({a}{op1}{b}){op2}{c}){op3}{d}"),
                    eval(f"{a}{op1}(({b}{op2}{c}){op3}{d})"),
                    eval(f"({a}{op1}({b}{op2}{c})){op3}{d}"),
                    eval(f"{a}{op1}({b}{op2}({c}{op3}{d}))"),
                    eval(f"({a}{op1}{b}){op2}({c}{op3}{d})"),
                ]
                for val in candidates:
                    if val == target:
                        solutions.append(f"found")
            except:
                pass
    return solutions
```

### Housing (Open Question)

Crunch has no permanent on-site home as of this version — see Standard IX-G. Continue producing and verifying it every session; record it in the production log under "Puzzles — Pending Integration" until a delivery mechanism is built. Do not skip production just because there is nowhere to publish it yet.

### Production Log Record

Record in the production log: numbers selected, target, solution selected for publication, total distinct solutions found, and confirmation that the Python script was run.

---

## Standard VIII — Word Web Puzzle

### Purpose

The Word Web is a daily word association puzzle. It must be topical, drawn from the session's actual news stories, and contain a deliberate element of difficulty.

### Puzzle Format

- 8 tiles total, displayed in a shuffled 2x4 or 4x2 grid
- 4 tiles share one hidden connection (Group A)
- 4 tiles share a second hidden connection (Group B)
- Tiles must be topical — drawn from the session's top news stories across all desks
- At least one deliberate decoy or ambiguous tile must be included
- Solution is revealed on click or tap

### Housing (Open Question)

Word Web has no permanent on-site home as of this version — see Standard IX-G. Continue producing it every session and record it in the production log under "Puzzles — Pending Integration."

### Production Log Record

Record in the production log: all 8 tiles, Group A connection and members, Group B connection and members, and any deliberate decoys noted.

---

# GROUP 3 — DATA ASSEMBLY & SITE ARCHITECTURE

This standard replaces v2.1's HTML assembly group and old Standard XIII. It is the sole reference for how article and market data is structured, stored, and delivered on the live site.

---

## Standard IX — Site Architecture & Article Data

### IX-A — Architecture Overview

The site is a single `index.html` file: a small client-side application, not a per-edition document. It loads article and market data from monthly JSON files named `articles-YYYY-MM.json`, fetched on page load. There is one file per calendar month. Each file has this top-level shape:

```json
{
  "articles": [ /* article objects, see IX-B */ ],
  "markets":  { /* see IX-C */ }
}
```

Sections (the only valid values for an article's `section` field): `canada` · `india` · `world` · `sport` · `world-cup` (the last only while the tournament is active). There are no nested Politics/Economy/Sports subsections within a desk — all articles in a section sit in a single flat list, and the reader filters by pill (Today / All / Canada / India / World / Sport / World Cup / Markets) and by date, not by a fixed document structure.

### IX-B — Article Object Schema

| Field | Type | Required | Notes |
|---|---|---|---|
| `id` | string | yes | Format: `{section}-{YYYYMMDD}-{3-digit sequence}`, e.g. `canada-20260616-067`. The sequence number runs continuously across the whole day's batch, not reset per section. **Must be entirely lowercase.** |
| `section` | string | yes | One of `canada` / `india` / `world` / `sport` / `world-cup`. **Must be entirely lowercase.** A casing slip here (e.g. `India` instead of `india`) silently makes the article invisible on the live site — this happened on June 16, 2026 and is the reason this rule is explicit. |
| `desk` | string | yes | Display label used in the byline — one of `Canada` / `India` / `World` / `Sport` / `World Cup`. Title-cased, unlike `section`. |
| `date` | string | yes | `YYYY-MM-DD`, the article's publication date. |
| `headline` | string | yes | |
| `summary` | string | yes | Shown in the collapsed card view. |
| `body` | array of strings | yes | One string per paragraph. No inline HTML. |
| `source_url` | string | yes | The confirmed, specific article URL per Standard II. |
| `source_name` | string | yes | Display name of the outlet, e.g. "Reuters". |
| `tags` | array of strings | not yet implemented | Planned field for the multi-tag system (GTA and other localities/topics). Until this ships, do not add it manually — flag GTA-relevant stories in the production log instead (Standard I). |

Before delivering any updated JSON file, validate it parses correctly (e.g. `python3 -m json.tool` or an equivalent load-and-parse check) and explicitly confirm every new article's `id` and `section` are lowercase. This is now a standing pre-delivery check, not optional diligence.

### IX-C — Markets Data Block

The `markets` object holds the latest verified market figures, organized as three arrays: `canada`, `india`, `world`. Each entry has: `name`, `val`, `chg`, `dir` (`up`/`down`/`flat`), `note` (timing/currency context), `type` (`index`/`commodity`).

This block is **overwritten**, not appended, each session — it is a current snapshot, not a historical archive. Sourcing follows Standard III's Market and Financial Data list. Canada and World entries are indices/commodities only. India entries include Sensex, Nifty 50, and IBJA Gold/Silver (per gram, ex-GST). If the publisher provides Saturday/Sunday or holiday figures, carry forward the prior session's close with clear labelling in `note`, as before.

Currency is not part of this block — see IX-D.

### IX-D — Currency Data (Automated)

Currency pairs (CAD/INR, CAD/USD, CAD/GBP, CAD/EUR, INR/USD, INR/CAD, INR/GBP, INR/EUR) fetch live, client-side, from the Frankfurter/ECB API on every page load. This requires no production-time action: no manual lookup, no XE.com screenshots, no reciprocal-consistency check. If a currency figure ever looks wrong, the fix is a site code change (the fetch logic in `index.html`), not a data correction in the JSON file.

### IX-E — Weather and AQI Data (Automated)

Weather and AQI fetch live, client-side, from Open-Meteo on every page load, for a fixed set of cities defined in `index.html`'s own script (Canada: Halifax, Montréal, Ottawa, Toronto, Winnipeg, Edmonton, Vancouver; India: New Delhi, Chandigarh, Kolkata, Mumbai, Hyderabad, Bengaluru, Chennai). This requires no production-time action. There is currently no GTA-specific weather row. If the city list ever needs to change, that is a code change to `index.html`, not a per-session task.

### IX-F — World Cup Live Data

Live match data, scores, and the homepage strip are automated via a Cloudflare Worker (`chronicler-football-proxy.thechronicler.workers.dev`) proxying football-data.org, refreshing every 3 minutes. This requires no manual production work for the homepage. Claude's editorial role during an active tournament is limited to: (a) drafting World Cup match-report and commentary articles as regular article objects (`section: "world-cup"`, `desk: "World Cup"`), and (b) checking each session whether `Specials/world-cup-2026.html` still requires manual updates, since it is unconfirmed whether that page also reads from the live feed.

### IX-G — Specials Pages and Puzzle Housing

Linked from the homepage Specials grid: `world-cup-2026.html` (while active) · `the-chronicler-iran-war-special.html` · `the-chronicler-special-report-canada-jobs-march-2026.html`.

Not linked, built for the old design, pending a new home: `the-chronicler-crunch-archive.html` · `crossbar.html` · `conquest.html`. Word Web has no Specials page at all currently. This is an open, active project — do not propose a fix unless asked; do continue producing Crunch and Word Web daily and logging them per Standard VII/VIII so nothing is lost while the housing question is unresolved.

There is currently no comic strip (Flatland News) anywhere in the live site.

### IX-H — Canonical URL, Instagram, and Disclaimer

All links across `index.html` and every Specials page must use `https://thechronicler.ca` as the sole base URL. The Instagram icon (linking to `https://www.instagram.com/the.chronicler.news`) and the fixed disclaimer text appear once each, in the header/footer of `index.html`, rather than being rebuilt fresh per edition as under v2.1. Verify these are intact at the start of a session rather than re-adding them — they should not normally need to change.

Fixed disclaimer text (never alter):

The Chronicler is an independent news digest compiled from publicly available third-party reporting. It does not employ foreign correspondents and does not claim original reporting unless explicitly stated. All source material remains the copyright of its respective publishers and is cited in each article. The Chronicler is not affiliated with any cited outlet. Market data carries inherent delays; verify with live sources before making financial decisions. This publication is created using AI tools for content curation, research, drafting, and presentation.

### IX-I — Analytics

`index.html` carries both the GA4 tag and the Cloudflare Analytics beacon in its `<head>`. JSON data files carry no analytics tags of any kind — they are pure data. Verify both tags remain intact if `index.html` itself is ever edited; do not add analytics tags to any JSON file.

### IX-J — Monthly File and Site Maintenance

A new `articles-YYYY-MM.json` file must exist before the first article of a new calendar month is published, or the site's fetch for that month will silently fail. The site auto-detects and prepends the current and previous month to its internal month list, but the older end of that list — used for the "Load older editions" pagination — is a hardcoded array inside `index.html`'s script and needs occasional manual upkeep as months accumulate. Check this when starting a new month; it will not fix itself.

---

# GROUP 4 — QUALITY ASSURANCE

These standards govern pre-publication checks and post-publication corrections.

---

## Standard X — Pre-Publication Checklist

Run this checklist before presenting the session's updated JSON to the publisher for final approval.

| # | Check | Requirement |
|---|---|---|
| A | Research slate approval | All section slates for the day approved by publisher in Session 1 |
| B | No-repetition check | No topic repeated from recent publication history without new development; no topic repeated within this session's slates without distinct angle |
| C | Source URLs | Every article has a specific article URL — no homepage-only citations |
| D | Source registry | All sources in Standard III registry; unlisted sources flagged |
| E | Recency | All articles within recency window per Standard IV |
| F | Fabrication check | No article drafted from memory; every URL traceable to a Session 1 search |
| G | JSON validity | File parses without error; no trailing commas; UTF-8 escaping correct |
| H | Casing | Every new `section` and `id` value is entirely lowercase |
| I | ID uniqueness | No duplicate `id` values within the month file |
| J | Bylines | Approved desk names and exact format per Standard VI |
| K | Markets block | Overwritten (not duplicated) with this session's verified figures; no currency entries present |
| L | Crunch puzzle | Python verification complete; logged per Standard VII |
| M | Word Web | 8 tiles, 2 groups, decoy noted; logged per Standard VIII |
| N | Tag flags | GTA and other future-tag candidates flagged in the production log (tags field itself not yet populated) |
| O | Canonical URL | All links use https://thechronicler.ca exclusively |
| P | Instagram and disclaimer | Present and unaltered in index.html header/footer |
| Q | Analytics | GA4 and Cloudflare beacon intact in index.html; absent from JSON files |
| R | Monthly file | Current month's JSON file exists; if a new calendar month has started, file created and the MONTHS array in index.html updated |
| S | World Cup | Homepage strip unaffected (automated); Specials world-cup page updated if still manual |
| T | Proper nouns | All venues, institutions, and named entities verified against sources — not from memory |

---

## Standard XI — Source Audit Table

*(Unchanged from v2.1.)*

A source audit table must be produced for every session before final approval.

Format:

| Desk / Section | Headline (abbreviated) | Source Domain | Pub Date | Std III | Recency | Specific URL |
|---|---|---|---|---|---|---|

Flags used:
- Pass — Compliant
- Warning — Issue noted (describe in summary)
- Fail — Violation (must be corrected before publication)

Summary block after table must state:
- Number of unapproved sources and recommendation
- Number of section/homepage URLs
- Number of recency violations
- Confirmation that all URLs were found via live Session 1 searches

At the end of every audit, explicitly list any unlisted source and ask the publisher: "Should this be added to Standard III?"

---

## Standard XII — Correction Log

| Level | Definition |
|---|---|
| Critical | Fabricated source URL · factual error · wrong attribution · invented quote |
| Significant | Recency violation · unapproved source used without flag · wrong market figure · stale story presented as current |
| Minor | Formatting issue · typo · style inconsistency |

Rules:
- Critical corrections must be resolved before publication
- A correction is applied by locating the specific article object by its `id` within the relevant month's JSON file, editing the affected field directly, and re-validating the file as syntactically correct JSON before resaving. This replaces the old `str_replace`-and-`grep`-against-HTML mechanic.
- A Critical-level correction discovered post-publication triggers a mandatory correction pass and a note in the next session's production log
- All corrections are batched by the publisher after reading the session's new articles and applied together

---

## PRODUCTION LOG FORMAT REFERENCE

Each session's production log must include, in order:

1. Session header (date, JSON file(s) touched — no Vol./No.)
2. Pre-Publication Checklist (Standard X) — pass/fail with notes
3. Research Slate — one table per section showing article, source, URL, search query used
4. Market Data Summary table (Standard IX-C) — indices/commodities only, no currency
5. Crunch Puzzle Audit (Standard VII) — numbers, target, solution, Python confirmation
6. Word Web Notes (Standard VIII) — tiles, groups, decoys
7. Source Audit Table (Standard XI)
8. JSON File Update Notes — which month file(s) touched, article IDs added, whether the markets block was overwritten, whether the MONTHS array in index.html was touched
9. Tag Flags — GTA or other future-tag candidates noted this session, pending the tags field
10. Any flags for publisher review
11. Corrections Log (Standard XII) — if any corrections applied

---

*End of Editorial Brief — The Chronicler — Version 3.0*
*https://thechronicler.ca · thechroniclerstaff@proton.me · @the.chronicler.news*
