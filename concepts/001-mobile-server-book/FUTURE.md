# Future Ideas

**Ideas that were cut from v0.1 but are worth revisiting. Nothing here is promised.**

---

## Hardware

### Removable thermal booster
A ribbed aluminium plate attached to the back panel with neodymium magnets. Increases surface area for sustained load. **Why deferred:** accessory, not core. Easy to add in v1.1.

### LiFePO4 battery pack
Replace X-UPS1 with a custom 12 V LiFePO4 pack and BMS. **Why deferred:** requires spot-welding and a custom BMS, violating the "no custom assembly" constraint. Worth revisiting if the product reaches a volume where a certified pack is affordable.

### LoRa / Meshtastic module
ESP32-S3 + SX1262 for long-range mesh communication and remote command. **Why deferred:** adds a second radio, antenna, and software layer. Useful for field teams but not required for the core use case.

### Voice control
ESP32-S3 with microphone and speaker for local wake-word and voice commands. **Why deferred:** adds integration and testing time. Nice for field use, not needed for the first revision.

### NFC authentication
NFC reader for physical key access and hardware kill switch. **Why deferred:** legally and technically sensitive. Adds hardware and software complexity.

### GPS and offline maps
GPS module and a small touchscreen with offline OpenStreetMap. **Why deferred:** turns the device into a field tablet, increases cost and weight. Not a server feature.

### 4G/5G internal modem
M.2 modem with SIM slot instead of a USB dongle. **Why deferred:** requires antenna cutouts, SIM slot, and extra power. USB modem works today.

### Solar input
MPPT controller and folding solar panel feeding the X-UPS1 input. **Why deferred:** external accessory. Useful for long field deployments.

### Second external Ethernet port
For redundant uplink or a second Starlink terminal. **Why deferred:** one uplink is enough for v0.1.

### Managed switch with VLANs
Replace the unmanaged switch with a managed one for per-port isolation. **Why deferred:** routing on Board A provides isolation today. Managed switch adds cost and configuration complexity.

---

## Software

### No-code management panel
A visual interface for creating bots, deploying sites, and managing the knowledge base without touching Docker or SSH. **Why deferred:** this is a separate software product requiring a development team and a year of work. Not feasible alongside hardware development in 6 months.

### Spatial RAG
Geotagged document retrieval that uses GPS position to surface relevant documents automatically. **Why deferred:** requires GPS and a location-aware index. Interesting, but not core.

### Cluster failover between two devices
Two Mobile Server Books in a pair, with automatic failover if one dies. **Why deferred:** doubles cost. The single device already isolates faults between its two boards.

### Custom agent framework
A framework for building multi-step bots with tool use and memory. **Why deferred:** software product, not hardware feature. Standard Telegram/VK/Discord libraries are enough for v0.1.

### Local web dashboard
A simple status page served from Board A, showing uptime, temperature, disk usage, and bot status. **Why deferred:** nice to have, but SSH and OLED screens cover the basic need. Good candidate for v1.1.

---

## Manufacturing

### Injection-molded front panel
Replace the laser-cut ABS panel with an injection-molded part. **Why deferred:** requires tooling and a minimum order quantity. Only makes sense at scale.

### CNC-milled aluminium tub
Replace the sandwich with a milled tub. **Why deferred:** expensive, wasteful, and locks the design to a machine shop. The sandwich is better for small batches.

### Custom PCB for power distribution
Replace the X-UPS1 and wiring with a single custom board. **Why deferred:** requires PCB design, assembly, and testing. Not needed for v0.1.

### Certified battery pack
A UN38.3 and IEC 62133 certified pack for shipping and retail. **Why deferred:** certification is expensive and only matters at retail scale.

---

## Business

### Crowdfunding campaign
Kickstarter or Indiegogo for the first batch. **Why deferred:** needs a working prototype and a marketing budget. Month 6 decision.

### Corporate pre-orders
Sell directly to agencies and field teams before mass production. **Why deferred:** needs a demo unit and a sales pitch. Month 5–6 decision.

### Open-source hardware files
Release the DXF, STL, and BOM under an open license. **Why deferred:** business model decision, not an engineering one.

---

## Rules for this file

1. **Nothing here is promised.** This is a parking lot for ideas, not a roadmap.
2. **Every item has a reason for deferral.** If the reason disappears, the item can be promoted.
3. **The list is allowed to grow.** Cutting ideas is healthy; losing them is not.
