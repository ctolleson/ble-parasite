# Bare nRF52840 b-parasite revision — design brief

**Status: design specification only.** `parasite.kicad_sch` and `parasite.kicad_pcb` in this directory are an unchanged copy of the upstream module-based hardware. They are not a bare-MCU implementation and must not be fabricated as such.

## Goal and baseline

Replace the Ebyte E73-2G4M08S1C nRF52840 module with the nRF52840-QIAA bare IC in its Nordic aQFN-73 package, while retaining the b-parasite v2.0.0 functions: capacitive soil moisture, SHTC3 temperature/humidity, ALS-PT19 light, CR2032 supply, 32.768 kHz clock, user/reset buttons, LED, SWD programming, and BLE sensor broadcasts. Preserve the original firmware GPIO assignments where possible.

The nRF52840 symbol and `Package_DFN_QFN:Nordic_AQFN-73-1EP_7x7mm_P0.5mm` footprint are present in the installed KiCad 10 library. The original board is two copper layers. Its module footprint, tracks, antenna keepout, and RF performance cannot be reused directly for the bare IC.

## GPIO and net mapping from upstream schematic/netlist

| Function | Upstream net | Bare MCU pin/function |
| --- | --- | --- |
| Soil excitation | PWM | P0.05 |
| Soil measurement | SENS_OUT | P0.03/AIN1 |
| Light measurement | PHOTO_OUT | P0.02/AIN0 |
| Light sensor supply | PHOTO_V | P0.29 |
| SHTC3 clock | SCL | P0.13 |
| SHTC3 data | SDA | P0.24 |
| LED | LED | P0.28 |
| User button | BTN1 | P0.30 |
| Reset | RST | P0.18/RESET |
| Low-frequency crystal | Y1/C3/C4 | P0.00/XL1, P0.01/XL2 |
| Programming | SWDIO, SWDCLK | SWDIO, SWDCLK |
| Existing test pad | 0.10 | P0.10/NFC2 |

The upstream v2.0.0 devicetree also assigns P1.10 to a fast-discharge control, while the exported v2.0.0 schematic netlist marks module P1.10 unconnected. Resolve that discrepancy against the firmware and actual assembled board before claiming behavioral equivalence.

## New bare-IC circuits required

1. Select the **QIAA VDD-supply configuration with DC/DC enabled** from Nordic's nRF52840 reference circuitry. The upstream firmware enables the internal DC/DC regulator (`&reg1` in `bparasite_nrf52840.dts`). Copy the applicable Nordic power network, every mandatory DEC capacitor, and DC/DC inductor with the specified values and placement. The E73 module previously contained this support circuitry.
2. Add the 32 MHz high-frequency crystal and its load capacitors using Nordic's QIAA reference values and layout. Retain the existing 32.768 kHz crystal only after checking its specified load capacitance against the selected crystal and PCB stray capacitance.
3. Add the Nordic QIAA RF matching network at the ANT output, followed by a selected 2.4 GHz antenna. Copy the matching section's topology and initial values from the **same reference-layout revision**. Keep footprints for tuning parts, and validate the matching and radiated performance on the assembled board. The E73 module's antenna is not part of the bare IC.
4. Carry VDD, VDDH, all ground/thermal pads, DEC, DCCH, VBUS, USB, and NFC pins exactly as specified by the selected supply and feature configuration. Mark genuinely unused interfaces appropriately; do not leave power or RF pins floating by analogy with the module schematic.
5. Keep SWDIO, SWDCLK, reset, VDD, and GND accessible on labeled programming pads. Maintain a reliable CR2032 contact and validate radio-current transients at end-of-life battery voltage.
6. Preserve the moisture probe electrode geometry and its copper/ground clearance. If changing the layer stack, exclude inner copper planes from the sensing region and recalibrate dry/wet coefficients in firmware.

## Board layout requirements

- Place the bare MCU, its decoupling, both crystals, DC/DC network, RF match, and antenna first, following the Nordic QIAA reference placement and routing as closely as practical.
- Keep the RF trace short and controlled; maintain the antenna's specified copper and component keepout on every layer.
- Give the MCU a continuous ground return and stitched ground near the RF network, while keeping ground out of the soil electrode sensing region.
- Keep sensor analog signals away from the RF feed, 32 MHz clock, and soil excitation trace.
- Do not reuse the existing E73 footprint or its routed nets on a fabrication candidate.

## Verification required before fabrication

- Update the schematic and PCB together; compare their netlists and confirm there is no E73 module footprint or symbol in the new revision.
- Run KiCad ERC and DRC with no unexplained errors, including schematic/PCB parity and antenna keepout checks.
- Review the MCU pin-to-pad mapping, Nordic reference circuit revision, antenna datasheet land pattern, crystal loading, thermal pad paste/vias, coin-cell discharge performance, and moisture-probe calibration with a hardware engineer.
- Assemble a prototype, verify SWD programming and BLE advertising, measure sleep/radio current and RF range, and exercise all sensors and buttons.

## Source documents

- Upstream b-parasite: https://github.com/rbaron/b-parasite
- Nordic nRF52840 reference circuitry: https://docs.nordicsemi.com/r/bundle/ps_nrf52840/page/ref_circuitry.html
- Nordic QIAA reference-layout downloads: https://www.nordicsemi.com/products/nrf52840/compatible-downloads
- Nordic nRF52840 power specification: https://docs.nordicsemi.com/r/bundle/ps_nrf52840/page/power.html

The upstream hardware files are CC BY-SA 4.0; retain the required attribution and license for any derived hardware design.
