# References: morrownr/USB-WiFi and lore.kernel.org/linux-wireless

This page documents the two main GitHub + kernel-mailing-list sources mined by the GitHub data and explains what they represent for the MediaTek USB WiFi market-footprint question.

## morrownr/USB-WiFi

- Repo: https://github.com/morrownr/USB-WiFi (accessed 2026-04-16)
- Maintainer: Nick Morrow (`@morrownr`); also maintainer of `morrownr/mt76` (the fork this dataset feeds into).
- Repo metadata snapshot (via `gh api repos/morrownr/USB-WiFi`, 2026-04-16):
  - Stars: 4,210
  - Watchers: 4,210
  - Forks: 246
  - Open issues: 365 (closed issues additionally counted in search results)
  - Topics include `mediatek`, `usb-wifi-adapters`, `linux-kernel-driver`, `kali-linux`
  - Created 2020-12-30, last push 2026-03-31, repo updated 2026-04-17
  - `has_discussions: true`, `has_wiki: true`
- Description: "USB WiFi Adapter Information for Linux"
- Self-reported view counts (from repo `README.md`): 45,917 sitewide views over the two-week period ending 2023-12-21.

### Why this repo is the canonical Linux USB WiFi community hub

- It is the most-starred Linux USB WiFi repository on GitHub by a wide margin (4,210 stars; the next-tier RTL8812AU driver fork repos sit in the low thousands), and Nick Morrow is widely cited in distro forums, Reddit posts (r/linux, r/linuxquestions, r/Kalilinux) and Hak5 community threads as the primary curator of "what works."
- It hosts curated lists used by hundreds of downstream guides:
  - `home/USB_WiFi_Adapters_that_are_supported_with_Linux_in-kernel_drivers.md` ("The Plug and Play List", 1,179 lines as of 2026-04-16) -- product-by-product reviews with photos, VID:PIDs and merchant links.
  - `home/The_Short_List.md` -- the 16-adapter "superstar" shortlist, currently dominated by MediaTek (mt7921au, mt7612u, mt7610u, mt7601u) plus Atheros AR9271 and Ralink RT5370/RT5572.
  - `home/Recommended_Adapters_for_Kali_Linux.md` -- the de-facto Kali pen-testing list; 9 of 13 modern recommendations are MediaTek-based.
  - `home/Speed_Comparison_Test.md` -- single-test-rig iperf3 throughput per adapter.
  - `home/USB_WiFi_Chipsets.md` -- chipset/kernel/feature support matrix.
- Issues function as both a help forum and a parts-availability ticker (e.g. "[News] First USB WiFi 7 Adapter that uses the mt7925 chip is now available (Netgear A9000)" -- issue #630, 71 comments).
- Pull requests and issues from external users frequently add iw_list dumps for newly tested adapters (`home/iw_list/`).

### Selection bias to be aware of when using this corpus

- Skews toward enthusiast / pen-test / monitor-mode users (Kali, Hak5 audience). Casual users are under-represented.
- Skews toward 2.4 / 5 / 6 GHz USB sticks aimed at desktops, laptops, Raspberry Pi and OpenWrt; embedded Wi-Fi modules and PCIe devices are mostly out of scope.
- A handful of issue threads are "News" / "List of bug reports" mega-threads pinned by the maintainer (e.g. issue #87, #107) that accrue 100+ comments and dominate raw mention counts -- these inflate any single-adapter mention count where the chipset name appears.

## morrownr/mt76 (the fork being fed)

- Repo: https://github.com/morrownr/mt76 (accessed 2026-04-16)
- Issue tracker is small; the issue we are answering is `morrownr/mt76#5` (Nick's question to Devin re: which device firmware to ship).
- Cross-reference search (e.g. `repo:morrownr/mt76 mt7921`) returns very few hits because conversation happens upstream (`morrownr/USB-WiFi`) or in linux-wireless.

## morrownr/rtw89

- Repo: https://github.com/morrownr/rtw89 (accessed 2026-04-16)
- Searched only as a sanity cross-reference. Returns mostly Realtek WiFi 7 discussion. A handful of comparison threads mention MediaTek adapters as the preferred alternative; relevant counts are folded into the master table below.

## lore.kernel.org/linux-wireless

- Archive: https://lore.kernel.org/linux-wireless/ (accessed 2026-04-16)
- Anubis-protected for browser user-agents; the capture method uses `User-Agent: git/2.40.1` and the per-query Atom feed (`?q=<term>&x=A`) to bulk-fetch search results without triggering the bot challenge.
- Hosts patches, reviews and bug reports for the in-kernel `mt76` driver family (MT7601U, MT7610U, MT7612U, MT7662U, MT7921U, MT7925U) and the `mt7925e` PCIe sibling.
- Useful as a maintainer / kernel-developer cross-reference: which adapters and VID:PIDs come up by name in driver patches and on-list user reports.
- Patchwork API at https://patchwork.kernel.org/api/ accepts the same `git/2.40.1` UA if more structured queries are needed.

## What this gives us for the dataset

For any adapter on Nick's curated lists we now have:

- Whether it appears in `The_Short_List.md` (high-confidence community favorite).
- Whether it appears in `Recommended_Adapters_for_Kali_Linux.md` (high-confidence pen-test favorite).
- Whether it appears in `Speed_Comparison_Test.md` (Nick has personally throughput-tested it).
- A count of how many distinct GitHub issues / PRs in `morrownr/USB-WiFi` reference it by brand-model or chipset name.
- A count of how many threads in `linux-wireless` reference it by brand-model or chipset.
- Representative thread URLs for citation in the master table.

These counts are noisy proxies for "Linux user adoption" -- not direct sales data -- but they are the most defensible community-mention signal we have for Linux specifically. Combined with the linux-hardware data's linux-hardware.org probe data, the manufacturer data's FCC/manufacturer catalogue, and the Reddit data's Reddit / distro-forum recs, they let us triangulate a defensible per-adapter footprint estimate for the firmware-shipping decision in `morrownr/mt76#5`.
