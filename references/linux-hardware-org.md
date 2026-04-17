# linux-hardware.org -- source description

## What it is

linux-hardware.org is a community telemetry catalogue of hardware found in
Linux- and BSD-powered computers. Users opt-in by running the `hw-probe`
tool, which gathers `lspci`, `lsusb`, `dmidecode`, kernel module bindings,
distro identifiers, and other hardware info, anonymises identifying fields,
and uploads the bundle to the public database. Each upload is called a
"probe."

The site exposes:

- Per-device pages keyed by bus + VID + PID (URL pattern
  `/?id=usb:VVVV-PPPP`, `/?id=pci:VVVV-PPPP-...`, etc.). Each page lists
  the kernel driver source file mapped to that ID by LKDDb, the total
  probe count under a `<h2>Status (N)</h2>` header, and a paginated table
  of every host computer that has reported the device, with that host's
  distro, type (laptop/desktop/server/soc/...), and a status flag
  (`works`/`detected`/`failed`).
- A device search at `/?view=search` filterable by vendor name, device
  class, and bus type.
- Aggregated trend views at `/?view=net_wireless_vendor` and
  `/?view=net_wireless_model` covering wireless vendor / model market
  share over time, sliceable by distro and time window.

As of 2026-04-16 the homepage reports **345,735 tested computers** and
**589,547 tested parts** in the database.

## Who runs it

Per the site's contacts page, the
project is led by **Andrey Ponomarenko**, also active on
github.com/linuxhw and github.com/lvc, with credits to LSB Infrastructure,
BroadTest, ROSA Linux, Virtuozzo Storage, Acronis Cyber Infrastructure,
and Acronis Backup Gateway. The github.com/linuxhw organisation hosts
the open-source `hw-probe` collector and the public probe corpus.

## Methodology and data source

- Probes are submitted voluntarily by end users running `hw-probe -all
  -upload`. The tool is BSD-licensed and openly auditable
  (https://github.com/linuxhw/hw-probe).
- Each probe captures the host's hardware tree along with kernel,
  distro, and DMI data. Probes are stored verbatim on GitHub at
  `github.com/linuxhw/Test/tree/master/Notebook` (and adjacent paths)
  alongside the linux-hardware.org rendered views.
- The site joins the raw probe corpus against LKDDb
  (https://cateee.net/lkddb/) to display kernel driver bindings for
  each VID:PID. The driver source file shown on a device page (for
  example `drivers/net/wireless/mediatek/mt76/mt7921/usb.c`) is the
  authoritative signal that the device uses the mt76 USB driver.

## Why it is a credible source for a market-footprint dataset

1. The corpus is large by Linux-USB standards: ~590K parts across
   ~346K computers, accumulating since at least 2014.
2. The probe payloads are public on GitHub, so any specific number
   reported by the site can be back-checked against the underlying
   probe files. There is no opaque server-side filter.
3. Probe submissions are timestamped and per-machine deduplicated via
   stable hash IDs (`hwid` query parameter), so probe counts reflect
   distinct host machines, not multiple uploads from one box.
4. The linux-hardware.org renderer's kernel-driver column makes it
   possible to filter strictly to devices that actually bind to mt76
   on a real Linux host -- not just devices that share MediaTek's
   vendor ID.

## Why it should not be over-trusted

1. **Self-selection bias.** Users who run hw-probe are by definition
   Linux enthusiasts. Distro mix in this data is **not** representative
   of the global Linux-using population, let alone the global PC
   population. ROSA Linux (the maintainer's distro) is wildly
   over-represented in older entries.
2. **Per-machine, not per-shipment.** A USB adapter that has been
   plugged into 100 different reporters' machines counts as 100
   probes, but the same adapter sold 100,000 times to non-reporters
   is invisible.
3. **VID:PID not unique to a chip family.** Manufacturers reuse PIDs
   across generations and silently swap silicon. linux-hardware.org's
   `Name` column is a USB string descriptor or a usb.ids lookup that
   may identify the device under an alias (for instance a TP-Link
   PID that originally shipped with mt76x0 silicon now shows up with
   a Realtek RTL8812AU device name when a later production batch
   swapped the chip). The kernel `By ID` column on each device page
   is the most reliable signal of what driver the device actually
   binds to today.
4. **Combo BT+WiFi devices appear under the BT bus interface.** Many
   M.2 modules built around MT7902/MT7921/MT7922/MT7925 expose
   Bluetooth on USB and WiFi on PCIe. linux-hardware.org sees the
   BT side and labels the entry as "bluetooth," driven by
   `drivers/bluetooth/btusb.c`, even though the part underneath is
   marketed as a WiFi adapter. For a USB-only WiFi market footprint
   these IDs should be excluded or noted as combo modules.

## Endpoints used in this study

| Endpoint | Purpose |
| --- | --- |
| `https://linux-hardware.org/?view=search&vendor=MediaTek&page=N` | Enumerate every device with `MediaTek` as the assigned USB vendor |
| `https://linux-hardware.org/?view=search&vendor=Ralink&page=N` | Enumerate `Ralink Technology` (VID `0x148f`) USB devices including the MT7601U / MT76x0 / MT76x2 rebrands |
| `https://linux-hardware.org/?id=usb:VVVV-PPPP` | Per-device detail page: total probe count, kernel driver source, distro tally |
| `https://linux-hardware.org/?id=usb:VVVV-PPPP&page=N#status` | Paginated host list for a single device |
| `https://linux-hardware.org/?view=net_wireless_vendor` | Aggregated wireless vendor share (interactive, mostly client-side) |
| `https://linux-hardware.org/?view=net_wireless_model` | Aggregated wireless model share |
| `https://linux-hardware.org/?view=trends` | Top-level trend dashboard |
| `https://linux-hardware.org/?view=about` | Project description |
| `https://linux-hardware.org/?view=contacts` | Maintainers |

## Citation footprint

Every numeric claim derived from this source cites the specific
`?id=usb:` URL plus access timestamp. Raw HTML captures are retained
privately.

## Access dates

All linux-hardware.org pages used for this report were fetched on
**2026-04-16 (UTC 2026-04-17 03:07-03:22)** with curl using a desktop
Chrome user-agent.
