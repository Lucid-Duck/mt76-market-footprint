# References: OpenWrt + wireless.docs.kernel.org + Linux mt76 USB device tables

This page documents the OpenWrt and kernel.org-side sources mined by the OpenWrt data and explains what each represents for the MediaTek USB WiFi market-footprint question.

## OpenWrt project

### What it is

OpenWrt is the de-facto Linux distribution for consumer routers and network-attached devices, with very wide hardware coverage and a long-running Table of Hardware (ToH) database. Many OpenWrt devices have one or more USB ports, and the project also packages the upstream `mt76` driver as a set of opkg modules (`kmod-mt76*`).

### Sources fetched

- Table of Hardware JSON: https://openwrt.org/toh.json (accessed 2026-04-16, 3.94 MB, 2,977 device entries)
- ToH CSV (sample): https://openwrt.org/_export/csv/toh (accessed 2026-04-16)
- Forum megathread "USB Wi-Fi that work in OpenWrt. Please add to list" -- https://forum.openwrt.org/t/usb-wi-fi-that-work-in-openwrt-please-add-to-list/185059 (pages 1, 3, 5, 8 fetched 2026-04-16)
- Forum thread "USB WiFi Stick" -- https://forum.openwrt.org/t/usb-wifi-stick/133214
- Forum thread "[Solved] MediaTek MT7921AU USB is not reliable when used as an AP" -- https://forum.openwrt.org/t/solved-mediatek-mt7921au-usb-is-not-reliable-when-used-as-an-ap/241861
- Forum thread "MT7612U USB WiFi, which driver(s)" -- https://forum.openwrt.org/t/mt7612u-usb-wifi-which-driver-s/246802
- Forum thread "[Solved] USB W-Fi Issues" -- https://forum.openwrt.org/t/solved-usb-w-fi-issues/187893
- Package wiki: https://openwrt.org/packages/pkgdata/kmod-mt76 (release 22.03.0)

### What OpenWrt is and is NOT useful for here

- The ToH is overwhelmingly **integrated routers and APs** (1,366 WiFi Routers + 430 WiFi APs + 417 SBCs out of 2,977 entries). Of 1,471 entries with a MediaTek/Ralink WiFi chip string, **only 2 entries point at a USB-connected MediaTek WiFi part** -- the HAK5 WiFi Pineapple Mark 7 (2x MT7601, "other" device type) and the ALFA Network WiFi CampPro Nano Duo (a range extender that pairs ath9k with a USB MT7610U). The ToH therefore tells us almost nothing direct about the USB-stick market.
- What the ToH IS good for: confirming which MediaTek chip families are visible to the OpenWrt ecosystem at all. The top integrated MediaTek/Ralink WiFi parts are MT7620A (117 devices), MT7628AN (74), MT7612E (72), MT7612EN (66), MT7976CN (65), MT7603EN (48), MT7981BA (44), MT7603E (43), MT7620N (35), MT7610E (31). USB cousins of these (MT7610U, MT7612U) inherit the same `mt76` codebase.
- The forum is the actual signal source for USB-stick popularity in OpenWrt. The community-curated megathread #185059 is the canonical "what works" list for USB WiFi on OpenWrt.

### Selection bias to be aware of

- OpenWrt users are biased toward router/AP use cases. AP-mode reliability of USB sticks is over-weighted relative to client-mode use. The MT7921AU "unreliable as AP" thread (#241861) is a representative example.
- OpenWrt forum users are biased toward small-board-computer + USB-stick combos (Raspberry Pi, NanoPi R4S/R5S, x86 mini-PCs running OpenWrt-on-x86). Desktop/laptop users are mostly absent.
- The forum has multiple-page threads where the same adapter is mentioned across years. Counts are not comparable to the morrownr/USB-WiFi issue corpus.

## wireless.docs.kernel.org (formerly wireless.wiki.kernel.org)

### What it is

The official Linux Wireless project documentation, hosted under the kernel.org umbrella and now served via Sphinx + Read the Docs at https://wireless.docs.kernel.org/ . The legacy DokuWiki at https://wireless.wiki.kernel.org/ is permanently 301-redirected to this site (verified 2026-04-16). Sources are at https://github.com/linux-wireless/docs .

### Sources fetched

- Supported devices index: https://wireless.docs.kernel.org/en/latest/en/users/devices.html (47 KB, fetched 2026-04-16)
- USB device list: https://wireless.docs.kernel.org/en/latest/en/users/devices/usb.html (fetched 2026-04-16)
- mediatek driver page: https://wireless.docs.kernel.org/en/latest/en/users/drivers/mediatek.html (50 KB, fetched 2026-04-16)

### What this source IS and is NOT useful for

- The `users/devices/usb.html` page is **explicitly empty**: it states "Note: this list is still incomplete as not all drivers have information." There is no per-USB-adapter matrix on the kernel wireless wiki today.
- The `users/drivers/mediatek.html` page IS the canonical kernel-wiki summary of which mt76 sub-drivers handle which chip families and which kernel version added support. This anchors version-availability claims (e.g. "MT7921 USB is supported since 5.18+", "MT7925 PCIe/USB since 6.7+").
- For per-adapter VID:PIDs the canonical source is the `drivers/net/wireless/mediatek/mt76/<family>/usb.c` device tables in the upstream Linux kernel tree, fetched directly from raw.githubusercontent.com/torvalds/linux -- a curated, gated list and the most authoritative single answer to "is this VID:PID a real adapter."

### Per-driver USB device counts (Linux master, fetched 2026-04-16)

| Driver | Source file | USB device entries | Chip family |
|---|---|---:|---|
| `mt7601u` | `drivers/net/wireless/mediatek/mt7601u/usb.c` | 17 | MT7601U (1x1 b/g/n) |
| `mt76x0u` | `drivers/net/wireless/mediatek/mt76/mt76x0/usb.c` | 24 | MT7610U / MT7630U / MT7650U (1x1 a/b/g/n/ac) |
| `mt76x2u` | `drivers/net/wireless/mediatek/mt76/mt76x2/usb.c` | 15 | MT7612U / MT7602U / MT7662U (2x2 a/b/g/n/ac) |
| `mt7615` (USB stub) | `drivers/net/wireless/mediatek/mt76/mt7615/usb.c` | 2 | MT7663U (the second is 0x043e:0x310c -- LG-prefix VID, single OEM design) |
| `mt7921u` | `drivers/net/wireless/mediatek/mt76/mt7921/usb.c` | 5 | MT7921AU (2x2 ax) -- entries cover Alfa, Netgear A8000, Netgear A7500, plus generic/Comfast |
| `mt7925u` | `drivers/net/wireless/mediatek/mt76/mt7925/usb.c` | 2 | MT7925U (2x2 be) -- entries are 0x0e8d:0x7925 generic + Netgear A9000 |

The relative size of these tables is itself a market signal: MT7610U/MT7612U/MT7601U are the long-tail of cheap USB sticks (24+15+17 = 56 distinct VID:PIDs); MT7921AU/MT7925U have only 7 between them so far but each VID:PID corresponds to a high-volume modern WiFi 6/6E/7 adapter.

## Coordination with other waves

- **(manufacturer + FCC):** already cross-referenced the kernel mt76 USB device tables for ALFA, Netgear, Panda etc. We re-used those VID:PID anchor points and did not duplicate brand-by-brand catalog work.
- **(WikiDevi):** WikiDevi proper was the WikiDevi data's scope; we made one query against the deviwiki.com mirror (https://deviwiki.com/wiki/MediaTek) only to confirm per-chipset device counts (MT7601U: 35 documented adapters, MT7610U: 30, MT7612U: 12, MT7663U: 1) and did NOT enumerate adapter-by-adapter -- that's the WikiDevi data's deliverable.
- **(GitHub + lore):** the morrownr/USB-WiFi curated lists are the GitHub data's primary; we used them only as a sanity check on which OpenWrt forum recommendations match Nick Morrow's shortlist.

## Caveats

- OpenWrt ToH coverage of USB WiFi sticks is essentially nil. Any "popularity in OpenWrt" claim about a USB stick must come from forum posts, not the ToH.
- The kernel wireless wiki page on USB devices is a stub. Do not cite it as an authoritative compatibility matrix.
- The Linux mt76 driver tables list devices that have been **reported and accepted upstream**. They under-count real adapters (off-brand sticks running a known reference design with a private VID:PID may not appear) and slightly over-count "real" products (some entries are reference designs that never shipped at retail).
- All USB stick reliability data here is heavily AP-mode-biased because OpenWrt's typical use case is "router with extra radio." Client-mode reliability on a desktop Linux machine is a different question.
