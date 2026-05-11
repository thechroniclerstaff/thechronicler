# The Chronicler — Editorial Brief
**Version:** 2.2 (Master)
**Effective from:** Vol. I, No. 40 (Saturday, May 9, 2026)
**Supersedes:** Editorial Brief v2.1 (archived)
**Publisher/Editor:** Anand
**Production Platform:** Claude (Anthropic) + Cloudflare Pages
**Canonical URL:** https://thechronicler.ca
**Instagram:** @the.chronicler.news
**Contact:** thechroniclerstaff@proton.me

---

## HOW TO USE THIS DOCUMENT

This is the single authoritative reference for all production standards. It is injected at the start of every production session. Standards are numbered I–XIII and ordered to mirror the actual production sequence — from the first action taken before any drafting begins, through to the final file check before publication.

The production log (separate document) records edition-specific data: article research slates, publisher approvals, market data, corrections, and source audits. It references standards by number but does not restate them. When a new standard is introduced or an existing one is amended, the change is recorded in the production log under the edition where it was identified, and then reflected here in the next version of this brief.

**Archive note:** All prior versions of this brief and production logs v1–v18 are preserved in the archived files. When checking decisions made in editions No. 1–22, consult those archive files. Editorial Brief v2.1 covers editions No. 23–39.

---

## PUBLICATION IDENTITY

| Field | Value |
|---|---|
| Name | The Chronicler |
| URL | https://thechronicler.ca |
| Instagram | @the.chronicler.news |
| Contact | thechroniclerstaff@proton.me |
| Style | Independent AI-assisted daily digital broadsheet |
| Aesthetic | Aged-paper broadsheet — Playfair Display body, UnifrakturMaguntia masthead |

### CSS Variables (required in every edition)

```css
:root {
  --ink:   #141008;
  --aged:  #f4edd6;
  --cream: #fdf9ee;
  --rust:  #8b2513;
  --gold:  #a07800;
  --grey:  #635d4e;
  --rule:  #d0c5a0;
  --green: #1a6b3c;
}
```

### Required Google Fonts
- UnifrakturMaguntia — masthead only
- Playfair Display — headlines
- Libre Baskerville — body text
- Source Sans 3 — UI labels, buttons, captions
- Caveat — Funnies / handwritten elements

---

# GROUP 1 — PRE-PRODUCTION GATES

These standards must be satisfied before any article copy is drafted. Violation of any Group 1 standard invalidates the draft.

---

## Standard I — Article-by-Article Production Workflow (Mandatory Gate)

### Purpose

The "research everything, draft everything, generate one file" workflow allows training-data memory contamination to pass all automated audit checks undetected. In Vol. I No. 23 (first attempt), 35 of 51 source URLs were fabricated — articles drafted from background knowledge with plausible URLs constructed post-hoc. This standard eliminates that failure mode by making publisher verification a mandatory gate at every article.

### The Two-Session Structure

**Session 1 — 10:00 PM (Research and Slate)**

Claude conducts all live searches for the upcoming edition. Research slates are presented one section at a time in this fixed sequence:

1. Canada — Current Events
2. Canada — Politics
3. Canada — Economy and Business
4. Canada — Sports
5. Canada — This Week in History
6. GTA — Current Events
7. GTA — Politics
8. GTA — Economy and Business
9. GTA — Sports
10. India — Current Events
11. India — Politics
12. India — Economy and Business
13. India — Sports
14. India — This Week in History
15. World — Current Events
16. World — Politics
17. World — Economy and Business
18. World — Sports
19. World — This Week in History
20. Upcoming Events — all desks consolidated

Each section slate (positions 1–19) shows per article: proposed headline, source domain, confirmed specific URL, and search query used. Publisher approves or rejects each section slate before the next is presented. No section slate is presented until the previous one is approved.

The Upcoming Events slate (position 20) shows per event: desk assignment, event name, date, venue, and source URL. Minimum 3 and maximum 5 events per desk; all events must fall within 14 days of the edition date; only publicly attendable social, cultural, sporting, or entertainment events qualify. Publisher approves the full consolidated list. Claude routes approved events to the correct desk sections during HTML assembly.

Session 1 closes only when all 20 slates are approved.

**Session 2 — 5:00 AM (Production)**

Claude drafts all articles from approved URLs, pulls live weather, market, and currency data, assembles the HTML, and presents the completed edition for publisher review. Publisher reads the complete edition, batches corrections, corrections are applied, and the edition is published by 6:30 AM ET.

### The Four Phases — No Phase May Be Skipped

**Phase I — Research Slate (Session 1)**

Required upload: The previous published edition HTML must be uploaded at the start of Session 1. Claude reads it before conducting any searches in order to enforce the no-repetition rule below.

Article counts per section (target):
- Current Events: 3 articles
- Politics: 3 articles
- Economy and Business: 3 articles
- Sports: 3 articles
- This Week in History: 1 article
- Upcoming Events: 3–5 events per desk

These are targets, not hard minimums. If Claude cannot identify sufficient qualifying stories for any section during Session 1 research, it flags the shortfall in the slate with the count found and the reason. The publisher decides whether to accept the reduced count or direct an alternative approach. A section may not be padded with weak or marginal stories solely to reach the target count.

No-repetition rule: No proposed headline may cover a topic already covered in the previous published edition unless a confirmed new development in that story has occurred and is evidenced by the live search result. No proposed headline may cover a topic already covered in any other section or desk within the current edition unless there is an aspect of the topic not covered by the first headline. If a topic recurs without a new development or a distinct angle, Claude must find an alternative story before presenting the slate. This check is performed against the uploaded previous edition HTML before any slate is presented, and also against all approved slates of the edition in progress.

Exceptions: The same historical event may be used by two desks in This Week in History only if each entry provides a meaningfully distinct national or regional angle. A sports result may be covered by both the Canada desk and the GTA desk only if the framing is demonstrably distinct — national angle for Canada, local angle for GTA.

**Phase II — Article-by-Article Drafting (Session 2)**

All articles are drafted from Session 1 approved URLs. If any source has been materially updated or superseded overnight, Claude stops, flags the original and new information explicitly, and waits for publisher direction before proceeding with that article. Articles are drafted section by section, desk by desk, in fixed order.

**Phase III — Data Verification (Session 2)**

Weather, market, and currency data are fetched live in Session 2 before HTML assembly. Publisher confirms or corrects values. Indian market data may be provided directly by the publisher from a live screen if search results are lagging — this is the approved workflow.

**Phase IV — HTML Assembly (Session 2)**

Assembled only after every article and every data point has received explicit publisher approval. Generated once. Post-generation errors corrected via str_replace and documented in the corrections log.

### Mandatory Gate Conditions

All must be true before HTML generation:

- Previous edition HTML uploaded and read at Session 1 start
- All 20 research slates approved by publisher in Session 1
- No topic repeated from the previous edition without a confirmed new development
- No topic repeated across sections or desks within the current edition without a distinct angle
- All section article counts meet targets or shortfalls flagged and approved by publisher
- Every article drafted from a Session 1 approved URL
- Any overnight source change flag resolved by publisher before drafting proceeds
- All weather, market, and currency data verified in Session 2
- Crunch puzzle solution computationally verified via Python
- Word Web tiles and groups confirmed as topically accurate

---

## Standard II — Source-First Rule and Fabrication Prevention

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

1. The article reads like background context or analysis rather than a specific recent event — verify the live source confirms the specific development before accepting the draft
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

### About This Registry

The registry is unified — sources are not assigned to specific desks. Any approved source may be used for any desk. A trusted outlet does not become less credible because it is covering a story outside its primary geography. The source audit column reads "Standard III" with a pass or fail; cross-desk use is never a flag.

### Approved Editorial Sources

CBC News (cbc.ca) · Globe and Mail (theglobeandmail.com) · CTV News (ctvnews.ca) · Global News (globalnews.ca) · National Post (nationalpost.com) · BNN Bloomberg (bnnbloomberg.ca) · Financial Post (financialpost.com) · TSN (tsn.ca) · Sportsnet (sportsnet.ca) · Radio-Canada / RCI (ici.radio-canada.ca) · CP24 (cp24.com) · The Canadian Press · Statistics Canada · Bank of Canada · Government of Canada (canada.ca) · Maclean's · iPolitics · The Walrus (thewalrus.ca) · Toronto Star (thestar.com) · Toronto Sun (torontosun.com) · Metrolinx News (metrolinx.com/en/news) · Toronto FC (torontofc.ca) · BlogTO (blogto.com — current events only) · City of Toronto Newsroom · Brampton Guardian · Markham Economist and Sun · Oakville News · Whitby This Week (durhamregion.com/whitby-this-week) · The Local (thelocal.to) · Region of Peel / York Region / Durham Region official sites · montreal.citynews.ca (Montreal CityNews) · The Hindu (thehindu.com) · The Indian Express (indianexpress.com) · Hindustan Times (hindustantimes.com) · Business Standard (business-standard.com) · LiveMint (livemint.com) · Financial Express (financialexpress.com) · Economic Times (economictimes.indiatimes.com) · NDTV (ndtv.com) · The Tribune (tribuneindia.com) · Deccan Herald (deccanherald.com) · India Today (indiatoday.in) · Frontline (frontline.thehindu.com) · ANI — Asian News International (aninews.in — primary newswire; citable at own domain or via republishing outlet) · PTI — Press Trust of India (ptinews.com — national newswire; citable at own domain or via republishing outlet) · PM India (pmindia.gov.in — primary government source) · ESPNcricinfo (espncricinfo.com) · IMD (imd.gov.in) · Reuters (reuters.com) · Associated Press (apnews.com) · BBC News (bbc.com/news) · Al Jazeera English (aljazeera.com) · CNN (cnn.com) · The Guardian (theguardian.com) · Bloomberg (bloomberg.com) · CNBC (cnbc.com) · Financial Times (ft.com) · The Economist (economist.com) · NPR (npr.org) · PBS NewsHour (pbs.org/newshour) · France24 (france24.com) · Deutsche Welle (dw.com) · South China Morning Post (scmp.com) · Foreign Policy (foreignpolicy.com) · Time Magazine (time.com) · The Atlantic (theatlantic.com) · WSJ (wsj.com) · MarketWatch (marketwatch.com) · AFP · UN OCHA (unocha.org) · Times of Israel (timesofisrael.com) · Formula 1 (formula1.com) · ESPN (espn.com) · FIFA (fifa.com) · UEFA (uefa.com) · Sky Sports (skysports.com) · BBC Sport (bbc.com/sport)

### Market and Financial Data (cards only — not for editorial articles)

XE.com (primary for all currency pairs) · Exchange-Rates.org (secondary currency) · Yahoo Finance · Kitco · GoldPrice.org · Bloomberg Markets · CNBC Markets · Trading Economics · Investing.com · MCX India · ICE · S&P Global (spglobal.com) · CME Group · BSE India (bseindia.com) · NSE India (nseindia.com) · IBJA Rates · Goodreturns · NASDAQ · LSEG / FTSE Russell · Hang Seng Index (hsi.com.hk) · Nikkei (asia.nikkei.com)

### Additional Approved Sources

Wikipedia — verifiable factual timeline events only, corroborated by a primary source · Government primary sources (PM India, Ontario Legislature, etc.) · Environment Canada (weather) · ABC Sports Australia · Ole Sports · Asia Sponsorship News · CBC Archives (cbc.ca/archives — history articles only) · biographi.ca (Dictionary of Canadian Biography, University of Toronto / Université Laval — history articles only)

### Registry Enforcement Rules

1. Confirm outlet is in registry before drafting any article
2. Flag unlisted outlets in the source audit with a recommendation
3. Prediction/preview sites (e.g. Dailysports.net) are never approved — replace with approved sports desks
4. Market data sources above are approved for market/currency cards only, not editorial articles
5. At end of every source audit, list any unlisted source used and ask the publisher: "Should this be added to Standard III?"

---

## Standard IV — Recency Requirements

| Section | Maximum Age from Publication Date |
|---|---|
| Current Events | 48 hours |
| Politics | 7 days |
| Economy and Business | 7 days |
| Sports | 7 days |
| This Week in History | Historical — exact calendar-date anniversary; recency exempt |
| Upcoming Events | Within 14 days of edition date |
| Market data | Latest available close or intraday; staleness must be noted on card |

Recency is measured from the publication date of the edition, not from the time of production.

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

---

## Standard VI — Byline Format

All articles use this exact format: The Chronicler [Desk Name] Desk · [Day, Month Date, Year]

Approved desk names: Canada Desk · GTA Desk · India Desk · World Desk

---

## Standard VII — Crunch Puzzle

### Purpose

The Crunch puzzle is a daily number puzzle published in The Chronicler Funnies section at the end of each edition. It must be solvable, mathematically clean, and computationally verified before inclusion. It may never be included on the basis of manual calculation alone.

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

### Production Log Record

Record in the production log: numbers selected, target, solution selected for publication, total distinct solutions found, and confirmation that Python script was run. This record feeds the Crunch archive page maintained in Specials.

---

## Standard VIII — Word Web Puzzle

### Purpose

The Word Web is a daily word association puzzle published in The Chronicler Funnies section at the end of each edition, alongside the Crunch puzzle. It must be topical, drawn from the edition's actual news stories, and contain a deliberate element of difficulty.

### Puzzle Format

- 8 tiles total displayed in a shuffled 2x4 or 4x2 grid
- 4 tiles share one hidden connection (Group A)
- 4 tiles share a second hidden connection (Group B)
- Tiles must be topical — drawn from the edition's top news stories across all desks
- At least one deliberate decoy or ambiguous tile must be included to increase difficulty
- Solution is revealed on click or tap

### Production Log Record

Record in the production log: all 8 tiles, Group A connection and members, Group B connection and members, and any deliberate decoys noted.

---

# GROUP 3 — HTML ASSEMBLY

This standard governs the complete HTML build of every edition. It is the sole reference for layout, design, and structural decisions during Session 2.

---

## Standard IX — Edition Assembly

### IX-A — Section Order and Structure

Every edition is assembled in this exact top-to-bottom sequence:

1. Document head — charset, viewport, title, Google Fonts link, CSS variables and styles
2. Two-tier sticky header — Mini-masthead bar (Tier 1) · Navigation bar with dropdowns (Tier 2)
3. Masthead — Publication name in UnifrakturMaguntia · Tagline · Edition date · Instagram icon · Vol. and No.
4. Breaking news bar — var(--rust) background · uppercase Source Sans 3
5. Canada desk — Current Events · Politics · Economy and Business · Sports · This Week in History · Upcoming Events
6. GTA desk — Current Events · Politics · Economy and Business · Sports · Upcoming Events
7. India desk — Current Events · Politics · Economy and Business · Sports · This Week in History · Upcoming Events
8. World desk — Current Events · Politics · Economy and Business · Sports · This Week in History · Upcoming Events
9. The Chronicler Funnies — Word Web · Crunch · Flatland News
10. Footer — publication name · Vol. and No. · date · canonical URL · Instagram icon · disclaimer
11. Shared read-aloud script — single script block at bottom of body

No deviation from this order is permitted.

Required anchor IDs on every section span:

canada-current · canada-politics · canada-economy · canada-sports · canada-history · canada-events
gta-current · gta-politics · gta-economy · gta-sports · gta-events
india-current · india-politics · india-economy · india-sports · india-history · india-events
world-current · world-politics · world-economy · world-sports · world-history · world-events
comics

---

### IX-B — Weather Card Design

Weather cards appear at the top of every Current Events section in a single-row grid.

Canada — 5 cities: Toronto · Montreal · Ottawa · Edmonton · Vancouver
GTA — 5 areas: Toronto · Brampton · Markham · Oakville · Whitby
India — 6 cities: New Delhi · Hyderabad · Mumbai · Bengaluru · Chennai · Pune
World desk: No weather cards.

Grid CSS:
- Canada/GTA: grid-template-columns: repeat(5, minmax(0, 1fr))
- India: grid-template-columns: repeat(6, minmax(0, 1fr))

Each card must include:
- City name
- Current temperature in degrees Celsius
- High / low range
- Conditions (text and emoji icon)
- AQI with colour-coded badge: Good (green) · Moderate (amber) · Poor (red) · Very Poor (purple)
- Wind and humidity row
- 3-day forecast strip at the bottom (day name, emoji, high/low)

Card styling:
- Canada/GTA top border: var(--ink)
- India top border: var(--rust)

Source note below cards: "Weather data: Environment Canada / IMD. Updated [time]."

---

### IX-C — Market Card Design

Market cards appear at the top of every Economy and Business section, before the articles.

Canada — 7 cards: S&P/TSX · WTI Crude · Gold (USD/oz) · CAD/USD · CAD/INR · CAD/EUR · CAD/GBP

India — 7 cards: Sensex · Nifty 50 · Gold (INR/10g, 24K) · INR/USD · INR/CAD · INR/GBP · INR/EUR

World — 7 cards (indices only — no currency cards): DJIA · NASDAQ-100 · S&P 500 · FTSE 100 · Nifty 50 · Hang Seng Index · Nikkei 225

GTA desk: No market cards.

Each card shows: instrument name · current value · change in points and percentage · source label · data time note.
Positive change: green · Negative change: red · Flat: amber.

A market note block must precede the grid explaining data timing. A market sources line with clickable links must follow the grid.

Indian markets timing note: NSE/BSE close at 5:00 AM ET. For Session 2 early-morning production runs, Indian market data reflects the previous day's close. If the publisher provides live intraday readings directly from a screen, those values are used and timestamped accordingly — this is the approved workflow.

---

### IX-D — Currency Rate Sourcing

- All currency rates must be sourced live from XE.com per pair during Session 2
- XE.com URL format: https://www.xe.com/currencyconverter/convert/?Amount=1&From=CAD&To=INR
- Cross-multiplication to derive rates is strictly prohibited
- Each pair must be fetched independently
- Rates recalled from training data are never acceptable
- A reciprocal consistency check must be performed and recorded in the production log: CAD/INR multiplied by INR/CAD must equal 1.000 (plus or minus 0.002 tolerance)
- The source line on each currency card must cite XE.com and the date of retrieval

Required currency pairs:

| Pair | Appears in |
|---|---|
| CAD/USD | Canada market cards |
| CAD/INR | Canada market cards |
| CAD/EUR | Canada market cards |
| CAD/GBP | Canada market cards |
| INR/USD | India market cards |
| INR/CAD | India market cards |
| INR/GBP | India market cards |
| INR/EUR | India market cards |

---

### IX-E — Two-Tier Sticky Header

Every edition HTML must include a two-tier sticky header fixed at the top of the viewport.

Tier 1 — Mini-Masthead Bar:
- Background: var(--ink) · Bottom border: 2px solid var(--rust)
- Left: The Chronicler in UnifrakturMaguntia 22px, var(--aged), linking to https://thechronicler.ca
- Right: Top button (smooth-scrolls to id="top") and Home button (links to https://thechronicler.ca)
- Button style: transparent background · 1px solid border · 10px Source Sans 3 · all-caps · hover fills var(--rust)

Tier 2 — Navigation Bar:
- Background: #1e1608
- Dropdowns on hover (desktop) and tap (mobile via JS .open class toggle)

| Nav Item | Dropdown Links |
|---|---|
| Canada | Current Events · Politics · Economy and Business · Sports · This Week in History · Upcoming Events |
| GTA | Current Events · Politics · Economy and Business · Sports · Upcoming Events |
| India | Current Events · Politics · Economy and Business · Sports · This Week in History · Upcoming Events |
| World | Current Events · Politics · Economy and Business · Sports · This Week in History · Upcoming Events |
| Funnies | Word Web · Crunch · Flatland News |

Notes:
- GTA dropdown has no This Week in History link
- Funnies is a dropdown with three items in this exact order: Word Web · Crunch · Flatland News
- Crunch links to both the in-edition puzzle anchor and the Crunch archive page in Specials

Archive exemption: Do not backfill editions No. 1–22.

---

### IX-F — Read-Aloud Buttons

Every article must include a read-aloud button using the browser-native Web Speech API.

- Button placement: Immediately after the byline div, before the first paragraph
- CSS class: read-btn
- Style: 10px Source Sans 3 · var(--rust) · no border · transparent background · cursor pointer
- Idle label: Read aloud · Active label: Stop

Behaviour:
1. Click reads all paragraph text in the parent article (source line excluded)
2. Click again cancels and resets label
3. Starting a new article while one is reading cancels current and starts new
4. Unsupported browsers — button hidden silently

Implementation: Single shared script block at bottom of body using event delegation. No per-article inline scripts.

Archive exemption: Do not backfill editions No. 1–22.

---

### IX-G — Upcoming Events Layout

Every desk must include an Upcoming Events section. Events are researched and approved as a consolidated slate in Session 1 (position 20) and routed to the correct desk during HTML assembly in Session 2.

Qualifying events only:
- Publicly attendable social, cultural, sporting, or entertainment events
- Music concerts · theatre productions · festivals · exhibitions · sporting events open to the public

These do NOT qualify:
- Electoral processes
- Budget tablings or government proceedings
- Parliamentary or legislative sessions
- Court proceedings

Event recency: All events must fall within 14 days of the edition date.

Layout rules by desk:

Canada, India, World desks:
- Upcoming Events appears alongside This Week in History in a two-column layout
- History column: wider (approximately 60% width)
- Upcoming Events column: narrower (approximately 40% width)
- Both columns sit within a single div.bottom-row two-column grid

GTA desk:
- No This Week in History section
- Upcoming Events appears as its own full-width standalone section
- Uses div.ev-standalone class

Event display format (all desks): Each event shown as a small card or table row with: event name · date · venue · brief description.

---

### IX-H — Games Section Layout

The Games section is part of The Chronicler Funnies. It appears after the Funnies section banner and before Flatland News.

Layout:
- Two-column div.bottom-row grid
- Left panel: Crunch puzzle (Standard VII)
- Right panel: Word Web puzzle (Standard VIII)
- Equal column width — 50/50 split

Crunch panel must include:
- Puzzle title: Crunch
- The four numbers and target displayed prominently
- Solution revealed on click/tap
- Link to the Crunch archive page in Specials

Word Web panel must include:
- Puzzle title: Word Web
- 8 tiles in a shuffled 2x4 or 4x2 grid
- Solution revealed on click/tap — Group A connection and members · Group B connection and members

Styling:
- Panel backgrounds: var(--aged)
- Borders: var(--rule)
- Title labels: Playfair Display · var(--rust)
- Tile styling: Source Sans 3 · var(--ink) background · var(--aged) text

---

### IX-I — Flatland News (Comic Strip)

The comic strip appears as the final editorial element of The Chronicler Funnies, after the Games section.

Title format: The strip is always titled "Flatland News — [Topic]" where the topic is drawn from the edition's actual news. No further attribution is required beyond the date. Example: "Flatland News — The Peace Table."

Content requirements:
- Minimum 3 panels, typically 4
- Content must be topical satire drawn from the edition's actual news stories
- Rendered as inline SVG in the HTML
- Uses Caveat font for handwritten captions and speech bubbles

Styling: Section follows immediately after the Games section. Comic strip precedes the footer. No disclaimer block between the comic and the footer — disclaimer appears in footer only.

---

### IX-J — Disclaimer

Fixed disclaimer text (never alter):

The Chronicler is an independent news digest compiled from publicly available third-party reporting. It does not employ foreign correspondents and does not claim original reporting unless explicitly stated. All source material remains the copyright of its respective publishers and is cited in each article. The Chronicler is not affiliated with any cited outlet. Market data carries inherent delays; verify with live sources before making financial decisions. This publication is created using AI tools for content curation, research, drafting, and presentation.

Placement:
- In the footer of every edition HTML
- In the footer of index.html
- Never under the masthead
- Never as a standalone block between the Funnies and the footer

---

### IX-K — Instagram Icon

An SVG Instagram icon with the handle @the.chronicler.news must appear in:
- Top of the masthead (right side of mast-top row) — every edition HTML
- Footer of every edition HTML
- Top and bottom of index.html

Icon links to: https://www.instagram.com/the.chronicler.news

---

### IX-L — Canonical URL

All links in all published files must use https://thechronicler.ca as the sole base URL. No other domain may appear in any published file under any circumstances.

This applies to: every edition HTML · index.html · the Crunch archive page · any special edition or supplementary file.

---

# GROUP 4 — QUALITY ASSURANCE

These standards govern pre-publication checks and post-publication corrections.

---

## Standard X — Pre-Publication Checklist

Run this checklist against the completed HTML before presenting to publisher for final approval.

| # | Check | Requirement |
|---|---|---|
| A | Research slate approval | All 20 slates approved by publisher in Session 1 |
| B | No-repetition check | No topic repeated from previous edition without new development; no topic repeated across sections without distinct angle |
| C | Source URLs | Every article has a specific article URL — no homepage-only citations |
| D | Source registry | All sources in Standard III registry; unlisted sources flagged |
| E | Recency | All articles within recency window per Standard IV |
| F | Fabrication check | No article drafted from memory; every URL traceable to a Session 1 search |
| G | Section order | Canada then GTA then India then World then Funnies then Footer |
| H | Section structure | All subsections per desk in correct order; article counts at target or shortfall approved |
| I | Weather cards | Canada/GTA 5 cards; India 6 cards; single row grid; all elements present including AQI and 3-day forecast |
| J | Market cards | Canada 7 · India 7 · World 7 · GTA 0; all values sourced live; timing noted |
| K | Currency rates | All pairs fetched live from XE.com; reciprocal consistency check passed |
| L | Crunch puzzle | Python verification complete; solution confirmed; recorded in production log |
| M | Word Web | 8 tiles; 2 groups; decoy noted; solution confirmed |
| N | Upcoming Events | 3–5 events per desk; within 14 days; qualifying events only; layout correct per desk |
| O | Two-tier sticky header | Present with all anchor IDs per Standard IX-E |
| P | Funnies dropdown | Three items: Word Web · Crunch · Flatland News — in that order |
| Q | Games section | Appears before Flatland News; Crunch left · Word Web right |
| R | Flatland News | Titled "Flatland News — [Topic]"; date only; no further attribution |
| S | Read-aloud buttons | On every article; shared script at bottom of body |
| T | Instagram icon | In masthead and footer of edition; top and bottom of index |
| U | Disclaimer | In footer of edition and index only; never under masthead; never as standalone block |
| V | Canonical URL | All links use https://thechronicler.ca exclusively |
| W | Index updated | New edition as latest; prior edition moved to archive; Special Editions registry consulted |
| — | Date selector updated | `max` date extended to current edition; new `editionMap` entry added; card title and subtitle reflect current range |
| X | Bylines | All articles use approved desk names and correct format per Standard VI |
| Y | Proper nouns | All venues, institutions, and named entities verified against sources — not from memory |
| Z | Crunch archive | Updated with current edition's puzzle entry |

---

## Standard XI — Source Audit Table

A source audit table must be produced for every edition at the end of Session 2, before final HTML approval.

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

All corrections are recorded in the production log under the edition where they occurred.

| Level | Definition |
|---|---|
| Critical | Fabricated source URL · factual error · wrong attribution · invented quote |
| Significant | Recency violation · unapproved source used without flag · wrong market figure · stale story presented as current |
| Minor | Formatting issue · typo · style inconsistency |

Rules:
- Critical corrections must be resolved before publication
- A Critical-level correction discovered post-publication triggers a mandatory correction pass via str_replace and a note in the next edition's production log
- All corrections are batched by the publisher after reading the complete edition HTML and applied together in Session 2

---

## Standard XIII — Index File Requirements

The index file (index.html) must contain:
- Latest Edition block: Current edition only, with date and link
- Archive section: All prior editions, newest first
- Special Editions section: Only confirmed entries from the Special Editions Registry below
- Date selector widget: Covers the full Vol. I run — must be updated every edition (see below)
- Instagram icon and link at top and bottom
- Full disclaimer in footer
- All links use https://thechronicler.ca

### Date Selector Update (required every Session 2)

The archive section contains an interactive date selector widget that maps every publication date to its edition file. Two changes are required at every index update:

1. **Extend the `max` attribute** on the `<input type="date">` element to the current edition's date (format: YYYY-MM-DD)
2. **Add the new entry** to the `editionMap` JavaScript object in the format:
   `'YYYY-MM-DD': {no: [N], label: 'Vol. I, No. [N]'},`

The card title and subtitle must also reflect the current edition range. As of Vol. I, No. 39 the card reads: *"Vol. I, Nos. 1–39 · March 8 – May 8, 2026."* Update both the `archive-no` div and the `archive-sub` div to match the new highest edition number and date.

Gap dates — days on which no edition was published — must not be added to the map. The widget already handles these gracefully by displaying a "no edition published" message.

Special Editions Registry (current):

| File | Title | Published With |
|---|---|---|
| the-chronicler-special-report-canada-jobs-march-2026.html | Special Report: Canada's Jobs Crisis — March 2026 | Vol. I, No. 8 (March 15, 2026) |
| the-chronicler-crunch-archive.html | The Chronicler Crunch — Daily Puzzle Archive | Ongoing — updated every edition |

Registry rules:
- Consult this registry before every index generation
- Do not add entries to the Specials section without publisher direction
- The Crunch archive is updated at the end of every Session 2 alongside the index file
- The Eid ul-Fitr corrected variant (the-chronicler-vol1-no14-1.html) appears in the Archive section under No. 14 — not in Special Editions

---

## PRODUCTION LOG FORMAT REFERENCE

Each edition's production log must include, in order:

1. Edition header (Vol., No., date, filename, index version)
2. Pre-Publication Checklist (Standard X) — pass/fail with notes
3. Research Slate — one table per section showing article, source, URL, search query used
4. Currency Rate Verification table (Standard IX-D)
5. Market Data Summary table (Standard IX-C)
6. Crunch Puzzle Audit (Standard VII) — numbers, target, solution, Python confirmation
7. Word Web Notes (Standard VIII) — tiles, groups, decoys
8. Source Audit Table (Standard XI)
9. Index Update Notes (Standard XIII)
10. Any flags for publisher review
11. Corrections Log (Standard XII) — if any corrections applied

---

*End of Editorial Brief — The Chronicler — Version 2.2*
*https://thechronicler.ca · thechroniclerstaff@proton.me · @the.chronicler.news*
