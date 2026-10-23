# Power Budget

**Measured draw, runtime estimates, and what happens when the battery runs out.**

---

## Component draw

| Component | Idle | Typical | Peak | Source |
|---|---|---|---|---|
| Orange Pi 5 Plus (single) | ~3 W | 6–8 W | ~12 W | Measured on 16 GB model with NVMe and Wi-Fi [citation:1] |
| Internal Gigabit switch | 1–2 W | 2 W | 2 W | Datasheet estimate |
| Wi-Fi 6 module | 0.5 W | 1 W | 1.5 W | Datasheet estimate |
| OLED displays (2) | 0.2 W | 0.3 W | 0.3 W | Datasheet estimate |
| **Server total (two boards)** | **~8 W** | **15–20 W** | **~28 W** | Sum of above |

**Note on Orange Pi idle:** reports vary between 2.2 W and 5 W depending on kernel and whether an NVMe disk is attached [citation:9][citation:13]. The 3 W figure is a conservative middle ground for a board with NVMe mounted but idle.

---

## Battery capacity

Two X-UPS1 modules, each holding four 18650 cells.

| Parameter | Value |
|---|---|
| Cells per module | 4 |
| Modules | 2 |
| Total cells | 8 |
| Cell capacity (nominal) | 3,000–3,500 mAh |
| Cell energy (3.7 V nominal) | ~11–13 Wh |
| **Raw pack energy** | **~88–104 Wh** |
| Usable after conversion losses (~85%) | **~75–88 Wh** |

The X-UPS1 outputs 5 V at 3 A per module, stackable to 6 A [citation:2]. Two modules give enough current headroom for two boards plus the switch.

---

## Runtime (server only, no Starlink)

Using **80 Wh** usable energy as the planning figure:

| Scenario | Draw | Runtime | What it means |
|---|---|---|---|
| Idle (boards sleeping, network alive) | ~8 W | ~10 hours | Overnight standby, bots receive but do not reply |
| Typical (websites, bots, light storage) | ~15 W | ~5.5 hours | Normal office or field session |
| Heavy (both boards loaded, small LLM running) | ~25 W | ~3.2 hours | Bot traffic spike plus document indexing |
| Peak (throttling imminent) | ~30 W | ~2.7 hours | Sustained worst case, not recommended |

**These numbers are estimates, not measurements.** The first prototype will include a USB power meter and a thermal camera. Real numbers go into `06-roadmap.md` after the build.

---

## Starlink Mini: separate budget

Starlink Mini draws **20–40 W average** in active use [citation:3][citation:4]. It requires a **USB-C PD 100 W** source (20 V / 5 A) . It is **not** powered from the X-UPS1.

| Accessory | Draw | Power source | Runtime |
|---|---|---|---|
| Starlink Mini | 20–40 W | Dedicated 100 W PD power bank | ~2.5 hours on a 99 Wh bank  |

**Why separate:** if Starlink shared the server battery, a 99 Wh pack would be exhausted in under an hour by the terminal alone. The server would then shut down while the satellite link was still the only uplink. Separate batteries mean the server survives even when Starlink is depleted.

---

## Charging

| Parameter | Value |
|---|---|
| Input | USB-C PD 100 W, via USB-C to DC barrel adapter |
| X-UPS1 charging current | 2.3–3.2 A per module [citation:2] |
| Full charge time (8 cells, two modules) | ~4–5 hours from 100 W source |
| Charge while running | Yes, X-UPS1 passes through input power |

Charging is overnight, not rapid-swap. The X-UPS1 does not support hot-swapping cells while the device is running.

---

## Low battery behavior

The X-UPS1 reports low battery and AC loss as GPIO signals [citation:2]. The software stack on Board A should:

1. **At 20% remaining:** log a warning, send an alert to the admin (Telegram, email, or OLED).
2. **At 10% remaining:** stop accepting new bot conversations, finish in-flight requests.
3. **At 5% remaining:** initiate graceful shutdown of both boards.

**What happens if the battery dies without warning:** the boards lose power. Databases may need recovery on next boot. This is why the UPS module exists: to prevent this scenario, not to survive it indefinitely.

---

## What this budget does not include

- **Solar input.** A folding panel feeding the X-UPS1 input is an accessory, not part of the base device. See `FUTURE.md`.
- **USB 4G/5G modem.** Adds 2–5 W depending on model. Not in the base budget.
- **External drives.** If a user attaches a USB SSD for backup, add 2–3 W.
- **Thermal throttling effect.** Throttling reduces power draw, which extends runtime. The "heavy" scenario assumes the device is not yet throttling.

---

## Honest summary

The Mobile Server Book is a **5-hour server on battery**, not a 24-hour one. It is designed to be charged overnight and run through a workday. For longer field deployment, a second battery pack or a solar panel is required. This is a physical limit of 18650 cells and 20 W continuous draw, not a design flaw.
