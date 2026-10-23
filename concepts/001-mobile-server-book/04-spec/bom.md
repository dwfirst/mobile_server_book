# Bill of Materials

**Every component, its role, and its estimated cost. All parts are off-the-shelf.**

---

## Compute

| Part | Qty | Unit price (USD) | Notes |
|---|---|---|---|
| Orange Pi 5 Plus, 16 GB RAM | 2 | 135–180 | RK3588, 8 cores, dual 2.5 GbE, M.2 M-Key PCIe 3.0 x4 [citation:7] |
| NVMe SSD, 512 GB, M.2 2280 | 1 | 35–50 | Board A (gateway) |
| NVMe SSD, 1 TB, M.2 2280 | 1 | 50–70 | Board B (storage) |

**Why Orange Pi 5 Plus:** it has a native M.2 M-Key slot with PCIe 3.0 x4, which gives NVMe speeds up to ~2000 MB/s . Raspberry Pi 5 requires an adapter and is limited to PCIe 2.0 x1. For a storage node, this difference matters.

---

## Network

| Part | Qty | Unit price (USD) | Notes |
|---|---|---|---|
| 5-port Gigabit Ethernet switch (disassembled) | 1 | 15–20 | Remove plastic shell, keep PCB |
| M.2 E-Key Wi-Fi 6 module | 1 | 15–25 | For Board A local access point |
| Wi-Fi antennas, 2 pcs | 1 set | 3–5 | Routed along top edge |
| Ethernet cables, short, 4 pcs | 1 set | 5–8 | Internal wiring |

---

## Power

| Part | Qty | Unit price (USD) | Notes |
|---|---|---|---|
| Geekworm X-UPS1 | 2 | 22–28 | Stackable UPS, 4× 18650 per module, 5V/3A output each [citation:9][citation:10] |
| 18650 Li-ion cells, protected, 3000–3500 mAh | 8 | 4–6 | 4 per UPS module |
| USB-C PD 100W charger or power bank | 1 | 30–50 | Input to X-UPS1 via USB-C to DC barrel adapter |
| DC barrel adapter, USB-C to 5.5×2.1 mm | 1 | 3–5 | Connects PD source to X-UPS1 |

**Why X-UPS1:** it combines charging, battery management, and regulated 5V output on one board. It accepts 6–24V input and outputs 5V/3A per module, stackable to 6A [citation:10]. Two modules cover two boards plus the switch.

**Why not a custom BMS:** spot-welding 18650 cells and building a BMS from discrete parts violates the "no custom assembly" constraint. X-UPS1 uses standard cell holders.

---

## Display and front panel

| Part | Qty | Unit price (USD) | Notes |
|---|---|---|---|
| OLED display, 1.3", SPI/I2C, 64×128 | 2 | 8–12 | One per board, shows IP, temperature, status [citation:6][citation:13] |
| Front acrylic sheet, dark blue, 2 mm | 1 | 8–12 | Laser cut |
| Front ABS panel, matte black, 3 mm | 1 | 10–15 | Laser cut, perforated |
| Silicone O-ring cord, 1.5 mm | 1 m | 3–5 | Perimeter seal |

---

## Cooling and enclosure

| Part | Qty | Unit price (USD) | Notes |
|---|---|---|---|
| Copper sheet, C110, 2 mm | 1 sheet (200×200 mm) | 15–20 | Laser cut into 50 mm discs |
| Aluminium sheet, 5052, 3 mm | 1 sheet (250×200 mm) | 15–20 | Back panel |
| PETG filament, 1 kg | 1 | 20–25 | 3D-printed frame |
| M3 stainless screws, assorted | 1 set | 5–8 | Countersunk, 10–20 mm |
| M3 nuts, low-profile | 1 set | 3–5 | |
| Thermal paste or graphite pad | 1 | 5–8 | Between SoC and copper |

---

## Optional accessories (not in base BOM)

| Part | Unit price (USD) | Notes |
|---|---|---|
| Removable ribbed aluminium booster | 15–25 | 5 mm sheet, magnets, graphite pads |
| USB 4G/5G modem | 30–60 | Plug into Board A USB port |
| Starlink Mini | ~$600 | External terminal, separate power bank [citation:4] |
| Folding solar panel, 50–100W | 60–100 | External, feeds X-UPS1 input |

---

## Total BOM (base device, no accessories)

| Category | Low estimate | High estimate |
|---|---|---|
| Compute | $305 | $410 |
| Network | $38 | $58 |
| Power | $135 | $185 |
| Display and front panel | $39 | $56 |
| Cooling and enclosure | $63 | $86 |
| **Total** | **$580** | **$795** |

**Target retail price: $1,200–1,400.** This is approximately 2× BOM, which covers assembly, testing, packaging, and margin.

---

## Sourcing notes

- **Orange Pi 5 Plus:** available from Chinese distributors at $135–180 depending on volume [citation:7]. Retail in Russia is higher (around 28,000 RUB) , but the BOM uses direct-from-manufacturer pricing for a small batch.
- **X-UPS1:** available from Geekworm and SpotPear. The SpotPear listing confirms the 6–24V input, 5V/3A output, and stackable design [citation:9].
- **OLED displays:** Waveshare and Seeed Studio both make 1.3" SPI/I2C modules in the $8–12 range [citation:6][citation:13].
- **Sheet materials:** local metal and plastic suppliers. Laser cutting services are widely available.

---

## What is not included

- **Batteries for Starlink Mini.** Separate power bank, separate budget.
- **Assembly labor.** Estimated at 15–20 minutes per unit for a trained person.
- **Testing equipment.** Thermal camera, multimeter, network tester — assumed to exist in the workshop.
- **Custom PCBs.** None. All connections are standard cables and headers.
