# Cooling Layer

**Passive thermal path from SoC to aluminium back panel. No fans, no moving parts.**

---

## Why passive

A fan solves heat but introduces:

- noise (unacceptable in an office or a field camp),
- a moving part that fails first,
- a dust path into the enclosure,
- a dependency on airflow that a closed backpack blocks.

Passive cooling trades peak performance for silence, reliability, and dust resistance. The device throttles under sustained heavy load instead of spinning up a fan. This is documented, not hidden.

---

## Thermal path
RK3588 die
|
| thermal paste
v
Copper heat spreader (50 mm disc, 2 mm thick)
|
| direct contact, clamped by screws
v
Aluminium back panel (3 mm sheet)
|
| natural convection + radiation
v
Ambient air

text

The path has **no intermediate gaps**. Each stage is clamped by the same M3 screws that hold the enclosure together, so contact pressure is uniform.

---

## Materials

| Part | Material | Thickness | Function |
|---|---|---|---|
| Heat spreader | Copper (C110) | 2 mm | Pulls heat from the die, spreads it over 50 mm |
| Back panel | Aluminium (5052 or 6061) | 3 mm | Final radiator, structural back |
| Thermal interface | Paste or graphite pad | 0.2–0.5 mm | Fills micro-gaps between die and copper |
| Clamping | M3 steel screws | — | Uniform pressure across the stack |

**Why copper, not aluminium, for the spreader:** copper conducts heat roughly twice as well as aluminium. A 50 mm copper disc moves heat away from the small die footprint faster than an aluminium disc of the same size.

**Why aluminium, not copper, for the back panel:** the back panel is large and structural. Aluminium is lighter, cheaper, and does not oxidize as visibly. Its lower conductivity is acceptable because the copper disc has already spread the heat over a wide area.

---

## Thermal budget

| Component | Typical load | Peak load |
|---|---|---|
| Board A (gateway, bots, DB) | 5–8 W | 12 W |
| Board B (storage, small LLM) | 8–12 W | 18 W |
| **Total** | **13–20 W** | **30 W** |

At 20 W continuous, a 3 mm aluminium panel of approximately 0.06 m² with natural convection holds the SoC around 55–65 °C in a 25 °C room. This is below the RK3588 throttling threshold (around 80–85 °C) but close enough that sustained peak load will trigger throttling.

**Honest limit:** the device is designed for **light and medium workloads**. Running a 7B model on Board B for hours will cause throttling. Running websites and bots at normal traffic will not.

---

## What throttling looks like

When the SoC approaches its thermal limit:

1. The kernel reduces CPU frequency (cpufreq governor steps down).
2. Inference speed on Board B drops from ~10 tokens/s to ~5 tokens/s.
3. Website response time increases by tens of milliseconds.
4. The device does **not** shut down. It degrades gracefully.

This is acceptable for the target workload. It is not acceptable for a user who expects workstation-class sustained performance. The README states this explicitly.

---

## Optional thermal booster

A **removable ribbed aluminium plate** can be attached to the back panel with neodymium magnets. It increases surface area and extends the time before throttling under sustained load.

- **When attached:** the device handles peak load for longer.
- **When removed:** the device is thinner and lighter for transport.

The booster is an accessory, not part of the base device. It is described in `04-spec/` and shown in `02-sketch/`.

**Magnet constraint:** magnets alone do not conduct heat. The booster must have copper posts that align with the copper discs on the back panel, plus graphite pads at the contact points. Magnets only hold the plate in place; they are not the thermal interface.

---

## What is not in this layer

- **No heat pipes.** They add height, cost, and a failure mode (dry-out, bending).
- **No vapour chamber.** Overkill for 20 W.
- **No active cooling.** No fans, no Peltier elements.
- **No liquid cooling.** Obviously.
- **No thermal simulation in v0.1.** The design is based on empirical data from similar passive builds. A thermal mockup is part of the roadmap (see `06-roadmap.md`).
