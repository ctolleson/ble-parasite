# MODULE-B design review — 2026-10-22

## Verified native KiCad checks

- Schematic ERC: zero violations (`erc.json`).
- PCB DRC: zero violations and zero unconnected items (`drc.json`), after copper refill and final schematic field synchronization.
- Native schematic netlist comparison: all 133 named component pads match the PCB. This includes 40 intentional no-connect nets; KiCad's `{slash}` name escape is normalized to `/` for comparison. U1.30 is +3V0 and J1.9 is GND.
- Rectangular 30.0 × 126.4 mm outline, x=53…83 and y=27…153.4 mm.
- Minimum routed width 0.15 mm. All 38 vias are ordinary F.Cu–B.Cu through vias, 0.60 mm diameter / 0.30 mm drill. Four layers; no microvias, blind or buried vias required by this layout.
- Seven resistors, six capacitors and the LED have 0805 footprints. Required specialized packages remain unchanged.
- The sensing electrode endpoint has only F.Cu and no solder-mask opening. CPARA and the surrounding probe ground remain covered.
- Front copper and silkscreen plot visually reviewed. The schematic PDF is included for review; a few harmless legacy junction dots remain in unused sheet space.

## MCP gate findings and review

Raw output is preserved in `mcp-gates.json`; it predates the final metadata-only synchronization.

- PCB quality gate: PASS, including courtyard checks.
- Transfer gate: FAIL on 40 intentional no-connect pads because it expects empty net names. Native KiCad assigns unique `unconnected-…` nets. The independent native-netlist comparison verifies all 40 as well as the 93 connected pads. No pad-net mismatch remains.
- Connectivity heuristic: flags U1.30 and J1.9 at wire-interior connections. Native netlisting confirms +3V0 and GND respectively, and ERC passes.
- Placement heuristic flags battery-holder overlaps with front components. BT1 is on B.Cu; those components are on F.Cu. Native same-side courtyard checks pass. Its outside-board flags for U1/BT1 do not match their transformed physical body extents: U1 body x=62.75…73.25, y=27.25…42.75 mm; BT1 body lies about x=59.65…76.35, y=50.95…80.05 mm, inside the board.
- C4/C5 and J2/R8 bounding-box overlap flags are not native courtyard intersections; DRC reports zero courtyard violations.
- The decoupling heuristic uses module center distance (11.05 mm). C5's +3V0 pad is at (72,44.45), near module power pads (69.6,42.15) and (70.4,42.15), connected by short direct host power tracks. The module has internal decoupling; C5/C6 remain host bypass/bulk capacitors.
- The top antenna keepout excludes copper, tracks and vias on all four copper layers. The MCP-generated rule area additionally names Edge.Cuts; this is a rule-area layer tag, not an extra outline segment. The native outline remains the four intended edges. Review antenna clearance in the final enclosure.

## Remaining release and prototype checks

- Exact J1/J2/J3 connector manufacturer part numbers and mechanical compatibility must be qualified. The generic J1 SMT land pattern is not a claim that every 1.27 mm header fits. Confirm J-Link cable orientation, adapter, and pin-7 keying. VTref is not target power.
- This revision changes parts and geometry. Recheck inventory, assembly sourcing, fabrication/assembly DFM, and cost before generating an order package; prior Gerbers and quotes are obsolete for MODULE-B.
- NFC requires an external antenna and its matching/tuning components. The integrated chip antenna is only for BLE.
- Firmware GPIO assignments must follow the new schematic. Verify external LF crystal startup, sleep current, BLE performance, NFC operation and soil-sensor calibration on prototypes.
- Human engineering review and hardware testing are still required. No production-readiness claim is made from ERC/DRC alone.

## Horizontal button alignment

SW1 is at (56, 51) mm and RST1 at (65, 51) mm, both rotated -90°. RST1 moved up 1.5 mm; its local reset route and via were adjusted to avoid placing the via in the SMT pad. Native DRC was rerun: zero violations and zero unconnected items. All component pad-net assignments are unchanged. The front preview and source hash are refreshed; the earlier MCP gate capture remains historical evidence.

## 3D preview

Restored available KiCad library model assignments, including the Raytac module, connectors and Omron B3U-1000P buttons. `pcb-3d.png` shows SW1 and RST1 on the same horizontal board axis. Q4 and U2 have no assigned 3D body, so their pads remain visible in the preview. This is a model preview, not a fully populated assembly photograph. Verified all footprint positions and pad geometry/net assignments remained unchanged. DRC after the library refresh: zero violations and zero unconnected items.
