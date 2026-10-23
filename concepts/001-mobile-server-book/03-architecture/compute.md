# Compute Layer

**Two independent boards, each with its own storage and role.**

---

## Why two boards, not one

A single board can run websites, bots, and storage. A single board is also a single point of failure: one kernel panic, one disk failure, one thermal event, and everything goes down together.

Two boards give:

- **Fault isolation.** A crash on Board A does not stop Board B.
- **Thermal spread.** Two SoCs at moderate load generate less peak heat than one SoC at full load.
- **Storage separation.** Each board owns its NVMe disk. No shared bus, no contention.
- **Clear roles.** Each board has a defined job, not a mix of everything.

Four boards were considered and rejected: thermal density, cost, and assembly complexity grow faster than useful capacity for the target workload.

---

## Board A — Gateway and bots

| Field | Value |
|---|---|
| Board | Orange Pi 5 Plus, 16 GB RAM |
| SoC | Rockchip RK3588, 8 cores (4×A76 + 4×A55) |
| Storage | NVMe SSD, 512 GB, M.2 2280, PCIe 3.0 x4 |
| Network | Internal Gigabit to switch; Wi-Fi 6 via M.2 E-Key module |
| Role | Entry point, reverse proxy, database, bot runtime |

**Responsibilities:**

- Nginx as reverse proxy and TLS terminator.
- PostgreSQL for websites and bot state.
- Docker runtime for 5–10 bots (Telegram, VK, Discord).
- Wi-Fi access point for local clients (laptop, phone).
- Optional USB 4G/5G modem as fallback uplink.

**Why this board carries the network role:** it is the only board with an M.2 E-Key slot available for a Wi-Fi module, and its position in the enclosure simplifies antenna routing.

---

## Board B — Storage and local inference

| Field | Value |
|---|---|
| Board | Orange Pi 5 Plus, 16 GB RAM |
| SoC | Rockchip RK3588, 8 cores (4×A76 + 4×A55) |
| Storage | NVMe SSD, 1 TB, M.2 2280, PCIe 3.0 x4 |
| Network | Internal Gigabit to switch only |
| Role | File server, local search, small local models |

**Responsibilities:**

- File server (Nextcloud or Seafile) for documents and archives.
- Local vector index for document search.
- Ollama with small models (Qwen-1.5B, Phi-3-mini) for bot replies.
- Backup target for Board A database dumps.

**Why this board carries storage:** 1 TB NVMe on a direct PCIe 3.0 x4 link gives sequential reads above 3 GB/s, which matters for vector index loading and file serving. Splitting storage from the gateway keeps disk I/O away from the network path.

---

## Storage layout

| Board | Disk | Contents |
|---|---|---|
| A | 512 GB NVMe | OS, Docker images, PostgreSQL data, bot logs |
| B | 1 TB NVMe | OS, file archive, vector index, small models, backups |

**Total raw capacity:** 1.5 TB.
**Effective capacity after OS and models:** approximately 1.2 TB.

No RAID is used. Each disk is independent. Redundancy, if needed, is a software concern (rsync, restic, or Borg to an external drive), not a hardware one.

---

## What each board does NOT do

- **Board A does not run local LLMs.** Its CPU time belongs to network and database work. Model inference on the gateway would compete with bot traffic.
- **Board B does not face the internet.** It has no route to the external uplink. It is reachable only from Board A over the internal switch.
- **Neither board runs a cluster scheduler.** They are two servers on one network, not a Kubernetes cluster.

---

## Upgrade path

- **RAM:** both boards use soldered LPDDR5. 16 GB is the ceiling. This is a constraint accepted at design time.
- **Storage:** both boards accept M.2 2280 NVMe up to 4 TB. A future revision can double capacity without changing the enclosure.
- **Compute:** if a future RK3588 successor board appears with the same footprint, it can replace either board without redesigning the case.
