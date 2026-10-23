# Tradeoffs

**What we cut, what we kept, and why. Every decision has an alternative and a rationale.**

---

## How to read this document

Each row is a decision made during the concept phase. The "Cut" column lists what was removed from the first revision. The "Why" column explains the engineering or commercial reason. The "Deferred to" column points to `FUTURE.md` if the idea is worth revisiting.

---

## Hardware tradeoffs

| Decision | Kept | Cut | Why | Deferred to |
|---|---|---|---|---|
| Number of boards | 2× Orange Pi 5 Plus | 4× Orange Pi 5 Plus | Four RK3588 boards in a passive case is a thermal and cost problem. Two boards cover the real workload of the target audience. | No |
| Board model | Orange Pi 5 Plus 16 GB | Raspberry Pi 5 | Native M.2 PCIe 3.0 x4, dual 2.5 GbE, 8 cores. Pi 5 requires an adapter and is limited to PCIe 2.0 x1. | No |
| Cooling | Passive, copper + aluminium | Active fan | Noise, dust ingress, moving part failure. Passive trades peak performance for silence and reliability. | No |
| Enclosure process | Laser cut + 3D print | CNC-milled aluminium tub | Milling wastes 70–80% of material and locks the design to a machine shop. Flat sheet is cheaper and local. | No |
| Power input | USB-C PD 100 W | Proprietary barrel jack | Standard, available everywhere, one cable for charging and running. | No |
| UPS | X-UPS1 with 18650 | Custom LiFePO4 pack with BMS | LiFePO4 is safer but requires spot-welding and a custom BMS, violating the "no custom assembly" constraint. | Maybe |
| Internal switch | Disassembled 5-port Gigabit | Managed switch with VLANs | Managed switch adds cost, size, and configuration complexity. Routing on Board A provides isolation. | No |
| Wi-Fi | M.2 E-Key module on Board A | Built-in Wi-Fi on both boards | Only Board A needs Wi-Fi. A second module adds cost and antenna routing. | No |
| 4G/5G | USB modem | Internal M.2 modem with SIM slot | Internal modem requires antenna cutouts, SIM slot, and extra power. USB modem is plug-and-play. | No |
| Starlink | External accessory | Integrated terminal in case | Starlink Mini does not fit inside a book-sized case and draws 20–40 W. Separate device, separate battery. | No |

---

## Software tradeoffs

| Decision | Kept | Cut | Why | Deferred to |
|---|---|---|---|---|
| Management | SSH + Docker + standard Linux | No-code visual OS | A no-code platform is a separate product requiring a development team and a year of work. Not feasible in 6 months. | Maybe |
| Local LLM size | 1.5B–3B models | 7B–8B models | ARM CPU inference on 7B models is slow and generates sustained heat. Small models give usable bot replies without throttling. | No |
| Orchestration | Docker Compose per board | Kubernetes cluster | Two boards do not need a scheduler. Compose is simpler and debuggable. | No |
| Voice control | None | ESP32-S3 + microphone + speaker | Adds a layer of integration and testing. Not needed for v0.1. | Yes |
| NFC authentication | None | NFC reader + kill switch | Legally and technically sensitive. Adds hardware and software complexity. | Yes |
| GPS and offline maps | None | GPS module + 5.5" touchscreen | Turns the device into a field tablet, increases cost and weight. Not a server feature. | Yes |
| Meshtastic / LoRa | None | ESP32-S3 + SX1262 module | Useful for remote command, but adds another radio and antenna. Deferred to v2. | Yes |
| Spatial RAG | None | Geotagged document retrieval | Requires GPS and a location-aware index. Interesting, but not core. | Yes |
| Bot framework | Standard Telegram/VK/Discord libraries in Docker | Custom agent framework | Custom framework is a software product, not a hardware feature. | No |

---

## Scope tradeoffs

| Decision | Kept | Cut | Why |
|---|---|---|---|
| Waterproofing | Dust and splash resistant | IP67 | IP67 requires gasket design, pressure testing, and sealed connectors. Adds cost and complexity. |
| Ruggedization | Aluminium and PETG frame | MIL-STD drop test | Certification is expensive and not needed for a desk or backpack device. |
| Redundancy | Two independent boards | Two-device cluster with failover | The two-device cluster is a stronger product but doubles cost. The single device already isolates faults between boards. |
| Target audience | Small teams, agencies, field operators | Mass consumer market | Mass market requires retail packaging, support, and certification. The product is niche by design. |
| Price point | $1,200–1,400 | Under $800 | Under $800 requires cheaper boards, smaller storage, or lower margin. The current price reflects real BOM and assembly cost. |

---

## What the cuts buy us

By removing the four-board cluster, the no-code OS, voice control, NFC, GPS, LoRa, and IP67, we get:

- **A buildable device in 6 months.** All parts are off-the-shelf, all processes are local.
- **A BOM under $800.** The base device is affordable for the target audience.
- **An honest spec.** No feature is promised that is not in the first revision.
- **A clear upgrade path.** Deferred ideas are documented in `FUTURE.md`, not lost.

---

## What we would revisit first

If the first revision ships and finds customers, the priority order for v2 is:

1. **Removable thermal booster** — cheap, simple, extends peak load.
2. **USB 4G/5G modem as standard option** — widens the field use case.
3. **LoRa / Meshtastic module** — remote command and mesh networking.
4. **Voice control** — natural interface for field use.
5. **No-code management panel** — lowers the barrier for non-technical users.

Everything else (NFC, GPS, IP67) is lower priority and may never be worth the cost.
