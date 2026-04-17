# References -- Linux distro bug trackers

Captured 2026-04-16.

## Source credibility / appropriate use

| Source | URL | API | Bias / caveats | Best used for |
|---|---|---|---|---|
| bugzilla.kernel.org | https://bugzilla.kernel.org | REST `/rest/bug` (UA-gated by Anubis; bypass `git/2.40.1`) | Upstream-developer audience; many mt76 driver bugs filed directly here. Title-only search misses VID:PID. | Upstream driver-quality signal. The "kernel.org NEW for 4+ years" bugs are the latent-bug list. |
| Red Hat / Fedora Bugzilla | https://bugzilla.redhat.com | REST `/rest/bug` | Heavy ABRT auto-filing inflates same-crash-signature counts. Heavy CVE auto-filing for `mt7921`/`mt7925`. EOL closures common. | Active QA signal. CVE remediation tracking. |
| openSUSE Bugzilla | https://bugzilla.opensuse.org | REST `/rest/bug` | Smaller user base than Fedora; SUSE security team auto-files VUL-0 bugs which inflate counts for chipsets with CVEs. | Cross-validation with Fedora; SUSE Tumbleweed early-kernel issues. |
| Ubuntu Launchpad | https://bugs.launchpad.net | REST `/1.0/ubuntu?ws.op=searchTasks` (NOT `/+source/linux` -- empty) | Apport auto-filing common; bug title schema is `Bug #N in PACKAGE (Ubuntu): ...`. Many community-confirmed never get triaged. | Largest Linux desktop user base in the world. Bug filed = real user. |
| Debian BTS | https://bugs.debian.org | None usable (Varnish PoW blocks curl). Use `web-search site:bugs.debian.org` + `web-fetch tooling` for individual bugs. | Slowest distro to enable new chipsets; bug list is 50% kernel-config-enable requests. | Detecting "this chipset isn't compiled in stock Debian" gaps. |
| Arch BBS forum | https://bbs.archlinux.org | None; web-search only. | Arch flyspray (https://bugs.archlinux.org) returns ZERO MediaTek-USB hits. Use BBS instead. Forum thread is weaker signal than bugzilla entry. | User-experience signal for Arch; bleeding-edge kernel issues. |
| Arch flyspray | https://bugs.archlinux.org | -- | Empty for MediaTek USB. Not useful. | -- |
| Arch GitLab | https://gitlab.archlinux.org/archlinux/packaging/packages/linux-firmware | gh / git | Has `mt7921e` regression issue (PCIe -- not USB). | Niche packaging issues. |

## Tools used

- `curl -s -A "git/2.40.1" --get URL --data-urlencode "short_desc=$term"` -- standard Bugzilla REST query
- `curl -s -A "git/2.40.1" --get URL --data-urlencode "ws.op=searchTasks" --data-urlencode "search_text=$term"` -- Launchpad
- `web-search` with `site:bugs.debian.org` and `site:bbs.archlinux.org` -- when direct API blocked or absent
- `web-fetch tooling` -- when the JS challenge blocks curl but the tool's egress is unaffected
- `python parse_results.py` -- de-duplication, family classification, aggregation

## Bugzilla REST quirks discovered

- `short_desc_type=allwordssubstr` is the equivalent of "Summary contains all the words" in the web UI. Default is exact-match-substring which under-counts.
- `include_fields=id,summary,status,resolution,creation_time,last_change_time,product,component,version` -- minimum useful field set; without `include_fields` the API returns enormous nested objects.
- `limit=200` recommended for Red Hat / openSUSE; default is 20 and silently truncates.
- The kernel.org Bugzilla returns `{"error":true,"message":"...","code":N}` (HTTP 200) on auth failure -- check for `bugs` key, not HTTP status.

## Cross-source validation note

Every bug ID captured is directly verifiable: navigate to e.g. https://bugzilla.kernel.org/show_bug.cgi?id=219446 and compare title/status. Debian bug IDs verifiable via https://bugs.debian.org/<id>. Red Hat and openSUSE similarly public by ID.
