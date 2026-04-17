# References - eBay sold listings + AliExpress order counts

Access timestamps: 2026-04-17 04:00-04:11 UTC unless noted otherwise.

## eBay sold-completed-listings URLs (all accessed 2026-04-17)

| Adapter | URL |
|---------|-----|
| Alfa AWUS036ACM | https://www.ebay.com/sch/i.html?_nkw=alfa+AWUS036ACM&_sop=13&LH_Sold=1&LH_Complete=1 |
| Alfa AWUS036AXML | https://www.ebay.com/sch/i.html?_nkw=alfa+AWUS036AXML&_sop=13&LH_Sold=1&LH_Complete=1 |
| Alfa AWUS036AXM | https://www.ebay.com/sch/i.html?_nkw=alfa+AWUS036AXM&_sop=13&LH_Sold=1&LH_Complete=1 |
| Alfa AWUS036ACHM | https://www.ebay.com/sch/i.html?_nkw=alfa+AWUS036ACHM&_sop=13&LH_Sold=1&LH_Complete=1 |
| Netgear A8000 | https://www.ebay.com/sch/i.html?_nkw=netgear+A8000&_sop=13&LH_Sold=1&LH_Complete=1 |
| Netgear A7500 | https://www.ebay.com/sch/i.html?_nkw=netgear+A7500&_sop=13&LH_Sold=1&LH_Complete=1 |
| Netgear A9000 | https://www.ebay.com/sch/i.html?_nkw=netgear+A9000+wifi&_sop=13&LH_Sold=1&LH_Complete=1 |
| Netgear A6210 | https://www.ebay.com/sch/i.html?_nkw=netgear+A6210&_sop=13&LH_Sold=1&LH_Complete=1 |
| TP-Link Archer TXE50UH | https://www.ebay.com/sch/i.html?_nkw=TP-Link+Archer+TXE50UH&_sop=13&LH_Sold=1&LH_Complete=1 |
| BrosTrend AX9L | https://www.ebay.com/sch/i.html?_nkw=BrosTrend+AX9L&_sop=13&LH_Sold=1&LH_Complete=1 |
| EDUP EP-AX1672 | https://www.ebay.com/sch/i.html?_nkw=EDUP+EP-AX1672&_sop=13&LH_Sold=1&LH_Complete=1 |
| Comfast CF-952AX | https://www.ebay.com/sch/i.html?_nkw=Comfast+CF-952AX&_sop=13&LH_Sold=1&LH_Complete=1 |
| Panda PAU0F | https://www.ebay.com/sch/i.html?_nkw=Panda+Wireless+PAU0F&_sop=13&LH_Sold=1&LH_Complete=1 |
| Asus USB-AC54 | https://www.ebay.com/sch/i.html?_nkw=asus+USB-AC54&_sop=13&LH_Sold=1&LH_Complete=1 |
| TP-Link TL-WDN6200 | https://www.ebay.com/sch/i.html?_nkw=tp-link+TL-WDN6200&_sop=13&LH_Sold=1&LH_Complete=1 |
| Generic mt7921 USB | https://www.ebay.com/sch/i.html?_nkw=mt7921+usb+wifi+adapter&_sop=13&LH_Sold=1&LH_Complete=1 |

Sort param `_sop=13` is most-recently-ended; filters `LH_Sold=1&LH_Complete=1` restrict to sold-and-completed listings in the 90-day visible window. Numbers cited in the dataset come from the per-card `s-card` DOM structure introduced in eBay's 2025 redesign.

## AliExpress URLs (all accessed 2026-04-17 04:09-04:11 UTC)

| Search | URL |
|--------|-----|
| mt7921 usb wifi | https://www.aliexpress.com/wholesale?SearchText=mt7921+usb+wifi |
| mt7925 usb wifi | https://www.aliexpress.com/wholesale?SearchText=mt7925+usb+wifi |
| mediatek usb wifi adapter | https://www.aliexpress.com/wholesale?SearchText=mediatek+usb+wifi+adapter |
| mt7612 usb wifi | https://www.aliexpress.com/wholesale?SearchText=mt7612+usb+wifi |

AliExpress shows a per-listing lifetime order count ("X sold") on each search-result card. This is the single best signal available for the cheap-OEM channel, which is invisible to linux-hardware.org probes (non-Linux end-users) and to Reddit/YouTube (no brand to mention).

## Tooling notes

- Headless Chromium with default settings auto-resolves the eBay "Pardon Our Interruption" splash challenge after ~3 seconds. AliExpress has no challenge for wholesale-search pages.
- All eBay extraction selectors target the current `s-card` class structure. The legacy `s-item__title` / `s-item__price` selectors return zero matches on 2025+ eBay.
- AliExpress uses opaque hashed React class names that change per deploy. Extraction goes through `document.body.innerText` and regex-mines the {title, price, "X sold"} triplet textually.

## Credibility tier

- **eBay: B.** Sold-listing counts are real transactions, visible for ~90 days after sale. Not a perfect proxy for current retail velocity but the best free retail signal available.
- **AliExpress: B-.** Lifetime order counts aggregate over the full listing history (months to years). High volumes are meaningful; absolute comparisons between listings with different ages are not.
