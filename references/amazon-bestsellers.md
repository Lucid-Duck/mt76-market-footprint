# Source reference: Amazon US Best Sellers + product search

## What it is

Amazon's "Best Sellers" lists are publisher-curated rankings of products in a given category, refreshed hourly. The "Best USB Computer Network Adapters" category at https://www.amazon.com/Best-Sellers-Computers-Accessories-Network-Adapters/zgbs/pc/13983791 is a mixed category covering both USB-WiFi adapters and USB-Ethernet adapters.

Search results at https://www.amazon.com/s?k=<query> return products matching the query, ranked by Amazon's relevance algorithm (which factors in sales velocity, review count, and ad placement).

## Credibility tier: B

Amazon Best Sellers ranks reflect actual sales volume on the world's largest online retailer, but with several caveats that prevent A-tier classification:

- **Unit sales are not published.** We see ranks and review counts, not "X units sold last month."
- **Review count is a coarse proxy for sales** with a long-tail bias: a product on sale for 5 years has more reviews than a 6-month-old product with the same monthly sales rate.
- **Sponsored placements** rank organically and as ads. We capture the sponsored flag but mixed signal remains.
- **Geo-routing** alters the listing set and pricing. The capture session was geo-located to Canada (CAD pricing) and may not reflect the exact US baseline.
- **Category boundaries are publisher-defined.** "USB Network Adapters" mixes WiFi and Ethernet, which dilutes the signal we want.
- **Amazon search ranking is opaque.** Two queries for the same product can return different rankings depending on session, location, and ad inventory.

## Why include this source despite tier-B credibility

Amazon's review-count signal is one of the only retail-side data points available for free without credentialed sales data. It complements the Linux-community-side signals (linux-hardware.org install base, GitHub mention volume, Reddit recommendations) by capturing the broader consumer market that Linux-specific signals miss.

In particular, Amazon catches:

- **Branded retail demand** -- the products people walk into Best Buy or click through Amazon to buy
- **Long-tail legacy products** -- adapters that haven't been actively marketed in years but still sell steadily
- **Sponsored / advertised placement** -- which manufacturers are spending money to put their products in front of buyers

It misses:

- **No-name OEM SKUs** -- generic dongles distributed in bulk to computer shops
- **B2B and developer / enterprise procurement** -- not generally on Amazon
- **AliExpress, Newegg, BestBuy, B&H** -- covered by separate source pages (see `ebay-aliexpress.md`).

## Methodology used

- Real Chromium browser via headless browser, accessibility tree extraction
- 4 page loads: 2 Best Sellers pages + 2 targeted searches
- Per-item structured capture: ASIN, title, rating, review count, price, sponsored flag
- All access timestamps recorded; data is a 2026-04-16 evening snapshot

## Anti-bot considerations

Amazon has aggressive anti-bot measures (Cloudflare-class detection, Akamai Bot Manager, behavioural fingerprinting). The capture session succeeded with 4 navigations at human-paced speed in a single browser session; results may differ on a future capture if rate-limiting tightens or session fingerprinting flags the request.

## Citation format

Every claim cites the source URL and access timestamp. Specific items are referenced by ASIN (Amazon Standard Identification Number, a stable per-product ID) so a reader can re-verify the same listing even if the URL slug changes.
