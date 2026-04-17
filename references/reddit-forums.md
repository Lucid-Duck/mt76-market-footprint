# Reference: what Reddit + forum recommendations represent (and don't)

Reddit / Linux-community-forum data source page for the mt76-market-footprint dataset. All data captured 2026-04-16.

## What this signal *does* represent

- **Aspirational buyer intent within a Linux/pen-test community.** Someone asking "what USB WiFi adapter should I buy for Kali / monitor mode / OpenWrt?" and the upvoted answers they receive. This is a *forward-looking* signal -- it reflects what Linux users *would* buy if buying today. linux-hardware.org install-base data by contrast reflects what's already in the field, which lags by years.
- **Brand and SKU-level mindshare** in a community that disproportionately shapes search results, blog posts, YouTube buying-guide videos, and Linux subreddit advice. Reddit recommendation winners tend to become Amazon best-sellers in the niche.
- **Driver-experience sentiment** for MediaTek silicon in particular -- comment threads expose how the kernel-stable MT7921/MT7612U sticks are received vs the bleeding-edge MT7925 internal NICs.
- **Aged signal mix** -- threads from 2023-2026 in the same corpus trace the evolution from "Alfa AWUS036ACM is the answer" (2023) toward "Netgear A8000 / AWUS036AXML for WiFi 6" (2025-2026).

## What this signal does *not* represent

- **Total install base.** A Realtek RTL8188EUS no-name dongle on millions of Raspberry Pis will get vastly fewer recommendations than an Alfa AWUS036ACM that ships a few thousand units a year, because nobody recommends a Pi-bundled freebie. linux-hardware.org install-count data is the rebuttal.
- **General-consumer USB WiFi market.** Almost everything in this signal is filtered through the "I want monitor mode / I run Linux / I do penetration testing / I run OpenWrt on a Pi" lens. The mass-market USB WiFi stick category (TP-Link Archer T2U Plus on Best Buy shelves, generic dongles in cheap laptops) is under-represented.
- **MediaTek's true USB share.** MediaTek silicon is in many cheap-brand SKUs (BrosTrend, Comfast, Cudy, Mercusys, EDUP) that have zero Reddit recommendation traffic. The Reddit-recommendation count for "MediaTek USB" massively understates how much MediaTek-chip USB hardware is actually sold and shipped.
- **Sentiment magnitude.** A 312-upvote angry thread about MT7925 is one mention in the count; the same chip getting "works fine" in 100 short comments is 100 mentions. Mention count is structural, not weighted.
- **Reliability signal.** Reddit recommendations skew toward what worked once for the recommender, not what is statistically reliable across kernel versions. Long-term firmware/driver health is better captured by mt76 git activity.

## Known biases

1. **Heavy Alfa bias.** Alfa Network has dominated the WiFi-pentest brand association for ~15 years. AWUS036ACM specifically has been the "buy this" answer since the MT7612U mt76 driver matured around 2018-2019.
2. **Pen-test use-case dominance.** "Monitor mode" and "packet injection" are recurring filter criteria. This privileges Alfa external-antenna sticks over compact desktop adapters. A "best USB WiFi for general home use on Linux" survey would invert several rankings.
3. **AWUS036ACM vs AWUS036ACH name confusion.** These two SKUs differ by one letter (M vs H) but have entirely different chipsets (MT7612U vs RTL8812AU) and entirely different Linux driver paths (mt76 vs out-of-tree rtw88/8812au-aircrack-ng). Many Reddit users name one and mean the other. Mention counts use the literal string and do not correct for the confusion. Treat the AWUS036ACM count as "around 23, likely a few of which mean ACH" and vice versa.
4. **Survivor bias.** Threads that died at zero replies don't surface in search results; threads where someone got an answer they liked do. This privileges entrenched recommendations over emerging ones.
5. **Recency thumb on the scale.** WiFi 6E + 7 hype in late 2025 / early 2026 is pulling MT7925 mentions up despite shaky driver state. The chipset-level MT7925 count of 17 should not be read as "MT7925 USB sticks are widely recommended" -- it's mostly internal-NIC discussion and complaints.
6. **Geographic skew.** Reddit is US/UK/AU/EU-heavy English-speaking. Asian-market USB adapter SKUs (Cudy, Mercusys, Comfast, BrosTrend) are vastly under-recommended on Reddit relative to actual sales volume in those regions.
7. **Wifi-pentest niche dominates mt76 mentions.** Most recommendations of mt76-class USB sticks come from people building Kali/OpenWrt/airgeddon setups. The mt76 driver's importance to mainstream Linux desktop WiFi (where it underpins many AMD-platform internal NICs via MT7921K) is acknowledged in passing but doesn't drive USB-stick recommendations because internal NICs aren't USB sticks.

## How to use this dataset alongside the other sources

- Cross-reference recommendation count (Reddit) with install-base count (linux-hardware.org) to find **brand-perception gap**: SKUs with low installs but high recs are aspirational/fast-growing; SKUs with high installs but low recs are commodity/legacy.
- Cross-reference recommendation count (Reddit) with mt76 driver discussion volume (`morrownr/USB-WiFi` issues + lore.kernel.org) to find **community attention concentration**: is the discussion about a specific SKU or about the chipset family?
- The *bare-chipset* mention rows (chipset:MT7921, chipset:MT76 generic, etc.) are the closest proxy to "MediaTek mindshare not anchored to a specific SKU" -- useful for the overall "is mt76 winning Linux mindshare?" question.

## Source URLs (canonical)

- Reddit JSON API base: `https://www.reddit.com/r/<sub>/search.json` and `https://www.reddit.com/comments/<id>.json`
- OpenWrt forum search: `https://forum.openwrt.org/search.json?q=<query>`
- ArchWiki USB-WiFi page (background only, not mined for recommendations): `https://wiki.archlinux.org/title/Network_configuration/Wireless`

## Capture scope

60 raw API captures (Reddit search listings + 16 comment trees + 5 OpenWrt forum dumps) covering 17 subreddits plus OpenWrt forum threads. Cited mention counts come from a regex-based adapter sweep across the capture set.
