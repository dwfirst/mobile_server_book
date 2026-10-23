# Network Layer

**Internal switch, external uplink, isolation, and remote access.**

---

## Topology
Internet / Starlink Mini / Router
|
| Ethernet (external uplink)
|
+---------v----------+
| Internal switch |
| 5-port Gigabit |
+--+------+------+---+
| | |
+----v-+ +--v---+ +v-----+
|BoardA| |BoardB| |spare |
+------+ +------+ +------+

text

The internal switch is a **disassembled 5-port Gigabit Ethernet switch**, mounted inside the enclosure without its plastic shell. It connects:

- Board A (gateway),
- Board B (storage),
- external uplink,
- two spare ports for service access or a future third board.

**Why a switch, not direct cables:** a switch gives every board a stable link, isolates traffic between ports, and allows the uplink to move between ports without rewiring.

---

## Board roles in the network

| Board | IP (example) | Reachable from | Internet access |
|---|---|---|---|
| Board A | 192.168.10.10 | LAN, external uplink | Yes (routed) |
| Board B | 192.168.10.20 | Board A only | No (no route) |
| Switch | unmanaged | — | — |

Board B is intentionally **not routable to the internet**. It is reachable only from Board A over the internal switch. This is the "sovereign storage" mode: the archive is on the network, but not on the internet.

---

## External uplink

The device has **one external Ethernet port** on the side panel. It can be connected to:

1. **A home or office router** — normal desk mode.
2. **A Starlink Mini terminal** — field mode, when satellite connectivity is needed.
3. **A USB 4G/5G modem** — fallback, plugged into Board A, appears as a second network interface.

The uplink is **not** a second internal switch port. It terminates on Board A, which acts as the router between the internal network and the outside world.

**Why Board A routes, not the switch:** routing decisions belong to software (iptables, nftables, or a small OpenWrt VM). The switch is unmanaged and does not route.

---

## Isolation of Board B

Board B has no default route. Its only network path is to Board A over the internal switch. To reach Board B from the outside, a user must:

1. Connect to Board A (SSH or web panel).
2. From Board A, open an SSH session to Board B.

This is deliberate. The storage board holds documents and backups. It should not be exposed to the internet, even behind a firewall, because a single misconfiguration on Board A would then expose it.

**Optional hardening:** a physical toggle switch on the back panel that disconnects Board B's Ethernet cable. When the toggle is off, Board B is unreachable even from Board A. This is a hardware kill switch for the storage node.

---

## Remote access

For remote management when the device is not on the local network:

- **WireGuard** on Board A, with a single peer configuration per user.
- **No open ports** on the external uplink except WireGuard's UDP port.
- **SSH** only over WireGuard, never directly exposed.
- **Web panel** (if any) bound to localhost, reached through an SSH tunnel.

**What this is not:** no cloud relay, no third-party VPN, no vendor account. The device connects only to endpoints the owner configures.

---

## Wi-Fi

Board A carries an **M.2 E-Key Wi-Fi 6 module**. It provides:

- a local access point for laptops and phones when no router is present,
- a client connection to an existing Wi-Fi network when Ethernet is not available.

**Antenna placement:** two small antennas routed along the top edge of the enclosure, away from the aluminium back panel to avoid shielding.

**Constraint:** Wi-Fi and the internal switch share Board A's network stack. Under heavy Wi-Fi traffic, bot latency can increase. This is documented and accepted for v0.1.

---

## What is not in this layer

- **No managed switch.** VLANs and port mirroring are not available in v0.1. Isolation is done by routing, not by switch configuration.
- **No redundant uplink.** One external port. A second uplink is a future revision.
- **No built-in LTE.** 4G/5G is a USB modem, not an internal M.2 card, to avoid antenna and SIM slot complexity in v0.1.
- **No mesh radio.** LoRa and Meshtastic are deferred to `FUTURE.md`.
