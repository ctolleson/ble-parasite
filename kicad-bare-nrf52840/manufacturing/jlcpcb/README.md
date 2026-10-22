# b-parasite bare nRF52840 — JLCPCB draft package

Revision **3.0-prototype**, 2026-10-21. KiCad 10.0.6.

**Status: draft package. Fabrication/assembly release remains blocked pending the checks below.** No order or upload has been made.

## Files to use

- `JLCPCB-Gerber-Drill.zip`: four copper layers, two masks, two silkscreens, two paste layers, outline, Gerber job metadata, and separate PTH/NPTH Excellon drills. Upload only after fabrication review.
- `assembly/JLCPCB-BOM.csv`: 43 fitted components; exact MPN in Comment, reference, footprint, supplier code, and quantity.
- `assembly/JLCPCB-CPL.csv`: matching 43 references, 42 Top / 1 Bottom. The bottom part is BT1.
- `assembly/engineering-bom.csv`: full native schematic export, including PCB-only contacts.
- `assembly/sourcing-review.csv`: catalog links and sourcing status.
- `assembly/polarity-pad-map.csv`: pad/net coordinates for ICs, polarized parts, crystals, antenna, and switches.
- `review/`: native ERC/DRC/parity reports, file audit, drill report/maps, schematic PDF, assembly PDFs, PCB previews, raw MCP gate results, and geometry regression evidence.
- `source/`: final KiCad project and project libraries. Open `parasite.kicad_pro`.
- `SHA256SUMS.txt` and `manifest.json`: package integrity and source provenance.

## Y2 update

Y2 now uses KYOCERA AVX CX2016DB32000D0FLJCC. Schematic and PCB fields, construction metadata, BOMs, sourcing notes, and exports were refreshed. `review/y2-change-audit.json` confirms unchanged board geometry and connectivity. Native schematic save normalized ordering and whitespace and added one hidden standard label field.

## Validation

Native KiCad: **0 ERC violations, 0 DRC violations, 0 airwires, 0 schematic parity issues** under the project’s configured checks. BOM/CPL reference sets and all exported positions/sides/rotations were checked against the PCB. No physical component is DNP. Gerber layer identities and drill totals were checked.

The remaining original warnings were fixed: library footprint mismatches, switch/phototransistor SMD attributes, battery-holder silkscreen crossing routed holes, the intentionally open soil-electrode track endpoint, and the SHTC3 library definition. TP9 is a copper-only endpoint inside the existing electrode; it adds no mask opening, paste, or fitted part. A test-point wire now terminates on its pin. RF, crystal, and probe tracks were preserved.

No DRC exclusions or rule-severity changes were added. The reports list inherited ignored checks: missing courtyard, track centering on vias, tuning geometry, footprint filters, and PTH/NPTH-in-courtyard checks; ERC ignores single global labels, four-way junctions, and footprint filters. A clean result is scoped to these settings.

## Release holds

1. **Y2 procurement:** KYOCERA AVX CX2016DB32000D0FLJCC, 32 MHz, CL 8 pF. DigiKey cut tape **478-CX2016DB32000D0FLJCCCT-ND**. The replacement matches the existing footprint; C14/C15 remain 12 pF. Its LCSC field is intentionally blank; confirm JLCPCB procurement or consignment before assembly. See `review/y2-replacement.md` for specifications and prototype checks.
2. **Fabrication acceptance:** obtain JLCPCB confirmation of four-layer epoxy-filled/copper-capped 0.15 mm through vias and the specified stack. Confirm RF geometry with its impedance solver. This is not a generic low-cost four-layer order.
3. **Assembly review:** check JLCPCB’s component preview against pad numbering and the assembly drawings, especially U1, U2, Q1, Q4, D1, D2, Y2, AE1, and bottom-side BT1. Native KiCad angles are normalized to 0–360°; vendor-specific rotation offsets have not been assumed. Confirm the 43 parts are available for the chosen assembly service. Catalog presence is not reserved stock.
4. **Prototype qualification:** verify battery mechanics/polarity, exposed-pad stencil coverage and reflow, SWD, oscillators, sensors, BLE, current consumption, and RF matching/emissions. Firmware and bench tests have not been performed.

The MCP project/manufacturing gates report FAIL. Their raw results are retained, with the discrepancies explained in `review/GATE-RECONCILIATION.md`. No automated gate was overridden or represented as passing. The fabrication-output workflow requires unresolved manufacturing and assembly holds to block release, so these are review files, not production-approved files.

## Ordering intent

See `FABRICATION-NOTES.md`. **No HDI, blind vias, buried vias, or laser microvias are used.** All 73 vias are through vias. Filled and copper-capped via-in-pad processing is required. Keep the three Tag-Connect guide holes and two routed mechanical openings unfilled.

J1 is a six-contact Tag-Connect SWD feature, not a fitted connector: 1 VTref/+3V0, 2 SWDIO, 3 RESET, 4 SWDCLK, 5 GND, 6 NC. J1 and TP1–TP9 are excluded from the JLC assembly files; board artwork is also excluded. C16 is populated in this prototype variant.

## Attribution

Derived from [rbaron/b-parasite](https://github.com/rbaron/b-parasite). Hardware remains [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). Retain upstream attribution. The supplied Nordic archive is not redistributed in this package.
