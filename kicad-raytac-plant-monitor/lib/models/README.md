# Sensor 3D model provenance

## U2 — Sensirion SHTC3

- Product: https://sensirion.com/products/catalog/SHTC3
- Official STEP download: https://sensirion.com/media/documents/2E0A04C9/61643CC2/Sensirion_Humidity_Sensors_Software_SHTC3_STEP_file.zip
- Archive member: `HT_CD_SHTC3_STEP_File_V1_D1.STEP`, copied without modification to `SHTC3.step` on 2026-10-22.
- The official archive is advertised as SHTC3; its STEP header retains an SHTC1 family name.
- Body envelope: 2 × 2 × 0.75 mm.
- KiCad rotation: (90, 0, 0) degrees; offset: (-122.25, -17.25, 0) mm; scale: (1, 1, 1). The offset compensates for the source model's remote origin.

## Q4 — Everlight ALS-PT19-315C/L177/TR8

- Existing repository model: `../snapeda/TR8--3DModel-STEP-56544.STEP`; its STEP header identifies the exact part.
- Datasheet: https://en.everlight.com/wp-content/plugins/ItemRelationship/product_files/pdf/ALS-PT19-315C-L177-TR8_V8.pdf
- Body envelope: 1.7 × 0.8 × 0.6 mm.
- KiCad rotation: (90, 0, 0) degrees; offset: (0, 0, 0) mm; scale: (1, 1, 1).

Both models were visually checked in native KiCad previews and in the final PCB render. Model transforms affect visualization only.
