# Reference: what YouTube + deep Reddit recommendations represent (and don't)

YouTube + Reddit deep-comment-tree data source. All data captured 2026-04-16.

## What this signal *does* represent

- **Pre-purchase video research surface (YouTube).** "Best USB WiFi adapter 2025" videos are the dominant pre-purchase research format for many consumers, including Linux/Kali buyers. A 326K-view David Bombal video about an Alfa adapter shapes more buyer intent than dozens of Reddit comments. YouTube reach is approximated via channel-tier inference (David Bombal 1.7M+ subs, Chris Titus Tech 500k+, vs no-name affiliate channels) plus per-video view counts where Brave Search exposed them inline.
- **Reddit deep-tree coverage.** Recommendations that live in *replies-to-replies* on long comment chains -- the kind that get truncated when only the top-of-tree is fetched. Top-of-tree surface is covered in `reddit-forums.md`; this page covers the long tail.
- **New-subreddit coverage.** r/AskNetsec, r/cybersecurity, r/HowToHack, r/pentesting, r/oscp, r/sysadmin, r/Fedora, r/archlinux, r/linux4noobs, r/linuxquestions etc -- adapter-buying-advice subs that the `reddit-forums.md` query set under-sampled.
- **Vendor-marketing signal.** Vendor channels (BrosTrend, LB-LINK, Rokland-as-Alfa-reseller) explicitly publishing "works on Linux" videos for their MediaTek SKUs is a real signal that vendors view Linux as a marketable target audience. Tagged separately from independent-reviewer endorsements.
- **Time progression.** YouTube videos from 2018 to 2026 in the same corpus trace how Alfa AWUS036NHA (~2018) -> AWUS036ACH (~2020-2022) -> AWUS036ACM (~2022-2024) -> A8000 / AWUS036AXML (~2024-2026) -> A9000 / BrosTrend AXE5400 (~2026) is the visible buyer-intent migration.

## What this signal does *not* represent

- **Total install base.** Reddit/YouTube discourse over-weights pen-test sticks and under-weights mass-market generics. linux-hardware.org probe data is the rebuttal.
- **YouTube view-count rigour.** Only ~27% of catalogued videos have view counts available. View-count rankings here are *directional*, not authoritative. View counts also age-bias toward older videos.
- **MediaTek's true USB market share.** Cudy, Mercusys, Comfast, EDUP, BrosTrend etc -- many have near-zero YouTube AND near-zero Reddit recommendation footprint despite shipping volume. Aspirational signal under-counts these by 1-2 orders of magnitude.
- **Sponsored / affiliate content.** Many "Top 5 / Top 7 best USB WiFi adapters" videos are affiliate-revenue formats with little actual hands-on Linux testing. Their adapter selections track Amazon search rankings more than Linux compatibility.
- **Localised buyer markets.** Brazilian Portuguese, Hindi, Bangla, Vietnamese Linux-WiFi YT channels exist but Brave's English-default search did not surface them. r/Pentesting (capital P) and r/wardriving returned essentially empty content. English-anglophone bias is structural to the data sources used.
- **MT7925 USB vs MT7925 internal-PCIe sentiment.** r/linux's MT7925 negative-sentiment thread (28 mentions) is overwhelmingly about *internal PCIe cards* on Linux 6.19-rc1, not USB sticks. The chipset reputation drags but the actual USB-side experience is different. Reddit-level mention counts conflate them.

## Known biases

1. **YouTube view-count age bias.** A 2019 video has had 6 years to accumulate views. A 2026 video has had weeks. The view-count leaderboard is a function of date as much as of intrinsic quality. Recent mt76-favouring content is under-represented in the view-weighted rank.
2. **Channel-tier estimation.** Done qualitatively (David Bombal very large, vendor channels small). No socialblade / YouTube Data API enrichment attempted.
3. **Bombal effect.** Single largest video in corpus is about the Alfa AWUS036ACH (Realtek). One charismatic mainstream YouTuber endorsing a non-mt76 adapter shapes the buyer-intent funnel more than 20 vendor-channel mt76 videos combined. The mt76 community lacks an equivalent celebrity endorsement at this scale.
4. **Search rate-limits.** Brave Search 429 and DuckDuckGo CAPTCHA cut the planned 11-query set short. The 8 queries that completed are biased toward English / Linux-focused / Kali-focused terms; less coverage of OpenWrt-specific or home-router-specific YT discourse.
5. **noembed.com oEmbed limit.** Provides title and channel name but no view counts and no upload date. View counts here are only those Brave snippets exposed inline.
6. **Sub case-sensitivity gotcha.** Reddit treats `r/Pentesting` and `r/pentesting` as different subs; lowercase has the active community.
7. **Filter cliff.** New-sub search results were filtered to threads matching adapter-relevant keywords (`alfa|awus|mt76|mt7921|mt7925|mt7612|a8000|mediatek|wifi|adapter`). Threads using only chipset numbers (e.g. "MT7902") would be missed -- and one such thread did surface in YouTube enrichment as a "GitHub Wi-Fi driver MT7902" video.

## How this signal fits alongside other sources

- **Combined with Reddit recommendation counts** -- deep-tree + new-sub coverage adds ~30% mention volume across all adapters with similar directional ranking. Cross-source agreement is strong.
- **Combined with linux-hardware.org install base** -- brand-perception gap: BrosTrend AXE5400 has YT vendor signal but zero install base yet (just released); Mercusys MA30H has product reality but zero YT or Reddit footprint -- a dormant SKU.
- **Combined with mt76 git activity / firmware availability** -- vendor videos (BrosTrend AXE5400 Feb 2026) signal that firmware delivery for MT7925AU USB needs to be production-ready by ~Q2 2026. Aspirational signal pre-dates buyer reality by 6-12 months; vendor YT leading-indicators are useful for firmware planning.
- **The bare-chipset mention rows** (`chipset:MT7921`, `chipset:MT7925`, `chipset:MT7612U`, `chipset:MT76 generic`) are the proxy for "MediaTek mindshare not anchored to a SKU." MT7921 + MT7612U dominate positive mindshare; MT7925 dominates negative-but-large mindshare (driven by internal-card complaints, not USB).

## Source URLs (canonical)

- YouTube watch URLs: `https://www.youtube.com/watch?v=<video_id>`
- noembed.com oEmbed proxy: `https://noembed.com/embed?url=<urlencoded YouTube URL>` -- returns title + author_name
- Brave Search: `https://search.brave.com/search?q=<query>+site%3Ayoutube.com&source=web`
- DuckDuckGo HTML: `https://html.duckduckgo.com/html/?q=<query>`
- Reddit JSON API: `https://www.reddit.com/r/<sub>/search.json` and `https://www.reddit.com/comments/<id>.json?limit=500&depth=20`

## Capture scope

37 unique YouTube videos enriched with channel + adapter + mt76 status; 27 Reddit search-listing captures across 17 new subreddits; 41 + 11 Reddit comment-tree captures. Cited mention counts come from a regex-based adapter sweep across the capture set.
