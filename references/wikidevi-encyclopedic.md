# Reference: WikiDevi / encyclopedic hardware databases

Credibility tier and limitations of each source mined by the WikiDevi data.

## wikidevi.wi-cat.ru -- Tier A

**Status:** LIVE as of 2026-04-16. Canonical successor / mirror to the
original wikidevi.com (which went offline circa 2019 after its
maintainer died).

**Software:** MediaWiki + Semantic MediaWiki extension. Pages have
both narrative text and structured semantic properties.

**Coverage:**
- ~6037 wireless adapters total per the front-page banner
- 35 USB MediaTek MT7601U adapters catalogued
- 31 USB MediaTek MT7610U adapters catalogued
- 9-13 USB MediaTek MT7612U adapters catalogued
- 6-7 USB MediaTek MT7662U adapters catalogued
- 9+ USB MediaTek MT7921A/AU/AUN adapters catalogued (as of Apr 2024)
- 1 USB MediaTek MT7925U adapter catalogued (Netgear A9000, Jun 2025)
- 0 USB MediaTek MT7902U or MT7927U adapters

**Strengths:**
1. Per-device pages include FCC ID with direct linkout to fcc.report
   and fccid.io -- enables one-click corroboration with FCC OET.
2. Per-device pages include the Windows hardware-id render
   (`USB\VID_xxxx&PID_xxxx`) -- this is the single most reliable
   place to extract a definitive USB VID:PID for the device.
3. Master generation index pages (`List_of_802.11ac/ax/be_Hardware`)
   pack chipset + VID:PID + FCC ID + grant date into single rows --
   the most efficient single source for parsing the WiFi 6 / Wi-Fi 7
   USB MediaTek population.
4. Coverage is best for devices with FCC paperwork. Most retail
   adapters have FCC paperwork; many embedded / Aliexpress-only
   adapters do not.
5. Edit history is preserved -- you can verify a chipset claim
   against the page revision date that introduced it.

**Limitations:**
1. The chipset-summary pages exist only for MT7601U and MT7610U.
   Newer MediaTek USB chip families (MT7612U onwards) have no
   dedicated wiki page; you have to derive device lists by
   grep-searching the generation index pages.
2. Some devices have multiple chipsets listed (e.g. the AVM
   FRITZ!WLAN AC 860 has both MT7610UN and MT7662U) -- a regex parser
   that pulls "first chipset mentioned" will be wrong on these.
3. The "X devices" count on chipset-summary pages can lag the actual
   SKU count in the generation tables. Trust the explicit per-device
   row count over the summary count.
4. No probe / sales / popularity data. wikidevi catalogues existence,
   not popularity. Cross with (linux-hardware.org).
5. Edit-cost barrier means new products can take 6-18 months to be
   added to the wiki after FCC grant. The Netgear A9000 (FCC
   2025-06-17) was added by Apr 2026 -- a 10-month lag.

**Citation pattern:**
- For per-device facts: `https://wikidevi.wi-cat.ru/<Brand>_<Model>` accessed YYYY-MM-DD.
- For chipset-family rollups: `https://wikidevi.wi-cat.ru/MediaTek_<chip>` accessed YYYY-MM-DD.
- For generation-wide rollups: `https://wikidevi.wi-cat.ru/List_of_802.11<gen>_Hardware` accessed YYYY-MM-DD.

## deviwiki.com -- Tier B (degraded mirror)

**Status:** LIVE as of 2026-04-16.

**Coverage:** Partial mirror of wikidevi.com content. The chipset-
summary pages (`/wiki/MediaTek_MT76XXX`) exist for the same set as
wi-cat (MT7601U, MT7610U, MT7612U, MT7650U, MT7662U) and report
similar device counts (35, 30, 12, 2, 6).

**Strengths:**
1. Independent mirror -- if wi-cat dies, deviwiki may still have
   archived content.
2. Useful for cross-confirming device counts.

**Limitations:**
1. Per-device pages are sparser than wi-cat in my sampling. Some
   pages that exist on wi-cat return 404 on deviwiki.
2. No semantic search / Special:Ask interface visible.
3. Less recent edit activity vs. wi-cat. Newer devices (MT7921 USB
   adapters, Netgear A8000, A9000) appear less consistently catalogued.

**Citation pattern:**
- `https://deviwiki.com/wiki/<page>` accessed YYYY-MM-DD.
- Use as a cross-check for wi-cat, NOT as a primary source.

## en.wikipedia.org -- Tier C (very thin)

**Status:** LIVE.

**Coverage:** The MediaTek article mentions:
- 2011 Ralink Technology acquisition gave MediaTek their WiFi product line.
- A few PCIe products (MT7921AN, MT7922AN/RZ616, MT7925AN/RZ717).

**It does NOT cover:**
- USB chip families (MT7601U, MT7610U, MT7612U, MT7650U, MT7662U, etc.)
- Per-product SKUs.
- Linux driver naming (mt76, mt7601u).

The `Comparison_of_open-source_wireless_drivers` article confirms
mt7601u driver is in kernel since 4.2, and mt76 supports
"MediaTek MT76xxx, MT79xxxx" -- but no per-device data.

**Strengths:**
1. Encyclopedic neutrality.
2. Good for verifying high-level corporate facts (Ralink acquisition).

**Limitations:**
1. NOT useful for the question "which adapters are most used by
   Linux users." Wikipedia is too coarse. Excluded as a substantive
   source.

## techpowerup.com -- Tier ? (URL-not-found; excluded)

**Status:** TechPowerUp's WiFi card database is not at the expected
URLs (`/wifi-cards/?mfgr=MediaTek` returns 404). They have GPU and
CPU databases at similar paths, but no WiFi card database surfaced
in this run.

**Excluded from this report.** A future capture could probe TechPowerUp's sitemap.xml for `/wifi-card/` or `/wifi/` paths, or try their full-text search at `/search/?q=MT7921`. If no WiFi database surfaces, TechPowerUp is not-applicable to this question.

## web.archive.org snapshots of original wikidevi.com -- Tier A (rate-limited)

**Status:** HTTP 429 rate limit on the Wayback Machine during capture.

**Theoretical coverage:** wikidevi.com had snapshots as recent as 2019. Pre-2019 device entries that may have been lost during the wi-cat migration could be retrievable with a low-rate scraper (1 req per 10s). Not urgent unless wi-cat itself goes down.

## How this source fits the dataset

1. **wi-cat is the primary device-catalogue source.** Its structured data has VID:PID, FCC ID, chipset, antenna config per row.
2. **For each VID:PID that linux-hardware.org probe data covers**, the corresponding wi-cat device page enriches with FCC ID and antenna config. The combination produces a single-row record per device: VID:PID, FCC ID, chipset, antenna, install-base probes, retail brand+model.
3. **For each FCC ID**, wi-cat's `?search=<fcc-id>` confirms the chipset and surfaces the actual product page. Many FCC entries are OEM modules; wi-cat reveals what brand/model that OEM module ended up shipping under.
4. **Treat the wi-cat device count per chip family as a "ceiling" for
   product diversity**, not for install base. There are 35 distinct
   MT7601U USB SKUs in the wiki; this tells you the chip is a
   commodity, not how many of them are plugged into Linux machines.
5. **Date fields = FCC grant dates, not retail launch dates.** Add
   ~3-12 months to estimate retail availability.

## Provenance receipts

Every claim derived from this source cites a specific wi-cat URL plus
fetch timestamp. Raw HTML and parsed JSON captures are retained
privately (5.6 MB, 112 files at capture time).
