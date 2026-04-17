# Methodology

## Research question

**What MediaTek WiFi adapters and modules are most commonly used by Linux users, across USB, PCIe/M.2, and SDIO form factors?**

This question has no single authoritative answer. It is triangulated from multiple imperfect signals. This document records every signal collected, how it was collected, and how it was weighted.

## Provenance standards

Every data point in this repo is traceable to a primary source. Specifically:

1. **Every numeric claim cites the URL it came from**, in the form `[source-name](url) accessed YYYY-MM-DD HH:MM TZ`.
2. **Every API response was captured at point of access**. This protects against site changes, deletions, or future disputes.
3. **Every credibility assessment is stated**, not assumed. A linux-hardware.org probe count is high-confidence. A single Reddit comment is anecdotal.
4. **No invented data.** If a field can't be filled from a primary source, it's left blank with a `[unknown]` marker.
5. **Time-stamping**: research is dated. Every artifact carries the date it was collected.

## Source list and weighting

| Source | Weight | Credibility tier | Justification |
|--------|--------|------------------|---------------|
| Linux kernel source (`drivers/net/wireless/mediatek/mt76/`) | n/a (ground truth) | A+ | Authoritative VID:PID -> driver -> chip mapping and MODULE_FIRMWARE declarations |
| upstream linux-firmware (kernel.org + gitlab) | n/a (ground truth) | A+ | Authoritative firmware blob inventory |
| linux-hardware.org probes | 0.30 | A | Real install data from voluntarily-submitted probes. Best proxy available for actual Linux install base |
| FCC ID + manufacturer catalogue | 0.15 | A | Authoritative for "does this product exist and use this chip." Anchors the candidate set |
| Lenovo PSREF + HP QuickSpecs + Dell spec pages + OEM driver catalogues | 0.15 | A | Laptop and desktop shipping configurations, chip-level identification |
| Framework Marketplace + System76 / Tuxedo / Slimbook / Ubuntu Certified Hardware | 0.10 | A | Linux-first vendor signal |
| morrownr/USB-WiFi GitHub mentions | 0.10 | A | Curated by the de facto Linux USB WiFi community lead |
| Distro bug trackers + MITRE / NVD CVE databases | 0.08 | B | Confirmed-device-owner signal; pain-point signal; regression tracking |
| Stack Exchange Q&A view counts | 0.05 | B | Help-seeking volume proxy |
| eBay sold-listing volume | 0.04 | B | Retail transaction proxy with a 90-day window |
| Reddit / forum recommendations | 0.02 | B | Aspirational signal |
| AliExpress order counts | 0.01 | B | Lifetime per-listing order count. Reveals cheap-OEM volume invisible to brand-anchored signals |
| Amazon review counts | 0.01 | C | Coarse proxy for retail sales velocity |
| YouTube reviews | n/a (context only) | C | Aspirational signal at the video-discovery level |
| iFixit teardowns | n/a (confirmation) | A | Authoritative for gaming-handheld and niche-device chip identification |

## Scoring

Each adapter receives a score per source:

```
score_source = (source_value - source_min) / (source_max - source_min)
```

Normalised to [0, 1] within that source. Then:

```
total_score = sum(weight_i * score_source_i for each source)
```

Adapters with data from more sources earn higher confidence.

## What is NOT measured

- **Performance.** Whether an adapter is *good* is a different question.
- **Future popularity.** Today's data is today's data.
- **Non-Linux usage.** An adapter that sells well but never gets plugged into Linux is irrelevant to this dataset.
- **Performance across the PCIe negative-sentiment baseline.** MT7925 and MT7921-family PCIe modules shipped by laptop OEMs carry distro regressions. This shapes community sentiment but is not the same question as "is this chip popular." Popularity data is the count; the sentiment caveats are in `references/reddit-forums.md` and `references/distro-bugs.md`.

## Kernel-source anchoring

The master CSV is cross-checked against the Linux kernel mainline source for `drivers/net/wireless/mediatek/mt76/`. Kernel source is treated as the ground truth for VID:PID -> driver -> chip mapping and `MODULE_FIRMWARE` declarations. Every "confidence A" row is present in that source.

FCC IDs are cross-referenced against the live FCC database. Kernel anchoring is to torvalds/linux commit `d730905bc3c0075275b2d109cd971735274b98c0`; linux-firmware to gitlab SHA `3fc7117bb925983bc39d7ba957ce5fafe1f65d41`; morrownr/mt76 to `131771025d08b096444a8d8f2464e5b077c51edc`. All three PCIe-only blob pairs (MT7902, MT7922, MT7925) are byte-identical between morrownr/mt76/firmware/ and upstream linux-firmware as of 2026-04-17.

Key kernel-source facts:
- `mt76x0/mt76x0.h` defines `MT7610E_FIRMWARE = "mediatek/mt7610e.bin"` and `MT7610U_FIRMWARE = "mediatek/mt7610u.bin"` as distinct files. mt76x0 firmware is NOT shared between PCIe and USB.
- `mt76x2/pci.c` and `mt76x2/usb.c` both declare `MODULE_FIRMWARE(MT7662_FIRMWARE = "mt7662.bin")`. mt76x2 firmware IS shared.
- `mt7921e.ko` is a 4-chip driver binding MT7920, MT7921, MT7922, and MT7902.
- `mt7925e.ko` and `mt7925u.ko` both declare the same `mediatek/mt7925/WIFI_RAM_CODE_MT7925_1_1.bin` blob pair. mt7925 firmware IS shared.
- MT7902 PCI device ID `14c3:7902` is present in `mt7921/pci.c` on torvalds master; driver landed via Sean Wang's 11-patch series through Felix Fietkau's nbd tree.
