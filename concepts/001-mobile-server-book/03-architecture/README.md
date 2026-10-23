# 03 — Architecture

**System layers, node roles, network, cooling, and power — as a set of small, reviewable documents.**

---

## Overview

The Mobile Server Book is organized as a stack of independent layers.
Each layer has a single responsibility and can be reasoned about on its own.

| Layer | Document | Responsibility |
|---|---|---|
| Compute | [`compute.md`](compute.md) | Two independent boards, their roles, and storage |
| Network | [`network.md`](network.md) | Internal switch, external uplink, isolation |
| Cooling | [`cooling.md`](cooling.md) | Passive thermal path from SoC to aluminium back panel |
| Power | [`power.md`](power.md) | USB-C PD input, UPS module, distribution to boards |
| Enclosure | [`enclosure.md`](enclosure.md) | Sandwich construction, materials, assembly |

---

## Design constraints

These constraints shape every layer:

1. **Two boards, not four.** Four Orange Pi 5 Plus in a passive case is a thermal and cost problem. Two boards cover the real workload of the target audience.
2. **No fans.** Zero moving parts in v0.1. Cooling is entirely passive.
3. **No custom PCBs.** All electronics are off-the-shelf modules connected by standard cables.
4. **No CNC milling.** Enclosure is laser-cut sheet material plus 3D-printed PETG.
5. **One external power input.** USB-C PD only. No proprietary connectors.
6. **Honest thermal limits.** If the workload exceeds passive cooling, the device throttles. This is documented, not hidden.

---

## High-level block diagram (text)
+-----------------------------+
| USB-C PD 100W input |
+--------------+--------------+
|
+--------------v--------------+
| UPS module (18650 cells) |
+--------------+--------------+
|
+---------------------+---------------------+
| |
+----------v----------+ +-----------v---------+
| Board A (Gateway) | | Board B (Storage) |
| - Nginx | | - File server |
| - PostgreSQL | | - Local search |
| - Bots (Docker) | | - Small LLM |
| - SSD 512 GB | | - SSD 1 TB |
+----------+----------+ +-----------+---------+
| |
+---------------------+---------------------+
|
+--------------v--------------+
| Internal Gigabit switch |
+--------------+--------------+
|
+--------------v--------------+
| External Ethernet uplink |
| (router / Starlink Mini) |
+-----------------------------+

text

---

## What is deliberately not here

- **No cluster orchestration.** The two boards are independent. They share a network but not a scheduler.
- **No shared storage.** Each board owns its disk. File sync between them is a software concern, not hardware.
- **No built-in Wi-Fi in the diagram.** Wi-Fi is an M.2 module on Board A, shown in `network.md`.
- **No satellite terminal inside the case.** Starlink Mini is an external accessory.

---

## Documents in this folder

- [`compute.md`](compute.md) — boards, roles, storage layout
- [`network.md`](network.md) — switch, uplink, isolation, remote access
- [`cooling.md`](cooling.md) — thermal path, materials, limits
- [`power.md`](power.md) — UPS, distribution, runtime estimates
- [`enclosure.md`](enclosure.md) — sandwich stack, materials, assembly order
