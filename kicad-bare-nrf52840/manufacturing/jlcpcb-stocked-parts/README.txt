Stocked-parts prototype revision, 2026-10-21
DRAFT PROTOTYPE — RF matching and vendor manufacturing/assembly approval required
Source commit: dd0f74427e0569133ebbe4d2bb6318abe8c2e551
Branch: feature-nrf52840-bare-mcu-plant-monitor

Use this complete set together. It supersedes the previous JLCPCB submission; nothing has been uploaded or ordered in this sourcing task.

Changes: stocked Johanson 2450AT18A0100001E antenna, C19=1pF, L5=3.9nH, corrected corner copper keepout; Murata GRM188R61C475KE11J 0603 bulk capacitor C6 with rerouted VDD. SHTC3-TR-2.5KS is the exact existing sensor's purchasing code. Other BOM equivalents are recorded in the DigiKey procurement workbook.

Validation: native ERC 0; DRC 0; unconnected 0; schematic parity 0. Existing ignored checks remain listed in the reports. All pad nets, schematic wires/labels, four-layer stackup and soil mask are preserved; C6 is the only moved component. 43 fitted references match BOM and CPL. Board-only SWD/test contacts are excluded from purchased parts.

Manufacturing gates: MCP PCB quality PASS. Transfer checker retains 44 intentional no-connect naming mismatches; native parity is clean. Generic JLCPCB standard DFM gate FAIL for 0.10mm traces and 0.15mm drills versus its 0.127/0.30mm limits; vendor approval of the selected fine-feature process, filled/capped vias and 1080 stackup remains necessary. Tool's five-layer count is a parser issue; board and Gerbers have four copper layers. No new HDI requirement is established by this substitution.

Before ordering:
1. Confirm JLCPCB can source/accept all exact DigiKey parts; empty LCSC entries need matching or consignment. DigiKey stock is not reserved and does not establish JLCPCB availability.
2. Confirm fabrication, via fill/cap, stackup/impedance, component polarity, pick-and-place rotations, stencil and sensor handling with supplier. CPL uses native KiCad rotation normalized to 0–360; no unverified supplier rotation offsets are applied.
3. Approve the antenna network as a prototype. Values are evaluation-board starting values, not a measured match. Tune S11 across BLE band on assembled boards with battery/enclosure; verify BLE range. Check oscillator startup/frequency, supply droop and soil calibration.
4. Obtain final all-in quote for five PCBA within $500. DigiKey parts estimate $90.69 excludes board fabrication, assembly, shipping, taxes, attrition/spares, CR2032 cells and programmer.

The existing v3 board marking is retained. Distinguish this revision by source commit and package hashes.
