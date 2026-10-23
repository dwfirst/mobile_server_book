# 001 — Mobile Server Book

**A portable, fanless, autonomous server for websites, bots, and personal data — built from off-the-shelf parts, assembled in a local workshop.**

---

## Status

| Field | Value |
|---|---|
| Stage | Concept |
| Revision | v0.1 |
| Target build time | 6 months |
| Production model | Local workshop (laser cut + 3D print + M3 screws) |
| Target unit cost | ~$600 BOM |
| Target retail price | $1,200 – $1,400 |

---

## Problem

Small businesses, digital agencies, and independent developers depend on cloud providers for:
- hosting websites and databases,
- running chat bots for Telegram / VK / Discord,
- storing client documents and knowledge bases.

This dependency creates three concrete risks:

1. **Privacy** — client data and bot conversations leave the owner's control.
2. **Availability** — account blocks, sanctions, or platform policy changes can kill the infrastructure overnight.
3. **Recurring cost** — monthly cloud bills scale with usage and never stop.

Existing alternatives do not fit the gap:
- A powerful laptop is fast but loud, hot, power-hungry, and a single point of failure.
- A cloud VPS is cheap at small scale but contradicts the privacy goal.
- A rack server is not portable and not silent.

---

## Solution

A flat, book-sized device that contains **two independent single-board computers**, each with its own NVMe storage, connected by an internal Gigabit switch. The device:

- runs websites, databases, and bots locally,
- stores files and documents on its own disks,
- is powered by a single USB-C PD power bank,
- is cooled **passively** through a sheet-aluminium back panel and copper heat spreaders,
- has **zero fans** and **zero subscription fees**.

A second external module (Starlink Mini) can be attached by Ethernet when satellite connectivity is needed. It is an accessory, not part of the case.

---

## Target audience

| Segment | Why they need it |
|---|---|
| Digital agencies | Host client bots and small sites without cloud bills |
| Independent developers | Run a private lab for bots, RAG, and small LLMs |
| Small businesses | Keep client data on-premises, not in a foreign cloud |
| Field teams (journalists, geologists) | Operate a local server where infrastructure is absent |
| Privacy-focused users | Own the hardware that holds their data |

---

## Key scenarios

### Scenario 1 — Desk node (office / home)

The device lies on a shelf, connected to a USB-C PD power bank and to the local network.
It serves 2–4 small websites, a PostgreSQL database, and 5–10 bots. The owner manages it over SSH or a local web panel.

### Scenario 2 — Field node

The device is carried in a backpack together with a power bank and, optionally, a Starlink Mini terminal.
It provides a local website, a document archive, and bot endpoints even when no mobile network is available.

### Scenario 3 — Isolated storage node

The second board runs only a file server and a local search index. It is reachable only from the local network, with no route to the internet. This is the "sovereign storage" mode.

---

## What this device is NOT

To keep the concept honest, the following are explicitly out of scope for revision v0.1:

- **Not an AI cluster.** It runs small local models (1.5B–3B) for bot replies. It does not run 7B+ models at interactive speed on ARM.
- **Not a gaming or workstation replacement.** It is a server, not a personal computer.
- **Not a voice assistant.** No microphone, no speaker, no wake word in v0.1.
- **Not a satellite terminal.** Starlink Mini is an external accessory.
- **Not a no-code platform.** Management is via SSH, Docker, and standard Linux tools.
- **Not waterproof.** Dust-resistant and splash-resistant, not IP67.

---

## Why it is feasible in 6 months

- All compute, storage, and power components are **off-the-shelf**.
- No custom PCBs, no injection molds, no CNC milling.
- The case is **laser-cut sheet metal and plastic** plus **3D-printed PETG**.
- Assembly is **screws and connectors**, no soldering beyond plugging cables.
- Software is standard Linux + Docker + Ollama, not a custom OS.

---

## Next documents

- [`01-idea.md`](01-idea.md) — problem, value, and user story in detail
- [`03-architecture/`](03-architecture/) — layers, network, cooling, power
- [`04-spec/`](04-spec/) — BOM, power budget, constraints
- [`05-tradeoffs.md`](05-tradeoffs.md) — what we cut and why
- [`06-roadmap.md`](06-roadmap.md) — 6-month prototyping plan
- [`FUTURE.md`](FUTURE.md) — deferred ideas for later revisions
- [`AUTHORS.md`](AUTHORS.md) — authorship, origin, and intellectual property
