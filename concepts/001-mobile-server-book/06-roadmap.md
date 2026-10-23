# Roadmap

**Six months from concept to a working prototype, with a check after every stage.**

---

## Principle

Every stage ends with a **measurable result**, not a feeling of progress.
If a stage fails its check, the next stage does not start until the issue is resolved.

---

## Month 1 — Bench prototype (no enclosure)

| Task | Deliverable |
|---|---|
| Order 2× Orange Pi 5 Plus, 2× NVMe SSD, X-UPS1 modules, 8× 18650 cells | Parts on the bench |
| Flash Armbian or Ubuntu for RK3588 on both boards | Both boards boot |
| Mount NVMe, run `fio` sequential and random tests | Baseline disk performance numbers |
| Install Docker, Nginx, PostgreSQL on Board A | Service stack runs |
| Install Ollama and a 1.5B model on Board B | Local inference works |
| Measure idle and load power with a USB power meter | Power numbers for `04-spec/power-budget.md` |
| Measure SoC temperature under load with `sensors` | Thermal baseline |

**Check:** both boards run their services for 48 hours without reboot. Power and temperature numbers are recorded and replace the estimates in the spec.

---

## Month 2 — Thermal mockup and enclosure design

| Task | Deliverable |
|---|---|
| Design the 3D-printed frame in FreeCAD or Fusion 360 | STL file for printing |
| Design the laser-cut parts (front ABS, front acrylic, copper discs, back aluminium) | DXF files for cutting |
| Print the frame in PETG | Physical frame |
| Cut copper and aluminium parts at a local laser service | Physical thermal stack |
| Assemble the boards into the frame with thermal paste | Stack runs outside the case |
| Measure temperature with the frame and copper discs installed | Thermal improvement data |
| Run a 4-hour sustained load test | Throttling behavior documented |

**Check:** the stack runs a 4-hour load without thermal shutdown. If it throttles, record the time-to-throttle and the steady-state temperature.

---

## Month 3 — Full enclosure and power integration

| Task | Deliverable |
|---|---|
| Assemble the full sandwich: front, frame, boards, copper, back | Complete device |
| Wire X-UPS1 modules to both boards and the switch | Battery-powered operation |
| Install OLED displays and ribbon cables | Status screens working |
| Install Wi-Fi module and antennas | Local access point works |
| Run the device on battery until shutdown | Real runtime numbers |
| Check low-battery GPIO behavior | Graceful shutdown script works |

**Check:** the device runs from battery for at least 4 hours under typical load. Low-battery shutdown happens without data corruption.

---

## Month 4 — Software stack and remote access

| Task | Deliverable |
|---|---|
| Deploy Nginx, PostgreSQL, and Docker bots on Board A | Services reachable from LAN |
| Deploy Nextcloud or Seafile on Board B | File server reachable from Board A only |
| Configure WireGuard on Board A | Remote access works |
| Configure iptables/nftables to block Board B from the internet | Isolation verified |
| Write systemd units for graceful shutdown | Services stop cleanly |
| Write a simple OLED status script | Screens show IP, temperature, status |

**Check:** from an external network, only WireGuard is reachable. Board B is not reachable from the internet. A full power loss during operation does not corrupt the database.

---

## Month 5 — Field test and feedback

| Task | Deliverable |
|---|---|
| Ship the prototype to 2–3 friendly users (agency, freelancer, field operator) | Real-world usage data |
| Collect logs: uptime, temperature, power draw, failures | Test report |
| Identify the top three problems from real use | Prioritized fix list |
| Fix the top three problems | Updated prototype |
| Measure assembly time with a stopwatch | Confirms the 15–20 minute estimate |

**Check:** at least two users run the device for two weeks without a critical failure. The fix list is shorter than the problem list.

---

## Month 6 — Finalization and pilot batch preparation

| Task | Deliverable |
|---|---|
| Finalize the BOM with actual supplier prices | Updated `04-spec/bom.md` |
| Finalize DXF and STL files for production | Manufacturing files |
| Write assembly instructions with photos | Assembly guide |
| Build 3–5 units for a pilot batch | Pilot units |
| Prepare a simple web page or PDF for potential buyers | Sales material |
| Decide: crowdfunding, pre-orders, or direct sales | Go-to-market decision |

**Check:** three pilot units are built and tested. The assembly guide is complete enough for a new person to build a unit without help.

---

## Risks and mitigations

| Risk | Likelihood | Mitigation |
|---|---|---|
| Orange Pi software is less mature than Raspberry Pi | Medium | Use Armbian, test early in Month 1, keep a Raspberry Pi 5 as fallback for Board A |
| Thermal throttling under sustained load | High | Document it honestly, design the optional thermal booster, limit LLM size |
| X-UPS1 charging is slow | Medium | Accept 4–5 hour charge, plan overnight charging, document it |
| Laser cutting service delays | Medium | Order parts in Month 1, keep a hand-cut cardboard mockup as backup |
| Wi-Fi antenna placement inside aluminium case | Medium | Test in Month 3, move antennas to top edge if needed |
| User expectations exceed the device | High | The README and concept passport state "not an AI cluster" explicitly |

---

## What success looks like

A device that:

- runs websites, bots, and file storage for a small team,
- survives a power outage without data loss,
- runs for 4–5 hours on battery,
- is silent,
- can be assembled in a local workshop from off-the-shelf parts,
- costs under $800 in parts and sells for $1,200–1,400.

That is the bar. Everything else is a future revision.
