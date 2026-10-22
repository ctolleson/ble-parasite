# Y2 replacement review — 2026-10-21

User-approved substitution: Golledge MP12308 to KYOCERA AVX CX2016DB32000D0FLJCC.

- 32 MHz fundamental, 8 pF load, 60 ohm maximum ESR.
- Exact order code: +/-10 ppm initial, +/-15 ppm temperature stability, -30 to +85 C. These terms total +/-25 ppm before aging and circuit-load error. Prototype frequency and startup measurements remain required. This temperature range limits the assembled product accordingly.
- CX2016DB family specification: 2 pF maximum shunt capacitance; 10 uW nominal / 100 uW maximum drive. Confirm exact ordered-part specification with supplier if deviations are proposed.
- Existing Crystal_SMD_2016-4Pin_2.0x1.6mm land pattern matches: pads 0.9 x 0.8 mm; centers 1.4 x 1.1 mm; crystal pins 1/3, grounded case pins 2/4.
- C14/C15 remain 12 pF. No placement, routing, stackup, mask, or paste change required.
- DigiKey cut tape: 478-CX2016DB32000D0FLJCCCT-ND; tape/reel: 478-CX2016DB32000D0FLJCCTR-ND.
- Signed-in DigiKey page observed 45,839 units; USD 0.91 at 1, 0.793 each at 10, 0.6894 each at 100. Availability is not a reservation.
- LCSC field intentionally blank: DigiKey identifiers must not be put in the JLCPCB LCSC column. JLCPCB procurement/consignment acceptance is still required.

Sources:
- https://www.digikey.com/en/products/detail/kyocera-avx/CX2016DB32000D0FLJCC/4453818
- https://ele.kyocera.com/sites/default/files/assets/products/crystal-device/cx2016db_e.pdf
- https://docs.nordicsemi.com/r/bundle/ps_nrf52840/page/clock.html

Native KiCad save normalized schematic ordering/whitespace and added the hidden standard Intersheetrefs field to CPARA; neither changes connectivity. Native PCB netlist import preview reported only Update Y2 fields, with zero warnings/errors.
