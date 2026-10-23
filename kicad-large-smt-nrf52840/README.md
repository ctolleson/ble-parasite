# Large-SMT bare-nRF52840 plant monitor

Independent design derived from the bare-nRF52840 revision at commit 62d79fc.
Branch starts at origin/main fa509fd, and the original project is preserved.

## Design requirements

- Imperial 1008 minimum for chip passives; use 1210 where standard parts are available.
- Bare nRF52840 and other ICs retain their required packages.
- Retain SWD, soil capacitance, temperature/humidity, light sensing, battery operation and BLE.
- CPARA and surrounding soil ground remain solder-mask covered.
- Enlarge the electronics region as required; preserve the sensing electrode geometry.
- RF package policy awaits the user's decision.

## Status

Baseline copy only. Not a manufacturing release. Footprint changes, placement, routing,
component qualification, ERC and DRC are pending. Existing quotes apply to the prior revision.
