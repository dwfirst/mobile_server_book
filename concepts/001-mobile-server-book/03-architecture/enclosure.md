# Enclosure Layer

**Sandwich construction, materials, assembly order, and what is deliberately not milled.**

---

## Why a sandwich

A machined aluminium tub is the obvious way to build a small server. It is also the wrong way for this project:

- CNC milling is expensive for a single unit and worse for a small batch.
- A milled tub wastes 70–80% of the aluminium as chips.
- It locks the design to a machine shop that most local workshops do not have.

The sandwich uses **flat sheet material** cut by laser and **3D-printed plastic** for the structural frame. Every part is either cut or printed. Nothing is milled.

---

## Stack (front to back)
[ Front acrylic sheet ] dark blue, 2 mm, laser cut
|
[ Front ABS panel ] matte black, 3 mm, laser cut, perforated
|
[ 3D-printed frame ] PETG, holds boards and switch
|
[ Copper spreaders ] 50 mm discs, 2 mm, laser cut
|
[ Aluminium back panel ] 3 mm sheet, laser cut

text

The stack is clamped by **M3 screws** that pass through all layers. The screws are countersunk on the front and capped with low-profile nuts on the back.

---

## Materials

| Part | Material | Thickness | Process |
|---|---|---|---|
| Front acrylic | Dark blue cast acrylic | 2 mm | Laser cut |
| Front panel | Matte black ABS | 3 mm | Laser cut, perforated |
| Structural frame | PETG | — | 3D printed |
| Copper spreaders | C110 copper | 2 mm | Laser cut |
| Back panel | 5052 or 6061 aluminium | 3 mm | Laser cut |
| Screws | Stainless steel M3 | — | Off-the-shelf |
| Gaskets | Silicone O-ring cord | 1.5 mm | Cut to length |

**Why PETG, not PLA:** PLA softens around 60 °C. The frame sits next to copper spreaders and warm boards. PETG holds its shape up to ~80 °C and is not brittle.

**Why cast acrylic, not extruded:** cast acrylic laser-cuts with clean edges and does not craze around the cut. Extruded acrylic is cheaper but looks worse under the OLED screens.

---

## Dimensions (target)

| Dimension | Value |
|---|---|
| Width | 220 mm |
| Height | 160 mm |
| Thickness | 35 mm |
| Weight (without batteries) | ~1.2 kg |
| Weight (with 8× 18650) | ~1.5 kg |

The footprint is slightly smaller than an A4 sheet, so the device fits in a laptop sleeve.

---

## Assembly order

1. **Laser-cut all sheet parts** — front acrylic, front ABS, copper spreaders, back aluminium.
2. **Print the frame** — PETG, one piece, with cutouts for boards, switch, UPS modules, and cable routing.
3. **Mount the copper spreaders** on the frame, aligned with the SoC positions on each board.
4. **Install the boards** — screw each Orange Pi to the frame, apply thermal paste between SoC and copper spreader.
5. **Install the switch and UPS modules** — double-sided thermal tape to the back panel, cables routed along the frame.
6. **Connect cables** — Ethernet, USB-C power, OLED ribbon cables.
7. **Close the sandwich** — front ABS, then front acrylic, then M3 screws through all layers.
8. **Test** — power on, check OLED screens, check network, measure temperatures under load.

Target assembly time for a trained person: **15–20 minutes**.

---

## Perforation and ventilation

The front ABS panel has a **perforated pattern** for two reasons:

- **Air exchange.** Passive cooling relies on natural convection. Perforations allow cool air in and warm air out, even though there is no fan.
- **Visual identity.** The pattern is part of the device's look. It is not decorative noise; it is a functional grille.

The perforation pattern is generated as a vector file and cut by the same laser as the panel outline.

---

## Sealing

The device is **dust-resistant and splash-resistant**, not waterproof.

- Silicone O-ring cord runs along the perimeter between the front ABS and the frame.
- The USB-C port has a silicone plug when not in use.
- The Ethernet port sits behind a small rubber flap.

**What this is not:** IP67. Achieving IP67 requires a gasket design, pressure testing, and a sealed connector ecosystem, which adds cost and complexity beyond the scope of v0.1.

---

## Optional thermal booster

A **removable ribbed aluminium plate** can be attached to the back panel with neodymium magnets.

- Plate: 5052 aluminium, 5 mm, laser cut with ribs.
- Magnets: 8× N42 neodymium discs, 10 mm diameter, pressed into the plate.
- Thermal interface: graphite pads on copper posts that align with the spreaders.

**Why magnets, not screws:** the booster is an accessory. It should attach and detach in seconds, without tools, and without leaving holes in the back panel.

**Why graphite pads, not paste:** paste is messy and dries out. Graphite pads are reusable and do not migrate.

---

## What is not in this layer

- **No CNC-milled tub.** Flat sheet only.
- **No injection-molded parts.** 3D printing only.
- **No custom hinges, latches, or spring-loaded mechanisms.** Screws and magnets.
- **No IP67 certification.** Dust and splash resistance only.
- **No integrated handle or strap.** The device is carried in a sleeve or a bag.
