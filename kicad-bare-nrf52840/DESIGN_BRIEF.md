# b-parasite: bare nRF52840 prototype

Revision 3.0-prototype • 2026-10-21 • KiCad 10.0.6

**Routed engineering prototype; not a manufacturing release.** Open `parasite.kicad_pro` in this directory. The original module-based design remains in `../kicad`.

## Implemented

- Bare Nordic nRF52840-QIAA, aQFN-73, with external DC/DC, DEC/VDD capacitors, 32 MHz and 32.768 kHz crystals.
- Original capacitive soil measurement, SHTC3 temperature/humidity, ALS-PT19 light measurement, LED, user/reset buttons, battery monitoring, and upstream BLE sensor-broadcast interface.
- Four copper layers, corner Johanson 2450AT18B0100001E antenna and initial matching network.
- J1 six-contact Tag-Connect SWD programming footprint, plus the individual test points.
- Keystone 1060 CR2032 holder on the back, moved down to clear the MCU. Holder center: (68,65.5) mm; positive contact above the cell, negative below. Keep soil below the marked y=84 mm limit and check holder/cell seating on the prototype.
- No 0 Ω crossing jumpers were needed. Inner-layer routing and filled/capped escape vias completed the routing.

BLE broadcasts carry measurements. A receiver such as Home Assistant must apply the watering threshold and send the desired notification; the PCB alone does not send a phone push notification.

## Programming: J1

Top-side `Connector:Tag-Connect_TC2030-IDC-NL_2x03_P1.27mm_Vertical`, with the footprint's guide holes and pin-1 geometry.

| Pin | Signal | Use |
| --- | --- | --- |
| 1 | +3V0 | Target voltage reference (VTref) |
| 2 | SWDIO | Debug/programming data |
| 3 | RST | P0.18/nRESET |
| 4 | SWDCLK | Debug/programming clock |
| 5 | GND | Common ground |
| 6 | NC | SWO not implemented |

Use the TC2030-CTX-NL cable for a 10-pin Cortex debug header, or the appropriate TC2030-IDC-NL cable with ARM20-CTX adapter for a conventional 20-pin J-Link interface. Verify cable orientation against the manufacturer drawing. Power the target from its CR2032 or a controlled 3 V supply; VTref is the target's sense voltage, not a request for 5 V programmer power. Avoid connecting an external supply in parallel with an installed primary coin cell. The upstream nRF52840 devicetree already selects `gpio-as-nreset`.

## PCB construction and JLCPCB options

The completed routing uses **ordinary through-vias, with via-in-pad filling and copper capping**. It does not use blind, buried, or laser microvias; HDI is not needed to achieve connectivity in this revision.

| Parameter | Design / order intent |
| --- | --- |
| Copper layers | 4: F.Cu / In1.Cu / In2.Cu / B.Cu |
| Minimum track / copper clearance | 0.10 / 0.10 mm |
| Small escape via | 0.35 mm copper diameter / 0.15 mm hole |
| Typical routing via | 0.45 mm copper diameter / 0.20 mm hole |
| Via treatment | Epoxy Filled & Capped, especially all vias in SMT pads |
| Surface finish | ENIG for the fine-pitch MCU and exposed programming contacts |
| Copper | 35 µm per layer as currently modeled; confirm inner-layer copper in quote |
| Mask | Green suggested; 0 expansion, 0.10 mm minimum web, 0.09 mm opening-to-other-net copper clearance |
| Outline | Approximately 22 × 126.4 mm; retain the actual Edge.Cuts outline |
| Controlled impedance | RF feed targeted at 50 Ω; vendor stackup/solver approval required |

The KiCad physical stack is mask 0.025 / Cu 0.035 / FR4 0.075 / Cu 0.035 / FR4 1.2 / Cu 0.035 / FR4 0.075 / Cu 0.035 / mask 0.025 mm, totaling **1.54 mm**. Treat this as a nominal 1.6 mm-class design assumption, not a selected JLCPCB production stack. Changing the outer dielectric or copper requires recalculating the RF feed width and reviewing the matching layout.

JLCPCB's published capabilities list 0.15 mm through-vias, 0.10/0.10 mm trace/space and filled/capped via-in-pad processing. Its documents say resin filling is included for six layers and above; they do not guarantee this exact four-layer stack and via combination. Obtain DFM confirmation of four-layer epoxy fill/cap, drill aspect ratio, mask registration and stackup before ordering. If that combination is unavailable, a six-layer stack may be the simpler purchasing option; HDI is a separate alternative that requires a new fanout and stackup review.

For an eventual quote select the actual smallest 0.15 mm via-hole option and **Epoxy Filled & Capped**, not ordinary solder-mask plugging. Include an instruction to fill/cap all via holes up to 0.30 mm, which covers the signal escapes, EP grounding vias and the via in TP8. Do not fill Tag-Connect guide holes or mechanical holes. Confirm the manufacturer's production files before release. No order has been placed and no design files have been uploaded to JLCPCB.

Critical via-in-pad locations, in PCB coordinates (mm):

| Net | Center | Copper / hole |
| --- | --- | --- |
| BTN1 | 66.00,39.25 | 0.35 / 0.15 |
| LED | 66.50,39.25 | 0.35 / 0.15 |
| SENS_OUT | 67.00,39.25 | 0.35 / 0.15 |
| PWM | 64.25,41.50 | 0.35 / 0.15 |
| RST | 67.00,44.75 | 0.35 / 0.15 |
| EP ground | 65.80,42.30; 68.20,42.30; 65.80,43.20; 68.20,43.20 | 0.45 / 0.20 |
| TP8/P0.10 | 58.50,57.50 | 0.45 / 0.20 |

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

The upstream v2.0.0 devicetree also assigns P1.10 to a fast-discharge control, while the exported v2.0.0 schematic netlist marks module P1.10 unconnected. P1.10 remains unconnected as in the original v2.0.0 hardware; verify the inherited discharge behavior during sensor bring-up.

## Circuit and RF review notes

The power network follows Nordic QIAA VDD/DC-DC reference circuitry: VDDH is tied to VDD; DCC uses 10 µH then 15 nH into DEC4/DEC6; DEC1=100 nF, DEC3=100 pF, DEC5=820 pF, DEC4/6=1 µF plus 47 nF. Unused VBUS is grounded; DECUSB, DEC2 and DCCH are NC for this configuration. Review against Nordic QIAA reference revision 1.3 and the supplied PCA10056 DK 3.0.3 reference before assembly.

HF crystal: Golledge MP12308, 32 MHz, CL=8 pF, with 12 pF loads. LF crystal: NDK NX3215SA-32.768K-STD-MUA-9 with 12 pF loads. Final oscillator loading depends on PCB stray capacitance and must be checked on hardware.

RF starting values: MCU shunt 0.8 pF / series 4.7 nH / shunt 0.5 pF, followed by antenna tuning series 1.2 pF / shunt 2.7 nH / series 3.3 nH. These are prototype starting values, not a measured match. Use C0G capacitors and suitable high-Q RF inductors. Keep the antenna's all-layer corner keepout clear of enclosure metal, the battery and coating.

**Upstream BLE firmware requests +8 dBm** (`code/samples/ble/prj.conf`). This layout has not been qualified at that setting. Start RF bring-up at reduced output power; validate matching, harmonic emissions and range before enabling the intended operating power. Nordic reference placement guidance and an electrically clean DRC do not certify RF performance.

The probe's CPARA copper geometry is retained; inner copper planes and signal routes stop above the sensing region (y=61 mm). The new stack and relocated cell change capacitance. Re-measure dry/wet calibration coefficients and test the complete insertion-depth range. Protect electronics from moisture while leaving the humidity sensor's sensing aperture usable.

## Validation and remaining review

See `review/` for the native KiCad reports and route-preservation check. Final checks include ERC, DRC with schematic parity, net assignment consistency, absence of DNP flags on physical components, and preservation of the hand-routed RF/crystal/probe traces. The route comparison samples protected segments to 2 µm tolerance; it is a geometry regression check, not an RF simulation.

Remaining warnings are reported rather than suppressed: the intentionally open-ended CPARA sensing electrode; legacy/library footprint differences; two battery-holder outline silkscreen clips at the original mechanical holes; and the inherited SHTC3 symbol-library difference. The holder outline clips are cosmetic. Confirm physical holder seating, cell removal and guide-hole access in mechanical review. Reference/value fields are hidden on crowded silkscreen; use the BOM and CAD for assembly identification.

Before manufacturing release: approve the exact stack/fill/cap process with the fabricator; review aQFN exposed-pad paste and filled-via finish; reconcile retained legacy library differences; review solder-mask and assembly spacing; check the battery contact polarity and mechanics. Then assemble prototypes and verify SWD, both crystals, BLE, all sensors, button/reset operation, sleep current, radio current transients and battery life. Firmware compilation, flashing and bench/RF measurements have not been performed in this task.

`design/*.json` records construction inputs used during development; it is not a complete generator for the final routed PCB. The native `.kicad_sch`, `.kicad_pcb` and `.kicad_pro` files are authoritative. `prototype-bom.csv` is an engineering BOM; passive part procurement, supplier stock and assembly substitutions require review. Test pads and Tag-Connect pads are PCB features, not fitted connectors.

## References and licensing

- [Upstream b-parasite](https://github.com/rbaron/b-parasite) — derived hardware remains CC BY-SA 4.0; retain upstream attribution and license.
- [Nordic nRF52840 reference circuitry](https://docs.nordicsemi.com/r/bundle/ps_nrf52840/page/ref_circuitry.html) and [reference downloads](https://www.nordicsemi.com/products/nrf52840/compatible-downloads).
- [Johanson antenna datasheet](https://www.johansontechnology.com/docs/3770/Antenna-2450AT18B0100001E-Rev5.0.pdf).
- [Tag-Connect Cortex cable](https://www.tag-connect.com/product/tc2030-ctx-nl-6-pin-no-legs-cable-with-10-pin-micro-connector-for-cortex-processors) and [ARM20-CTX pinout](https://www.tag-connect.com/wp-content/uploads/bsk-pdf-manager/ARM20-CTX_19.pdf).
- [Keystone 1060](https://www.keyelco.com/product.cfm/product_id/726).
- [JLCPCB capabilities](https://jlcpcb.com/capabilities/pcb-capabilities), [via-covering choices](https://jlcpcb.com/help/article/pcb-via-covering) and [quote page](https://cart.jlcpcb.com/quote/), checked 2026-10-21.

The user-supplied Nordic reference archive is kept locally and is not included in the repository or review package.
