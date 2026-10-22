BLE Plant Monitor - request for U.S. fabrication and assembly
Prepared 2026-10-21. Source revision fb406b8. NOT YET SUBMITTED TO ANY VENDOR.

Requested scope: 5 fully assembled prototype boards, all 43 fitted references per board / 31 unique purchasing groups. Full turnkey PCB fabrication + component procurement + assembly on both sides + inspection + depanelization + domestic shipping and tax. Request explicit confirmation that BOTH bare-board fabrication and assembly take place in the United States. No offshore substitution. Cost target $500 all-in, or $490 new expenditure if including the already-paid $10 JLC matching service in the project budget. Prefer economical lead time; no rush premium required.

Fabrication requirements:
- 4 copper layers, board outline approx. 126.41 x 22 mm; use Gerber Edge.Cuts as authority. Supplier may add rails/panel tooling and remove before delivery.
- Nominal 1.6 mm FR4, TG155 or better. Existing JLC04161H-1080 reference construction totals 1.5182 mm before mask. Copper/dielectric from top: 0.035 / 0.0764 / 0.0152 / 1.265 / 0.0152 / 0.0764 / 0.035 mm. Outer prepreg FR4 1080, modeled Dk3.91; core modeled Dk4.6. Propose closest domestic stackup and obtain engineering approval before changing RF geometry.
- Minimum trace width 0.10 mm. Inspect exact clearances and annular rings from supplied Gerbers; do not round metric dimensions up to catalog inch limits without review.
- Via pad/drill pairs in CAD: 0.35/0.15 mm, 0.45/0.20 mm, 0.60/0.30 mm; 73 vias. Clarify interpretation of Excellon hole sizes and finished-hole requirements. Through-hole aspect ratio near 10.1:1 for actual stack or 10.7:1 at nominal 1.6 mm for 0.15 mm hole. Confirm process capability; do not silently replace with laser microvias.
- Request nonconductive epoxy-filled, copper-capped vias as in current fabrication release, ENIG, green solder mask, white silkscreen, electrical test. Confirm via treatment through engineering review; CAD global via settings do not replace this written fabrication requirement.
- Retain solder mask over the soil electrodes on both sides.

Assembly requirements:
- Bare Nordic nRF52840-QIAA-R AQFN-73 with exposed pad and staggered contacts, SHTC3 sensor, 0402 passives, front SMT and rear Keystone1060 battery holder. SWD Tag-Connect pads are board contacts, not an installed connector.
- Source C18 Murata GJM1555C1HR50WB01D, 0.5pF +/-0.05pF, 50V C0G, 0402 high-Q RF.
- Supplier-neutral BOM includes approved C6 reel suffix alternate and C12 equivalent; source parts from authorized distributors. Confirm any additional substitutions first. Quantities exclude vendor attrition; quote required spares separately.
- Inspect AQFN solder joints by X-ray; provide final placement/polarity preview for approval. Placement file has mm coordinates and KiCad rotations; perform vendor orientation review.
- Avoid sensor contamination and follow Sensirion reflow/handling requirements.
- Assembly drawings reflect unchanged placements; C18 procurement-only change leaves footprint and electrical value unchanged.

Prototype limitations: native ERC/DRC/unconnected/schematic parity all zero. Antenna network is an unmeasured starting point; post-build RF tuning/range, oscillator startup, supply droop and soil calibration still required. No firmware programming, CR2032 battery, enclosure, or functional-test fixture is included in this request.

Please return itemized NRE, board cost, stencil/assembly, parts and attrition, inspection, shipping, sales tax, lead time, manufacturing locations, required DFM changes, and an all-in total before any order.
