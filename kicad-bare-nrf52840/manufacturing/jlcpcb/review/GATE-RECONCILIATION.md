# Validation scope and gate discrepancies

## Native results

The authoritative native KiCad reports in this folder have zero enabled-rule ERC/DRC violations, zero airwires, and zero schematic parity issues. No new exclusions or severity suppressions were introduced. Existing ignored checks are explicitly listed in the JSON reports and package README.

## MCP results retained as FAIL

- **Layer count:** the MCP profile reports five copper layers; native stackup, Gerber job, drill report, and four copper Gerbers establish four. This is a parser/counting discrepancy.
- **Manufacturer limits:** MCP’s “JLCPCB standard” profile demands 0.127 mm traces and 0.30 mm drills. Actual geometry is 0.10 mm / 0.15 mm. Current vendor capabilities support finer features, but four-layer fill/cap and stackup acceptance remain a real release hold.
- **Placement:** its coarse overlap list includes back-side BT1 against front-side parts, and text/footprint extents around the board edge. Native courtyard and physical clearance checks pass. Mechanical seating still requires review; no claim of collision-free installed battery mechanics is made.
- **Transfer:** 44 discrepancies compare intentional no-connect pin nets against an empty net. Native KiCad schematic parity reports zero issues. No no-connect pin was connected to satisfy this checker.
- **Footprint count:** the two extra board-only references G*** and REF** are the upstream artwork footprints, excluded from BOM and position files. They are not missing schematic components.
- **Cosmetics:** MCP score 80/100 (PASS); the schematic remains readable, and the added electrode annotation was visually checked. This is not an electrical failure.

Raw MCP results are included for transparency, not as release approval. The MCP evidence/manifest generator ignored the requested output path and enumerated only its default-output BOM; those tool results do not cover this package. The package’s own SHA256 manifest and `file-audit.json` cover the actual supplied files and source hashes.

## Export verification

Native KiCad exports replaced the failed MCP placement export. BOM and CPL were filtered to the same 43 fitted references; J1, TP1–TP9, and artwork are PCB features. Coordinate, side, rotation, DNP, copper-layer, file terminator, and drill-hit assertions passed. Plated drills: 73. Non-plated guide drills: 3. Two circular openings are present in Edge.Cuts.

RF/crystal/probe geometry was compared with the prior routed design using sampled segment positions, widths, and layers at 2 µm tolerance: all protected segments matched. This regression check is not an electromagnetic simulation or a hardware qualification.

## Human/vendor release requirements

Resolve Y2 sourcing, obtain stack/fill/cap and impedance acceptance, inspect component orientation/polarity and stencil data, approve mechanical seating and sensor handling, and perform prototype tests. Do not treat this draft as production-approved.
