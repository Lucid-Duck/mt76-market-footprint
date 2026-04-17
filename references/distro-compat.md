# References - distro-compat databases (the distro-compat data)

All accessed 2026-04-16 unless otherwise noted.

## Distro wikis (canonical pages)

- Debian WiFi wiki: https://wiki.debian.org/WiFi -- last modified 2025-05-15
- Debian Firmware wiki: https://wiki.debian.org/Firmware -- last modified 2025-08-16
- Gentoo WiFi wiki: https://wiki.gentoo.org/wiki/Wifi -- last edited 2026-03-05
- Ubuntu community wiki: https://help.ubuntu.com/community/WifiDocs/WirelessCardsSupported
- Ubuntu community wiki: https://help.ubuntu.com/community/WifiDocs

### Pages attempted but inaccessible

- Arch wiki: https://wiki.archlinux.org/title/Network_configuration/Wireless -- 403 Anubis
- Arch wiki: https://wiki.archlinux.org/title/Wireless_network_configuration -- 403 Anubis
- openSUSE: https://en.opensuse.org/HCL:Network -- 403
- openSUSE: https://en.opensuse.org/HCL:Wireless -- 403
- openSUSE: https://en.opensuse.org/HCL:Wireless_LAN -- 403
- openSUSE: https://en.opensuse.org/SDB:Network-Wireless -- 403
- Kernel wireless: https://wireless.docs.kernel.org/en/latest/en/users/drivers/mediatek.html -- 403

### Pages that don't exist (404)

- https://help.ubuntu.com/community/HardwareSupportComponentsWirelessNetworkCardsUSB
- https://wiki.debian.org/HardwareCompatibilityList
- https://wiki.debian.org/MediaTek , /Mediatek , /mt76 , /firmware-mediatek , /MediaTek_(MTK)
- https://wiki.debian.org/Firmware/Nonfree
- https://wiki.gentoo.org/wiki/MediaTek , /wiki/Wifi/Old_information
- https://openwrt.org/docs/guide-user/network/wifi/usb_wifi
- https://openwrt.org/docs/guide-user/network/wifi/usb-wifi
- https://openwrt.org/docs/techref/hardware/wifi-radios

## Ubuntu certified hardware

- https://ubuntu.com/certified/component/934 -- MT7921 PCIe (Dell certified machines)
- https://ubuntu.com/certified/component/920 -- MT7921 PCIe (Lenovo certified)
- https://ubuntu.com/certified?q=MediaTek -- search results page (mostly EVK boards + internal MT7925)

## OpenWrt package data

- https://openwrt.org/packages/pkgdata/kmod-mt7601u
- https://openwrt.org/packages/pkgdata/kmod-mt76x0u
- https://openwrt.org/packages/pkgdata/kmod-mt76x2u
- https://openwrt.org/packages/pkgdata/kmod-mt7921u
- https://openwrt.org/toh/start

## Distro forum / discussion threads

- Arch BBS: https://bbs.archlinux.org/viewtopic.php?id=284164 (Comfast CF-952AX, mt7921au, March 2023)
- Arch BBS: https://bbs.archlinux.org/viewtopic.php?id=224859 (MT7601U adapter)
- Arch BBS: https://bbs.archlinux.org/viewtopic.php?id=250273 (mt76x2u)
- Arch BBS: https://bbs.archlinux.org/viewtopic.php?id=215348 (Brostrend WNA016 mt7610u)
- Arch BBS: https://bbs.archlinux.org/viewtopic.php?id=272081 (MT7601U)
- Manjaro: https://forum.manjaro.org/t/wifi-driver-doesnt-work-mediatek-inc-mt7612u-802-11a-b-g-n-ac-wireless-adapter/125204 (ALFA AWUS036ACM, Oct 2022)
- openSUSE: https://forums.opensuse.org/t/ralink-mediatek-mt766u-mt7632u-and-mt7612u-chipsets-net-dyn-ac1200/127308 (NET-DYN AC1200, Aug 2017)
- Fedora: https://discussion.fedoraproject.org/t/cannot-get-usb-wifi-adapter-to-work-in-fedora-40/133771 (ALFA AWUS036AXM mt7921u, Oct 2024)
- Fedora tag: https://discussion.fedoraproject.org/tag/mediatek
- Linux Mint: https://forums.linuxmint.com/viewtopic.php?t=400012 (MediaTek MT7921; redirect issue, not directly fetched)
- OpenWrt: https://forum.openwrt.org/t/solved-usb-w-fi-issues/187893 (Feb 2024)
- OpenWrt: https://forum.openwrt.org/t/mt7612u-usb-adapter-not-working/134856 (Aug 2022)
- OpenWrt: https://forum.openwrt.org/t/solved-mediatek-mt7921au-usb-is-not-reliable-when-used-as-an-ap/241861 (Oct 2025)

## Cross-distro aggregator sources

- morrownr/USB-WiFi (in-kernel adapters list): https://github.com/morrownr/USB-WiFi/blob/main/home/USB_WiFi_Adapters_that_are_supported_with_Linux_in-kernel_drivers.md
- morrownr/USB-WiFi (chipsets table): https://github.com/morrownr/USB-WiFi/blob/main/home/USB_WiFi_Chipsets.md
- morrownr/USB-WiFi (firmware install): https://github.com/morrownr/USB-WiFi/blob/main/home/How_to_Install_Firmware_for_Mediatek_based_USB_WiFi_adapters.md
- morrownr/USB-WiFi discussion #294: https://github.com/morrownr/USB-WiFi/discussions/294
- morrownr/7612u repo: https://github.com/morrownr/7612u
- wikidevi mirror mt76: https://wikidevi.wi-cat.ru/Mt76

## Third-party blog posts

- monotux.tech, 2025-08-26: https://www.monotux.tech/posts/2025/08/openwrt-mediatek/
