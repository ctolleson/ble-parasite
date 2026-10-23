# Raytac nRF52840 plant monitor — MODULE-B

Independent KiCad project using Raytac MDBT50Q-1MV2 with its integrated chip antenna. Open `parasite.kicad_pro` in KiCad 10. The prior bare-MCU design remains in its original branch.

## Interfaces

- **J1:** 2 × 5, 1.27 mm Cortex SWD header for a J-Link probe or compatible adapter. Pin 1: target +3V0/VTref; 2: SWDIO; 3: GND; 4: SWDCLK; 5: GND; 6: SWO (P1.00); 7: NC/key; 8: NC; 9: GND detect; 10: nRESET. Power the target from its battery; VTref is a voltage reference, not a probe power input. Check cable orientation against pin 1 before connecting. Header pin 7 is electrically unused; remove it if required by a keyed cable.
- **J2:** 1 × 4, 2.54 mm I2C expansion header. Pins 1–4: +3V0, GND, SDA/P0.24, SCL/P0.13. Shared with the SHTC3 and existing 10 kΩ pull-ups. Use short wiring and do not apply 5 V.
- **J3:** 1 × 2, 2.54 mm external NFC antenna header. Pin 1: NFC1/P0.09; pin 2: NFC2/P0.10. An external antenna and matching/tuning appropriate to its inductance are required. The module's chip antenna is for 2.4 GHz BLE, not NFC.

## Hardware

- CR2032 supply; module VDD and VDDH share +3V0, REG0 bypassed. DCCH is unused. VBUS is grounded and USB data pins are unused, following Raytac's no-USB application circuit.
- Module contains the 32 MHz crystal, RF matching and regulator inductors. External 32.768 kHz crystal retained.
- All seven ordinary host resistors, all six capacitors and D2 use standard imperial **0805** (2.0 × 1.25 mm nominal bodies). The Raytac module, sensors, crystal, diode, transistor, switches and connectors retain their required packages. Values are preserved; 0805 manufacturer part numbers and electrical ratings are recorded in the schematic and PCB fields. Inventory has not been rechecked for this revision.
- Rectangular **30.0 × 126.4 mm** board with straight, parallel vertical sides, replacing the earlier 49 mm-wide head. Components are above the soil limit; the CR2032 holder is on the back.
- Four copper layers; 0.15 mm minimum signal width, 0.125 mm clearance, ordinary through vias with 0.60 mm copper / 0.30 mm drill. No blind/buried vias or microvias.
- Antenna at the top board edge with an all-layer copper/tracks/vias keepout across the head. Keep metal, the battery, cables and enclosure hardware away from the antenna.
- Soil probe electrode and surrounding ground remain covered by solder mask.

## Validation status

KiCad 10 native ERC: **0 violations**. Native DRC: **0 violations, 0 unconnected items**. Independent comparison of the native schematic netlist and saved PCB: **133/133 pad nets match**, including all 40 intentional no-connect pads. Saved reports and previews are in `review/`.

MCP PCB quality gate passes. Other MCP heuristics flag native no-connect net names, wire-interior pin joins, opposing-side placement and module-center-to-cap distance; these are reviewed in `review/REVIEW.md`. They are not represented as blanket gate passes.

**Manufacturing status: needs engineering review; not a manufacturing release.** Select exact connector SKUs and confirm their land patterns/cable fit before procurement. J1 currently uses KiCad’s generic 2×5 1.27 mm SMT pin-header footprint. Check the assembly process for the module and underside battery holder. A new manufacturing package and quotation are needed for this revision. Firmware must use the new GPIO assignments and configure NFC/SWO as required. Verify soil calibration, sensor behavior, sleep current, crystal start-up, BLE range and NFC antenna tuning on hardware.

## References

- [Raytac module specification](https://www.raytac.com/tw/download/index.php?index_id=24)
- [Nordic nRF52840 pin assignments](https://docs.nordicsemi.com/r/bundle/ps_nrf52840/page/pin.html)
