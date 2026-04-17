# FCC ID lookups and manufacturer catalogues - source credibility

Compiled by: the manufacturer data
Date: 2026-04-16

## FCC ID database (fccid.io, fcc.report, FCC.gov OET)

The Federal Communications Commission's Office of Engineering and Technology (OET) authorisation database is the canonical record of every wireless device legally sold in the United States. Every device that intentionally radiates RF in licensed or unlicensed bands must be tested and certified before import or sale; the public docket exposes the test report, internal photos (typically with chipset markings legible), external photos, schematics (often confidential), user manuals, and the grant letter.

**Authoritative within scope, with caveats:**

- FCC ID = US-only certification. Devices may exist in EU/Asia without an FCC ID. Do not interpret "no FCC ID" as "non-existent product."
- Internal photos showing a chipset marking are the strongest possible chipset evidence -- it is a US-government-archived photo of the production silkscreen. This trumps any marketing claim.
- The FCC ID is two parts: a 3-5 character "grantee code" (assigned to the company; e.g., 2AB87 = Iconnect / Alfa, PY3 = Netgear, TE7 = TP-Link) plus a per-product code chosen by the applicant.
- Module designations: chipset vendors like MediaTek file FCC IDs on their reference modules (e.g. RAS-MT7921 for MT7921 reference). Brand-name adapters can either re-use the module ID or file their own.

**Free aggregators (mirror FCC.gov, easier UI):**

- `https://fccid.io` - widely used, free, has thumbnails of internal photos. Some IDs missing or fuzzy-matched. Occasional 404s on URLs that were not yet indexed.
- `https://fcc.report` - alternative aggregator with similar coverage; useful if fccid.io misses an ID.
- `https://www.fcc.gov/oet/ea/fccid` - the original. Slow but authoritative.

When this report cites an FCC ID, the chain is:
1. FCC ID number (canonical)
2. Internal photo URL (where chipset is visible)
3. Cross-reference against Linux mt76 driver VID:PID table (independent confirmation)

A device counts as "FCC-confirmed MediaTek" only if both (a) the FCC internal photo shows an MT-prefix chip OR (b) the wikidevi/techinfodepot mirror of the FCC filing names the chipset, AND (c) the Linux mt76 driver claims its VID:PID. Anything missing one leg drops to "marketing-claim" or "driver-only" tier.

## Manufacturer catalogues

Coverage and reliability vary by brand:

- **Alfa Network** (alfa.com.tw, alfanetwork.al) - small Taiwan vendor, dominant in pen-test community. Spec pages name chipset directly. High signal-to-noise. EU site (.al = Albania domain registered to Alfa Network EU distributor) is sometimes down (404s observed during this run).
- **Netgear** (netgear.com) - large catalogue, current models only. Chipset NEVER named on spec pages. Must cross-reference FCC and driver tables.
- **TP-Link** (tp-link.com/us) - very broad catalogue, current models well organised. Chipset NEVER named on spec pages. FCC IDs starting with `2AXJ4` and `TE7` map to TP-Link.
- **Asus** (asus.com) - vast SKU sprawl. The same model letter (e.g. USB-AC55) can be either a MediaTek MT7612U OR a Realtek part depending on hardware revision. Chipset MUST be confirmed per revision.
- **BrosTrend** (brostrend.com) - relatively transparent. Their Linux-targeted "L" suffix line is publicly cataloged on linux.brostrend.com with kernel-driver requirements. Does not always state chipset on the product page but does on the support docs.
- **EDUP** (szedup.com, edupshop.com) - Chinese OEM, sells under own brand and white-labels. Chipset usually mentioned in Amazon listing copy ("MT7921AU"). FCC IDs sparse (some EDUP products are sold without US FCC certification).
- **Cudy** (cudy.com) - mid-tier. Spec pages do not name chipset. Best evidenced via download centre - the driver INF/INI inside the package reveals the underlying chip.
- **Comfast** (comfast.com) - bargain-tier; *known* to silently swap chipsets between hardware revisions of the same model number (e.g. CF-952AX V2 swapped from MT7921 to Realtek). Treat all Comfast claims as revision-specific.
- **Tenda** (tenda.com / tendacn.com) - product page often 404s; relies on third-party catalogues. Chipset lottery similar to Comfast.
- **Mercusys** (mercusys.com) - TP-Link's budget sub-brand. Predominantly Realtek-based USB adapters; MediaTek presence appears low.
- **Rosewill / WAVLINK / D-Link / Linksys / AVM** - legacy or low-volume brands. Where MediaTek presence exists it is mostly mt76x0 / mt76x2 era devices that are end-of-life.

## Driver source as third leg of evidence

The Linux kernel `drivers/net/wireless/mediatek/mt76/` source tree contains explicit `USB_DEVICE(VID, PID)` entries. Every entry is a device that the upstream maintainers have personally accepted a patch for, which means at minimum someone provided the maintainer with a real device with that VID:PID and proved it speaks MT76 protocol. This is **stronger** evidence than any marketing claim because:

1. The device must have actually been tested by the patch author (or someone validating).
2. The maintainer (Felix Fietkau, Lorenzo Bianconi for mt7921, Sean Wang for mt7925) gates the merge.
3. Any wrong VID:PID would result in driver attaching to the wrong device and bricking it -- there is a strong negative selection.

Sources used:
- `drivers/net/wireless/mediatek/mt76/mt7921/usb.c` - MT7921U / MT7902U
- `drivers/net/wireless/mediatek/mt76/mt7925/usb.c` - MT7925U
- `drivers/net/wireless/mediatek/mt76/mt76x2/usb.c` - MT7612U / MT7662U
- `drivers/net/wireless/mediatek/mt76/mt76x0/usb.c` - MT7610U / MT7630U / MT7650U
- (mt7601u is in `drivers/net/wireless/mediatek/mt7601u/` outside the mt76 tree)

## Community catalogue: morrownr/USB-WiFi

The repo `https://github.com/morrownr/USB-WiFi`, maintained by Nick Morrow himself (the same maintainer who asked the question this dataset answers), is the de facto community catalogue of which adapters work on Linux. Its `home/USB_WiFi_Adapters_that_are_supported_with_Linux_in-kernel_drivers.md` file lists every adapter with brand, model, chipset, USB version, antenna config, and minimum kernel version. This is the highest-quality crowdsourced list available, but it is biased toward Linux-compatible models -- a Realtek 8821CU adapter that requires out-of-tree drivers may be popular on Windows but absent here.

For the purposes of this dataset, morrownr/USB-WiFi is treated as **driver-confirmed** evidence (third leg) but not as the primary source -- where possible, every entry is cross-referenced against either an FCC filing or a manufacturer page.
