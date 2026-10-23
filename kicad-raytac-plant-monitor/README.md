# Raytac nRF52840 plant monitor — MODULE-A

Independent KiCad project using Raytac MDBT50Q-1MV2 with its integrated chip antenna. Open `parasite.kicad_pro` in KiCad 10. The prior bare-MCU design remains in its original branch.

## Interfaces

- **J1:** 2 × 5, 1.27 mm Cortex SWD header for a J-Link probe or compatible adapter. Pin 1: target +3V0/VTref; 2: SWDIO; 3: GND; 4: SWDCLK; 5: GND; 6: SWO (P1.00); 7: NC/key; 8: NC; 9: GND detect; 10: nRESET. Power the target from its battery; VTref is a voltage reference, not a probe power input. Check cable orientation against pin 1 before connecting. Header pin 7 is electrically unused; remove it if required by a keyed cable.
- **J2:** 1 × 4, 2.54 mm I2C expansion header. Pins 1–4: +3V0, GND, SDA/P0.24, SCL/P0.13. Shared with the SHTC3 and existing 10 kΩ pull-ups. Use short wiring and do not apply 5 V.
- **J3:** 1 × 2, 2.54 mm external NFC antenna header. Pin 1: NFC1/P0.09; pin 2: NFC2/P0.10. An external antenna and matching/tuning appropriate to its inductance are required. The module's chip antenna is for 2.4 GHz BLE, not NFC.

## Hardware

- CR2032 supply; module VDD and VDDH share +3V0, REG0 bypassed. DCCH is unused. VBUS is grounded and USB data pins are unused, following Raytac's no-USB application circuit.
- Module contains the 32 MHz crystal, RF matching and regulator inductors. External 32.768 kHz crystal retained.
- Ordinary host resistors and capacitors use 1210 packages, exceeding imperial 1008. Larger PLCC-2 LED retained. Specialized sensor, crystal, diode, transistor and switch packages are exceptions to the ordinary-passive rule.
- Four copper layers; 0.15 mm minimum signal width, 0.125 mm clearance, ordinary through vias with 0.60 mm copper / 0.30 mm drill. No blind/buried vias or microvias.
- Antenna at the top board edge with an all-layer copper/tracks/vias keepout across the head. Keep metal, the battery, cables and enclosure hardware away from the antenna.
- Soil probe electrode and surrounding ground remain covered by solder mask.

## Validation status

Schematic conversion: ERC reports zero violations. PCB routing and final validation are in progress; this commit is not a manufacturing release. A new manufacturing package and quotation are needed for this revision. Firmware must enable NFC pin use and configure SWO as required; verify sensor behavior, sleep current, crystal start-up, BLE range and NFC tuning on hardware.

## References

- [Raytac module specification](https://www.raytac.com/tw/download/index.php?index_id=24)
- [Nordic nRF52840 pin assignments](https://docs.nordicsemi.com/r/bundle/ps_nrf52840/page/pin.html)
