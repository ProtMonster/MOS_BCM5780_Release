# bcm5780_eth.device

SANA-II network driver for the on-board dual Gigabit Ethernet
(Broadcom BCM5780, PCI 14e4:166a) of the **PowerMac G5 PowerMac11,2**
under **MorphOS 3.x**.

MorphOS supports the on-board Ethernet of the G5 only on PowerMac7,x
models. This driver makes the on-board ports usable on PowerMac11,2.

**Status: beta (v0.8).** Tested so far only on the author's machine.
Testers with a PowerMac11,2 are welcome – see "Feedback" below.

## Download

Get the LHA archive from the
[Releases](https://github.com/ProtMonster/MOS_BCM5780_Release/releases) page.

## Features

- Both ports supported (unit 0 and unit 1), also at the same time
- Works with the MorphOS TCP/IP stack, DHCP, also at boot
- Link loss / cable replug handled
- Promiscuous mode and multicast (all-multicast) support
- Throughput and CPU load similar to a PCIe RTL8168 card in the same
  machine (about 470 Mbit/s raw TCP receive, 37–38 MB/s SMB copy)
- 2-hour full-speed soak test: stable, 0 bad frames

## Installation

1. Copy `bcm5780_eth.device` to `SYS:Devs/Networks/`.
2. Reboot.
3. Open Network prefs, add a new interface:
   - Device: `bcm5780_eth.device` – enter **only the device name**, not a
     path (a path inserted by the file requester fails at boot)
   - Unit: `0` (first port) or `1` (second port)
   - Address: DHCP (or your static settings)
4. Recommended: interface → Expert → "IP Request Buffers" = **64**.
   The default (16) is too small for Gigabit bursts; 64 noticeably
   improves throughput. This is a stack setting and helps other
   Gigabit drivers too.
5. Save and enable the interface.

Check the installed version with:

```
version SYS:Devs/Networks/bcm5780_eth.device
```

## Feedback

Please report results (working or not) either in the morph.zone forum
thread or as a GitHub issue. Useful information:

- Machine model and MorphOS version
- Which port (unit 0 / unit 1) you used
- Whether DHCP and normal use work
- Any problems you noticed
- The output of the included `bcmdiag` tool

## Disclaimer

This is beta software. Use it at your own risk.

## License

BSD-2-Clause. Register offsets, values and the initialisation sequence
are derived from SLOF `lib/libbcm/bcm57xx.c` (IBM, BSD license). Linux
tg3 was used as a reference only; no code was copied. Details are in the
README included in the archive.
