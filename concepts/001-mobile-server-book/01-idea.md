# 01 — Idea

**Why this device exists, who it is for, and what value it delivers.**

---

## The core problem

Small teams and independent operators increasingly run digital infrastructure that used to belong to large organizations:

- a website or two,
- a database,
- a handful of chat bots in Telegram, VK, or Discord,
- a growing archive of client documents.

The default answer today is "put it in the cloud." That answer carries three hidden costs:

1. **Privacy loss.** Client data, bot conversations, and documents live on someone else's hardware, under someone else's jurisdiction.
2. **Availability risk.** Account blocks, sanctions, payment failures, or policy changes can take the entire infrastructure offline with no recourse.
3. **Recurring spend.** Monthly bills scale with usage and never end, even when the workload is small.

A laptop is fast but loud, hot, power-hungry, and a single point of failure. A cloud VPS is cheap at small scale but contradicts the privacy goal. A rack server is neither portable nor silent.

---

## The idea

Build a **flat, book-sized, fanless server** that:

- runs websites, databases, and bots locally,
- stores files on its own NVMe disks,
- is powered by a single USB-C PD power bank,
- is cooled passively through sheet aluminium and copper heat spreaders,
- costs less than a year of equivalent cloud hosting,
- can be assembled in a local workshop from off-the-shelf parts.

The device is not a cloud replacement for heavy workloads. It is a **sovereign node for light and medium tasks**: sites, bots, documents, small local models.

---

## Value proposition

| For | Value |
|---|---|
| Digital agencies | Host client bots and small sites without cloud bills |
| Independent developers | Private lab for bots, RAG, and small LLMs |
| Small businesses | Client data stays on-premises |
| Field teams | Local server where infrastructure is absent |
| Privacy-focused users | Own the hardware that holds their data |

---

## User stories

### Story 1 — The agency owner

> "I run five client bots and two small sites. Cloud bills are $180/month and growing. I want a box on my shelf that does the same job, is silent, and I own it."

The device replaces the cloud VPS for light workloads. The owner keeps the same Docker-based workflow but pays once.

### Story 2 — The independent developer

> "I want to experiment with local LLMs and RAG without sending my data to OpenAI. I need a small, quiet machine that runs 1.5B–3B models and a vector database."

The device provides a local inference target with no API keys and no per-token cost.

### Story 3 — The field operator

> "I work in places where the mobile network is unreliable. I need a local website, a document archive, and bot endpoints that work even when the internet does not."

The device runs fully offline for local services and can attach a Starlink Mini terminal when satellite connectivity is available.

---

## What makes it different

- **Fanless by design.** No moving parts, no noise, no dust ingress through fans.
- **Two independent nodes.** If one board fails, the other keeps running its services.
- **Off-the-shelf parts.** No custom PCBs, no injection molds, no CNC milling.
- **Assembled locally.** Laser cutting, 3D printing, M3 screws. Buildable in any workshop.
- **Honest scope.** It does not pretend to be an AI cluster or a workstation. It is a quiet, portable server for real, small workloads.

---

## Non-goals (v0.1)

- Running 7B+ models at interactive speed on ARM.
- Replacing a gaming laptop or a workstation.
- Voice control, NFC authentication, or no-code management UI.
- Waterproofing (IP67) or ruggedization beyond dust and splash resistance.
- Built-in satellite terminal.

These are deferred, not rejected. See [`FUTURE.md`](FUTURE.md).
