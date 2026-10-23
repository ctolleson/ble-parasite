# Large-SMT bare-nRF52840 plant monitor

Independent design derived from the bare-nRF52840 revision at commit 62d79fc.
Branch starts at origin/main fa509fd, and the original project is preserved.

## Design requirements

- Imperial 1008 minimum for chip passives; use 1210 where standard parts are available.
- Bare nRF52840 and other ICs retain their required packages.
- Retain SWD, soil capacitance, temperature/humidity, light sensing, battery operation and BLE.
- CPARA and surrounding soil ground remain solder-mask covered.
- Enlarge the electronics region as required; preserve the sensing electrode geometry.
- User approved exceptions: C17-C19, L3-L5 (RF matching), Y1, Y2, Q4, D1, AE1 (specialized parts). U1/U2 retain required IC packages. Q1 retains its SOT-23 package (approximately 2.9 x 2.4 mm overall including leads).

## Status

23 resistor/capacitor footprints enlarged to 1210; L1 changed to 4 x 4 mm SRN4018, L2 to Coilcraft 1008, and D2 to PLCC-2. Not a manufacturing release. Placement, routing, capacitor purchasing-part qualification, ERC and DRC are pending. Existing quotes apply to the prior revision.
