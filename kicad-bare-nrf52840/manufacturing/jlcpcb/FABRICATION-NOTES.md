# Fabrication and assembly notes — draft

## Construction

| Item | Requirement |
|---|---|
| Layers | 4, ordered F.Cu / In1.Cu / In2.Cu / B.Cu |
| Material / order thickness | FR-4, nominal 1.6 mm |
| Intended stack | JLC04161H-1080; confirm with fabrication engineer |
| Copper | Outer 35 µm; inner 15.2 µm modeled |
| Outer dielectric | 1080 prepreg, 0.0764 mm each; Dk 3.91 modeled |
| Core | 1.265 mm; Dk 4.6 modeled |
| Mask model | 0.01524 mm each side; Dk 3.8 |
| Finish / mask | ENIG / green solder mask |
| Minimum track / clearance | 0.10 / 0.10 mm |
| Minimum via | 0.35 mm copper diameter / 0.15 mm drill; minimum annulus 0.10 mm |
| Via processing | All via holes ≤0.30 mm: epoxy filled, planarized, copper capped |
| Silkscreen | White, as supplied; small component references are intentionally hidden |
| RF | 50 Ω target; confirm outer dielectric/copper and adjacent ground geometry before CAM changes |

The modeled stack totals **1.54868 mm** including mask, matching the sum of the listed layer assumptions. The ordering class is nominal 1.6 mm. Do not silently change the outer prepreg to make the numerical total 1.6 mm. Loss tangent 0.02 is an unverified modeling assumption; obtain the actual material data from JLCPCB. [JLCPCB published stackups](https://jlcpcb.com/impedance).

The design uses conventional mechanically drilled through vias. JLCPCB publishes multilayer trace/space capability of 0.09/0.09 mm, via holes down to 0.15 mm, and epoxy-filled/capped via-in-pad processing. Confirm the combination on this four-layer board; the standard MCP profile’s 0.127 mm trace and 0.30 mm via limits do not describe these selected options. [JLCPCB capabilities](https://jlcpcb.com/capabilities/pcb-capabilities).

Select **Epoxy Filled & Capped**, not solder-mask plugging or ordinary tenting. [JLCPCB via processing](https://jlcpcb.com/help/article/pcb-via-covering).

## Drilling and routing

- PTH file: 73 holes — six 0.15 mm, fifty 0.20 mm, seventeen 0.30 mm. All are vias and should be filled/capped.
- NPTH file: three 0.9906 mm Tag-Connect guide holes, rounded to 0.991 mm in the drill report. Do not fill.
- Two circular mechanical openings are **routed Edge.Cuts features**, not drill-file omissions. Centers (61,68) and (75,68) mm; radius 1.41421356 mm.
- Outline centerlines span approximately 22 × 126.4142 mm. Gerber job bounding dimensions include the 0.05 mm outline stroke: 22.05 × 126.4642 mm. Follow supplied Edge.Cuts geometry.
- Do not add copper thieving, panel tabs, fiducials, tooling holes, order numbers, or other markings in the electrode or antenna regions without review. Vendor panelization must retain antenna clearance and sensing geometry.

Critical vias in SMT pads include (PCB coordinates, X right/Y down, mm):

| Net / region | Centers | Diameter / drill |
|---|---|---|
| BTN1, LED, SENS_OUT | (66,39.25), (66.5,39.25), (67,39.25) | 0.35 / 0.15 |
| PWM | (64.25,41.5) | 0.35 / 0.15 |
| RESET | (67,44.75) | 0.35 / 0.15 |
| U1 exposed-pad ground | (65.8,42.3), (68.2,42.3), (65.8,43.2), (68.2,43.2) | 0.45 / 0.20 |

The sixth 0.15 mm via at (70.8,41.3) is outside its MCU pad. The file specifies filling/capping all vias, so no pad-overlap classification is required by CAM. TP8 at (58.5,57.5) has no drill in the final board.

## Assembly and coordinate conventions

BOM uses exact MPN in Comment and supplier code in “LCSC Part #”. Y2 is KYOCERA AVX CX2016DB32000D0FLJCC, DigiKey 478-CX2016DB32000D0FLJCCCT-ND. Confirm JLCPCB procurement/consignment; no LCSC code has been assumed. Retain C14/C15 at 12 pF and verify oscillator startup/frequency on the prototype. The former Y1 code C32346 was wrong (12.5 pF load); **C519280** matches the intended NDK 9 pF part. [NDK crystal listing](https://www.lcsc.com/product-detail/C519280.html).

BOM and CPL have the same 43 references. Both sides are combined; BT1 is Bottom. Coordinates are in mm from the **absolute KiCad page origin**, matching Gerber and drill exports. X is rightward; Y is upward in exports, hence negative values for this board. Do not independently translate, mirror, or negate only one file. CPL rotations are native KiCad angles modulo 360; positive is counterclockwise. Check selected vendor library orientations before release. [JLCPCB CPL requirements](https://jlcpcb.com/help/article/pick-place-file-for-pcb-assembly) and [BOM requirements](https://jlcpcb.com/help/article/bill-of-materials-for-pcb-assembly).

BT1 center is (68,-65.5) mm in CPL, rotation 270°. Positive pad 1 is (68,-50.85); negative pad 2 is (68,-80.15). Do not install the CR2032 during reflow. Confirm holder/cell access around the two mechanical openings. Stencil review must include the aQFN exposed pad, the filled via surfaces, and the SHTC3 sensor opening. Do not wash/coat the sensor aperture without following Sensirion’s handling guidance.

## First article

Inspect aQFN soldering, test shorts with the cell removed, use a controlled current-limited supply, program over SWD, verify both crystals and all sensors, then characterize sleep/transmit current and RF matching. The upstream firmware requests +8 dBm; this layout is not qualified at that power. Recalibrate soil readings for the new stack and battery position. These tests remain unperformed.
