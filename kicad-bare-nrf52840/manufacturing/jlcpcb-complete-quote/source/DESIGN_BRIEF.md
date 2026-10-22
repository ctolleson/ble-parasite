# b-parasite: bare nRF52840 prototype

Revision 3.0-prototype • 2026-10-21 • KiCad 10.0.6

**Routed engineering prototype; JLCPCB draft files are in `manufacturing/jlcpcb/`. Release holds are listed in that package’s README.** Open `parasite.kicad_pro` in this directory. The original module-based design remains in `../kicad`.

## Implemented

- Bare Nordic nRF52840-QIAA, aQFN-73, with external DC/DC, DEC/VDD capacitors, 32 MHz and 32.768 kHz crystals.
- Original capacitive soil measurement, SHTC3 temperature/humidity, ALS-PT19 light measurement, LED, user/reset buttons, battery monitoring, and upstream BLE sensor-broadcast interface.
- Four copper layers, corner Johanson 2450AT18A0100001E antenna and initial matching network.
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
| Copper | Outer 35 µm / inner 15.2 µm; JLC04161H-1080 stack intent |
| Mask | Green suggested; 0 expansion, 0.10 mm minimum web, 0.09 mm opening-to-other-net copper clearance |
| Outline | Approximately 22 × 126.4 mm; retain the actual Edge.Cuts outline |
| Controlled impedance | RF feed targeted at 50 Ω; vendor stackup/solver approval required |

The KiCad physical stack now models JLC04161H-1080: mask 0.01524 / Cu 0.035 / 1080 prepreg 0.0764 / Cu 0.0152 / FR4 core 1.265 / Cu 0.0152 / 1080 prepreg 0.0764 / Cu 0.035 / mask 0.01524 mm, totaling **1.54868 mm**. Order nominal 1.6 mm and confirm the listed stack with JLCPCB. Outer dielectric Dk=3.91, core Dk=4.6; loss tangent remains an unverified 0.02 model assumption. ENIG is set in the PCB. Changing the outer dielectric/copper requires reviewing RF impedance.

JLCPCB's published capabilities list 0.15 mm through-vias, 0.10/0.10 mm trace/space and filled/capped via-in-pad processing. Its documents say resin filling is included for six layers and above; they do not guarantee this exact four-layer stack and via combination. Obtain DFM confirmation of four-layer epoxy fill/cap, drill aspect ratio, mask registration and stackup before ordering. If that combination is unavailable, a six-layer stack may be the simpler purchasing option; HDI is a separate alternative that requires a new fanout and stackup review.

For an eventual quote select the actual smallest 0.15 mm via-hole option and **Epoxy Filled & Capped**, not ordinary solder-mask plugging. Include an instruction to fill/cap all via holes up to 0.30 mm, which covers the signal escapes and EP grounding vias. Do not fill Tag-Connect guide holes or mechanical holes. Confirm the manufacturer's production files before release. No order has been placed and no design files have been uploaded to JLCPCB.

Critical via-in-pad locations, in PCB coordinates (mm):

| Net | Center | Copper / hole |
| --- | --- | --- |
| BTN1 | 66.00,39.25 | 0.35 / 0.15 |
| LED | 66.50,39.25 | 0.35 / 0.15 |
| SENS_OUT | 67.00,39.25 | 0.35 / 0.15 |
| PWM | 64.25,41.50 | 0.35 / 0.15 |
| RST | 67.00,44.75 | 0.35 / 0.15 |
| EP ground | 65.80,42.30; 68.20,42.30; 65.80,43.20; 68.20,43.20 | 0.45 / 0.20 |

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

HF crystal: KYOCERA AVX CX2016DB32000D0FLJCC, 32 MHz, CL=8 pF, with 12 pF loads. DigiKey cut tape: 478-CX2016DB32000D0FLJCCCT-ND. ESR 60 ohm maximum; initial tolerance +/-10 ppm, temperature stability +/-15 ppm over -30 to +85 C. LF crystal: NDK NX3215SA-32.768K-STD-MUA-9 with 12 pF loads. Final oscillator loading depends on PCB stray capacitance and must be checked on hardware.

RF starting values: MCU shunt 0.8 pF / series 4.7 nH / shunt 0.5 pF, followed by antenna tuning series 1.0 pF / shunt 2.7 nH / series 3.9 nH. These are prototype starting values, not a measured match. Use C0G capacitors and suitable high-Q RF inductors. Keep the antenna's all-layer corner keepout clear of enclosure metal, the battery and coating.

**Upstream BLE firmware requests +8 dBm** (`code/samples/ble/prj.conf`). This layout has not been qualified at that setting. Start RF bring-up at reduced output power; validate matching, harmonic emissions and range before enabling the intended operating power. Nordic reference placement guidance and an electrically clean DRC do not certify RF performance.

The probe's CPARA copper geometry is retained; inner copper planes and signal routes stop above the sensing region (y=61 mm). The new stack and relocated cell change capacitance. Re-measure dry/wet calibration coefficients and test the complete insertion-depth range. Protect electronics from moisture while leaving the humidity sensor's sensing aperture usable.

## Validation and remaining review

See `review/` for the native KiCad reports and route-preservation check. Final checks include ERC, DRC with schematic parity, net assignment consistency, absence of DNP flags on physical components, and preservation of the hand-routed RF/crystal/probe traces. The route comparison samples protected segments to 2 µm tolerance; it is a geometry regression check, not an RF simulation.

All original enabled-rule warnings are resolved: **ERC 0, DRC 0, airwires 0, schematic parity 0**. Library footprints and SHTC3 were refreshed; project SMD attributes corrected; the battery-holder body outline moved to Fab; and a covered copper-only TP9 endpoint terminates the soil electrode within its existing copper. No warning was waived. See the package README for inherited ignored-rule scope. Native validation and final package checks are in `manufacturing/jlcpcb/review/`. Confirm physical holder seating and cell removal in mechanical review.

Before manufacturing release: approve the exact stack/fill/cap process with the fabricator; review aQFN exposed-pad paste and filled-via finish; review solder-mask and assembly spacing; check the battery contact polarity and mechanics. Then assemble prototypes and verify SWD, both crystals, BLE, all sensors, button/reset operation, sleep current, radio current transients and battery life. Firmware compilation, flashing and bench/RF measurements have not been performed in this task.

`design/*.json` records construction inputs used during development; it is not a complete generator for the final routed PCB. The native `.kicad_sch`, `.kicad_pcb` and `.kicad_pro` files are authoritative. The latest `manufacturing/jlcpcb-stocked-parts/assembly/JLCPCB-BOM.csv` and `JLCPCB-CPL.csv` contain the same 43 fitted references. Exact MPNs are recorded; LCSC mappings for substituted parts are blank pending supplier confirmation; Y2 uses KYOCERA AVX CX2016DB32000D0FLJCC with DigiKey ordering code 478-CX2016DB32000D0FLJCCCT-ND. Its LCSC field remains blank pending confirmed JLCPCB procurement or consignment. Y1 now correctly uses C519280 (9 pF), not C32346 (12.5 pF). Supplier availability and assembly orientations require review. Test pads and Tag-Connect pads are PCB features, not fitted connectors.

## References and licensing

- [Upstream b-parasite](https://github.com/rbaron/b-parasite) — derived hardware remains CC BY-SA 4.0; retain upstream attribution and license.
- [Nordic nRF52840 reference circuitry](https://docs.nordicsemi.com/r/bundle/ps_nrf52840/page/ref_circuitry.html) and [reference downloads](https://www.nordicsemi.com/products/nrf52840/compatible-downloads).
- [Johanson antenna datasheet](https://www.johansontechnology.com/docs/3827/Antenna-2450AT18A0100001E-Rev4.0.pdf).
- [Tag-Connect Cortex cable](https://www.tag-connect.com/product/tc2030-ctx-nl-6-pin-no-legs-cable-with-10-pin-micro-connector-for-cortex-processors) and [ARM20-CTX pinout](https://www.tag-connect.com/wp-content/uploads/bsk-pdf-manager/ARM20-CTX_19.pdf).
- [Keystone 1060](https://www.keyelco.com/product.cfm/product_id/726).
- [JLCPCB capabilities](https://jlcpcb.com/capabilities/pcb-capabilities), [via-covering choices](https://jlcpcb.com/help/article/pcb-via-covering) and [quote page](https://cart.jlcpcb.com/quote/), checked 2026-10-21.

The user-supplied Nordic reference archive is kept locally and is not included in the repository or review package.


## Stocked-parts prototype revision — 2026-10-21

The current source uses the DigiKey-selected parts listed in `procurement/digikey-2026-10-21/`. Earlier manufacturing files under `manufacturing/jlcpcb/` and the earlier JLCPCB matching submission describe a superseded revision. The new draft is `manufacturing/jlcpcb-stocked-parts/`; do not mix its BOM, CPL, or Gerbers with the earlier revision.

- U2 remains SHTC3. Its exact purchasing code is SHTC3-TR-2.5KS, DigiKey 1649-SHTC3-TR-2.5KSCT-ND. No sensor, pinout, or firmware change.
- C6 is Murata GRM188R61C475KE11J, 4.7 uF ±10%, 16 V X5R, in a new 0603 footprint at (61.4,45.4), -90 degrees. Its VDD connection was rerouted. Typical manufacturer DC-bias graph is about 3.5 uF at 3 V, not a guaranteed minimum; validate supply droop on prototypes.
- AE1 uses the stocked Johanson A antenna. C19=1 pF and L5=3.9 nH are evaluation-board starting values. The 6.5 mm corner copper clearance is enforced with an all-copper-layer keepout. The different PCB geometry requires VNA tuning and BLE range testing with the battery and enclosure installed.
- Other substitutions preserve package, pinout, value and required ratings as detailed in the procurement review. Stale LCSC codes were cleared on substituted parts; DigiKey availability does not establish JLCPCB procurement or consignment acceptance.
- Soil copper remains covered by solder mask. SWD, bare nRF52840, four-layer stackup, and battery location are preserved.
- Native ERC and DRC with schematic parity: zero violations, zero unconnected items, zero parity issues, using the existing rule configuration. MCP transfer and generic standard-manufacturing gates retain documented limitations. Supplier approval of 0.10 mm traces, 0.15 mm drilled vias, fill/cap, assembly orientation, sensor handling and part procurement remains necessary.
- Prototype-only status: no hardware qualification or final assembled-price approval is implied. Parts cost for five boards is USD 90.69, excluding fabrication, assembly, shipping, taxes and spares.
