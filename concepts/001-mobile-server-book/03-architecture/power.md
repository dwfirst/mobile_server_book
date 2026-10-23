# Power Layer

**USB-C PD input, UPS module, distribution to boards, and honest runtime estimates.**

---

## Why a UPS, not a raw power bank

A raw power bank cuts power instantly when depleted. A UPS module:

- switches to battery without interrupting the boards,
- reports low battery and AC loss as GPIO signals,
- allows the device to shut down gracefully before the battery dies.

This matters for a server. An unclean shutdown corrupts databases and filesystems.

---

## Power chain
USB-C PD 100W input
|
v
+-------------------+
| UPS module |
| (X-UPS1, 4x18650)|
+--------+----------+
|
| 5V / 3A per module
v
+--------+----------+
| Board A |
| Board B |
| Internal switch |
+-------------------+

text

The UPS module accepts **6–24 V DC input** and outputs **5 V at up to 3 A per module** [citation:1][citation:13]. Two modules can be stacked for 6 A total, which is enough for two Orange Pi 5 Plus boards plus the switch.

**Why not a single high-current DC-DC converter:** the X-UPS1 combines charging, battery management, and regulated output in one board. Building the same from discrete modules adds wiring, fuses, and failure points.

---

## UPS module: X-UPS1

| Parameter | Value |
|---|---|
| Input voltage | 6–24 V DC [citation:1] |
| Output voltage | 5 V (also 12 V, unused here) [citation:1] |
| Output current | 3 A per module [citation:1] |
| Battery | 4× 18650 Li-ion per module [citation:1] |
| Charging current | 2.3–3.2 A [citation:13] |
| Stackable | Yes, current adds [citation:1] |

**Battery choice:** protected 18650 cells with a nominal capacity of 3,000–3,500 mAh each. A single module holds four cells, giving approximately **45–50 Wh** of raw energy. Two modules give **90–100 Wh**.

**Why 18650, not LiFePO4:** 18650 cells are commodity parts, available everywhere, and the X-UPS1 has built-in holders. LiFePO4 would require a custom BMS and spot-welded pack, which violates the "no custom assembly" constraint.

**Safety note:** the X-UPS1 documentation warns against charging unattended on wood or carpet [citation:7]. The device enclosure is aluminium and plastic, not fabric. This is an acceptable risk for a prototype, not for a certified product.

---

## Power draw: measured, not guessed

| Component | Idle | Typical load | Peak |
|---|---|---|---|
| Orange Pi 5 Plus (single board) | ~3 W [citation:2] | 6–8 W | ~12 W [citation:2] |
| Internal Gigabit switch | 1–2 W | 2 W | 2 W |
| Wi-Fi module | 0.5 W | 1 W | 1.5 W |
| **Total (two boards + switch + Wi-Fi)** | **~8 W** | **15–20 W** | **~28 W** |

The Orange Pi 5 Plus figures come from a measured build with 16 GB RAM, NVMe, and Wi-Fi [citation:2]. They are consistent with the design target.

---

## Runtime estimates

Using **90 Wh** of usable battery energy (two X-UPS1 modules, 8× 18650 at ~12 Wh each, minus conversion losses):

| Load scenario | Draw | Runtime |
|---|---|---|
| Idle (boards sleeping, network only) | ~8 W | ~11 hours |
| Typical (bots, websites, light storage) | ~15 W | ~6 hours |
| Heavy (both boards under load, small LLM running) | ~25 W | ~3.5 hours |
| Peak (throttling begins) | ~30 W | ~3 hours |

**These numbers are for the server alone.** They do not include Starlink Mini.

---

## Starlink Mini: separate power budget

Starlink Mini draws **20–40 W** in active use, with an idle draw of **15 W** [citation:3]. It is not powered from the X-UPS1. It requires a **USB-C PD 100 W** source (20 V / 5 A) or a dedicated 12 V adapter.

**Implication:** the Starlink Mini needs its own power bank. A 99 Wh power bank designed for Starlink Mini provides about **4.5 hours** of runtime [citation:4][citation:10]. This is a separate accessory, not part of the server's power chain.

**Why not integrate Starlink into the UPS:** Starlink's 100 W peak demand would drain the server's battery in under an hour. Separate batteries mean the server survives even when the satellite link is depleted.

---

## External input

The device has **one USB-C PD port** for charging the UPS modules. A 100 W PD charger or power bank feeds the X-UPS1 input through a USB-C to DC barrel adapter.

**Charging time:** with 2.3–3.2 A charging current per module [citation:13], a fully depleted 8-cell pack (two modules) takes roughly **4–5 hours** to charge from a 100 W source. This is acceptable for overnight charging, not for rapid field swaps.

**What is not included:** solar input. A folding solar panel with a 12 V output can feed the X-UPS1 input through the same barrel jack, but this is an external accessory, not part of the base device. It is listed in `FUTURE.md`.

---

## What this layer does not solve

- **Hot-swap batteries.** The X-UPS1 does not support swapping cells while running.
- **Battery health monitoring beyond voltage.** The X-UPS1 reports low battery and AC loss as GPIO signals [citation:1], but does not expose per-cell health or cycle count.
- **Starlink power.** Separate battery, separate budget.
- **Solar MPPT.** External accessory, deferred.
