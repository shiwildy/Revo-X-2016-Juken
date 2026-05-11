# Map Analysis Report — 10-05-2026

Generated: 2026-05-11 | Motor: Honda Revo FI 110 + Juken 5++ Dualband

Dokumen ini mencatat deep analysis setiap angka di map `10-05-2026/` per section, per core, per row. Sumber referensi: `agents/REFERENCE_AXIS.md`, `agents/REFERENCE_DETAIL.md`, `agents/CONDITION.md`.

---

## 1. Hardware & Constraint Checklist

- [x] Motor: Honda Revo FI 110 (bore 50mm, stroke 55.597mm, comp 9.3:1)
- [x] ECU: Juken 5++ Dualband
- [x] Injector: CBR150 `8 hole` (upgrade dari KZR, flow ~+10-15%)
- [x] TPS: racing (calibrated 0%↔100% di Juken)
- [x] Throttle body: `22mm` stock
- [x] Busi: `CPR7EA` iridium, gap `0.80-0.90mm`
- [x] Filter: Daytona regular (paper premium, flow = stock AHM)
- [x] Oli: Mobil Delvac 15W-40
- [x] Kelistrikan: DC fullwave
- [x] Bahan bakar: RON 92 (primary) atau RON 95 (premium)
- [x] AHAS idle target: `1400 ± 100 rpm` warm; cold 1100-1200

### Constraint rules locked

- [x] `BASEMAP` immutable kecuali zona redline (`RPM ≥7750` × `TPS ≥65%`) per authorized exception 2026-05-10
- [x] Injector duty cycle max target: `≤70%` (per `REFERENCE_DETAIL.md` section 4)
- [x] Axis contract: TPS 21 titik, RPM main 33 titik, RPM ignition 17 titik
- [x] Semua tuning change logged via git commit message

---

## 2. Matrix Contract Validation

| File | Section | Expected | Actual | Status |
|------|---------|----------|--------|--------|
| `core1.txt` | `BASEMAP` | `21x33` | `21x33` | ✓ |
| `core1.txt` | `FUEL CORRECTION` | `21x33` | `21x33` | ✓ |
| `core1.txt` | `INJECTOR TIMING` | `21x33` | `21x33` | ✓ |
| `core1.txt` | `IGNITION TIMING` | `21x17` | `21x17` | ✓ |
| `core2.txt` | `BASEMAP` | `21x33` | `21x33` | ✓ |
| `core2.txt` | `FUEL CORRECTION` | `21x33` | `21x33` | ✓ |
| `core2.txt` | `INJECTOR TIMING` | `21x33` | `21x33` | ✓ |
| `core2.txt` | `IGNITION TIMING` | `21x17` | `21x17` | ✓ |

---

## 3. Summary Metrics per Core

| Metric | core1 | core2 | Notes |
|--------|-------|-------|-------|
| Max duty cycle | `70.00%` | `70.03%` | Target ≤70% |
| Max duty cell  | `TPS 80% × RPM 8250` | `TPS 100% × RPM 7750` | Where duty peaks |
| BASEMAP range  | `1.34..10.48ms` | `1.35..10.48ms` | Fuel pulse in ms |
| FC range       | `3..10` | `4..12` | Enrichment units |
| IGN range      | `6.0°..25.5°` | `8.0°..30.0°` | BTDC |
| INJ_T range    | `265..285°` | `263..285°` | Start angle BTDC |

---

## 4. BASEMAP Immutability Audit

Scope authorized: `RPM ≥7750` × `TPS ≥65%` (redline duty-cap exception).
Semua cell di luar zona itu HARUS identik dengan baseline `20-04-2026/`.

### core1

- Total cells modified: `22`
- In-scope (redline zone): `22` ✓
- Out-of-scope violations: `0` ✓

| TPS | RPM | Original | New | Δ% | Reason |
|-----|-----|----------|-----|----|----|
| 65% | 9000 | `9.20ms` | `9.06ms` | `-1.5%` | duty cap |
| 70% | 8750 | `9.50ms` | `9.32ms` | `-1.9%` | duty cap |
| 70% | 9000 | `9.50ms` | `9.06ms` | `-4.6%` | duty cap |
| 75% | 8500 | `9.70ms` | `9.59ms` | `-1.1%` | duty cap |
| 75% | 8750 | `9.70ms` | `9.32ms` | `-3.9%` | duty cap |
| 75% | 9000 | `9.70ms` | `9.06ms` | `-6.6%` | duty cap |
| 80% | 8250 | `10.00ms` | `9.79ms` | `-2.1%` | duty cap |
| 80% | 8500 | `10.00ms` | `9.59ms` | `-4.1%` | duty cap |
| 80% | 8750 | `10.00ms` | `9.32ms` | `-6.8%` | duty cap |
| 80% | 9000 | `10.00ms` | `9.06ms` | `-9.4%` | duty cap |
| 85% | 8250 | `10.00ms` | `9.79ms` | `-2.1%` | duty cap |
| 85% | 8500 | `10.00ms` | `9.59ms` | `-4.1%` | duty cap |
| 85% | 8750 | `10.00ms` | `9.32ms` | `-6.8%` | duty cap |
| 85% | 9000 | `10.00ms` | `9.06ms` | `-9.4%` | duty cap |
| 90% | 8250 | `10.00ms` | `9.79ms` | `-2.1%` | duty cap |
| 90% | 8500 | `10.00ms` | `9.59ms` | `-4.1%` | duty cap |
| 90% | 8750 | `10.00ms` | `9.32ms` | `-6.8%` | duty cap |
| 90% | 9000 | `10.00ms` | `9.06ms` | `-9.4%` | duty cap |
| 100% | 8250 | `10.07ms` | `9.79ms` | `-2.8%` | duty cap |
| 100% | 8500 | `10.07ms` | `9.59ms` | `-4.8%` | duty cap |
| 100% | 8750 | `10.07ms` | `9.32ms` | `-7.4%` | duty cap |
| 100% | 9000 | `10.07ms` | `9.06ms` | `-10.0%` | duty cap |

### core2

- Total cells modified: `28`
- In-scope (redline zone): `28` ✓
- Out-of-scope violations: `0` ✓

| TPS | RPM | Original | New | Δ% | Reason |
|-----|-----|----------|-----|----|----|
| 65% | 9000 | `9.20ms` | `8.97ms` | `-2.5%` | duty cap |
| 70% | 8750 | `9.50ms` | `9.23ms` | `-2.8%` | duty cap |
| 70% | 9000 | `9.50ms` | `8.97ms` | `-5.6%` | duty cap |
| 75% | 8250 | `9.80ms` | `9.70ms` | `-1.0%` | duty cap |
| 75% | 8500 | `9.80ms` | `9.50ms` | `-3.1%` | duty cap |
| 75% | 8750 | `9.80ms` | `9.23ms` | `-5.8%` | duty cap |
| 75% | 9000 | `9.80ms` | `8.97ms` | `-8.5%` | duty cap |
| 80% | 8000 | `10.15ms` | `10.00ms` | `-1.5%` | duty cap |
| 80% | 8250 | `10.15ms` | `9.70ms` | `-4.4%` | duty cap |
| 80% | 8500 | `10.15ms` | `9.50ms` | `-6.4%` | duty cap |
| 80% | 8750 | `10.15ms` | `9.23ms` | `-9.1%` | duty cap |
| 80% | 9000 | `10.15ms` | `8.97ms` | `-11.6%` | duty cap |
| 85% | 8000 | `10.15ms` | `10.00ms` | `-1.5%` | duty cap |
| 85% | 8250 | `10.15ms` | `9.70ms` | `-4.4%` | duty cap |
| 85% | 8500 | `10.15ms` | `9.50ms` | `-6.4%` | duty cap |
| 85% | 8750 | `10.15ms` | `9.23ms` | `-9.1%` | duty cap |
| 85% | 9000 | `10.15ms` | `8.97ms` | `-11.6%` | duty cap |
| 90% | 8000 | `10.20ms` | `10.00ms` | `-2.0%` | duty cap |
| 90% | 8250 | `10.20ms` | `9.70ms` | `-4.9%` | duty cap |
| 90% | 8500 | `10.20ms` | `9.50ms` | `-6.9%` | duty cap |
| 90% | 8750 | `10.20ms` | `9.23ms` | `-9.5%` | duty cap |
| 90% | 9000 | `10.20ms` | `8.97ms` | `-12.1%` | duty cap |
| 100% | 7750 | `10.27ms` | `10.23ms` | `-0.4%` | duty cap |
| 100% | 8000 | `10.27ms` | `10.00ms` | `-2.6%` | duty cap |
| 100% | 8250 | `10.27ms` | `9.70ms` | `-5.6%` | duty cap |
| 100% | 8500 | `10.27ms` | `9.50ms` | `-7.5%` | duty cap |
| 100% | 8750 | `10.27ms` | `9.23ms` | `-10.1%` | duty cap |
| 100% | 9000 | `10.27ms` | `8.97ms` | `-12.7%` | duty cap |

---

## 5. FUEL CORRECTION Per-Row Analysis

Setiap row TPS dianalisa. AFR estimate = `14.7 / (1 + FC/100)` (rough, actual depends on BASEMAP calibration).

### core1 FC

| TPS | FC@1000 | FC@2500 | FC@4000 | FC@5500 | FC@7000 | FC@9000 | Row Min | Row Max | AFR@5500 | Zone |
|-----|---------|---------|---------|---------|---------|---------|---------|---------|----------|------|
| `0%` | 3 | 3 | 3 | 3 | 3 | 3 | 3 | 3 | `14.27` | DECEL |
| `2%` | 3 | 3 | 4 | 4 | 4 | 4 | 3 | 4 | `14.13` | CLOSED_CRUISE |
| `5%` | 3 | 4 | 5 | 5 | 5 | 5 | 3 | 5 | `14.00` | CLOSED_CRUISE |
| `10%` | 3 | 4 | 6 | 6 | 6 | 6 | 3 | 6 | `13.87` | CRUISE_LEAN |
| `15%` | 3 | 5 | 6 | 7 | 7 | 7 | 3 | 7 | `13.74` | CRUISE_LEAN |
| `20%` | 3 | 5 | 6 | 7 | 7 | 7 | 3 | 7 | `13.74` | CRUISE_LEAN |
| `25%` | 4 | 6 | 7 | 8 | 8 | 8 | 4 | 8 | `13.61` | CRUISE_LEAN |
| `30%` | 4 | 6 | 7 | 8 | 8 | 8 | 4 | 8 | `13.61` | MBT_PART_LOAD |
| `35%` | 4 | 6 | 7 | 8 | 8 | 8 | 4 | 8 | `13.61` | MBT_PART_LOAD |
| `40%` | 4 | 6 | 8 | 8 | 8 | 8 | 4 | 8 | `13.61` | MBT_PART_LOAD |
| `45%` | 4 | 7 | 8 | 9 | 9 | 9 | 4 | 9 | `13.49` | MBT_PART_LOAD |
| `50%` | 4 | 7 | 8 | 9 | 6 | 3 | 3 | 9 | `13.49` | MBT_PART_LOAD |
| `55%` | 4 | 7 | 8 | 9 | 6 | 3 | 3 | 9 | `13.49` | MID_LOAD_PEAK |
| `60%` | 4 | 7 | 8 | 9 | 6 | 3 | 3 | 9 | `13.49` | MID_LOAD_PEAK |
| `65%` | 3 | 7 | 9 | 10 | 6 | 3 | 3 | 10 | `13.36` | MID_LOAD_PEAK |
| `70%` | 3 | 7 | 9 | 10 | 6 | 3 | 3 | 10 | `13.36` | MID_LOAD_PEAK |
| `75%` | 3 | 7 | 9 | 10 | 6 | 3 | 3 | 10 | `13.36` | MID_LOAD_PEAK |
| `80%` | 3 | 7 | 9 | 10 | 6 | 3 | 3 | 10 | `13.36` | WOT_POWER |
| `85%` | 3 | 7 | 9 | 10 | 6 | 3 | 3 | 10 | `13.36` | WOT_POWER |
| `90%` | 3 | 7 | 9 | 10 | 6 | 3 | 3 | 10 | `13.36` | WOT_POWER |
| `100%` | 3 | 7 | 9 | 10 | 6 | 3 | 3 | 10 | `13.36` | WOT_POWER |

### core2 FC

| TPS | FC@1000 | FC@2500 | FC@4000 | FC@5500 | FC@7000 | FC@9000 | Row Min | Row Max | AFR@5500 | Zone |
|-----|---------|---------|---------|---------|---------|---------|---------|---------|----------|------|
| `0%` | 4 | 5 | 5 | 4 | 4 | 4 | 4 | 5 | `14.13` | DECEL |
| `2%` | 4 | 5 | 5 | 5 | 5 | 5 | 4 | 5 | `14.00` | CLOSED_CRUISE |
| `5%` | 4 | 5 | 6 | 6 | 6 | 6 | 4 | 6 | `13.87` | CLOSED_CRUISE |
| `10%` | 4 | 6 | 6 | 7 | 7 | 7 | 4 | 7 | `13.74` | CRUISE_LEAN |
| `15%` | 4 | 6 | 7 | 8 | 8 | 8 | 4 | 8 | `13.61` | CRUISE_LEAN |
| `20%` | 4 | 6 | 8 | 9 | 9 | 9 | 4 | 9 | `13.49` | CRUISE_LEAN |
| `25%` | 5 | 7 | 8 | 9 | 9 | 9 | 5 | 9 | `13.49` | CRUISE_LEAN |
| `30%` | 5 | 7 | 9 | 10 | 10 | 10 | 5 | 10 | `13.36` | MBT_PART_LOAD |
| `35%` | 5 | 7 | 9 | 10 | 10 | 10 | 5 | 10 | `13.36` | MBT_PART_LOAD |
| `40%` | 5 | 8 | 10 | 10 | 10 | 10 | 5 | 10 | `13.36` | MBT_PART_LOAD |
| `45%` | 5 | 8 | 10 | 11 | 11 | 11 | 5 | 11 | `13.24` | MBT_PART_LOAD |
| `50%` | 5 | 8 | 10 | 11 | 7 | 4 | 4 | 11 | `13.24` | MBT_PART_LOAD |
| `55%` | 5 | 8 | 10 | 11 | 7 | 4 | 4 | 11 | `13.24` | MID_LOAD_PEAK |
| `60%` | 5 | 8 | 10 | 11 | 7 | 4 | 4 | 11 | `13.24` | MID_LOAD_PEAK |
| `65%` | 4 | 9 | 11 | 12 | 7 | 4 | 4 | 12 | `13.12` | MID_LOAD_PEAK |
| `70%` | 4 | 9 | 11 | 12 | 7 | 4 | 4 | 12 | `13.12` | MID_LOAD_PEAK |
| `75%` | 4 | 9 | 11 | 12 | 7 | 4 | 4 | 12 | `13.12` | MID_LOAD_PEAK |
| `80%` | 4 | 9 | 11 | 12 | 7 | 4 | 4 | 12 | `13.12` | WOT_POWER |
| `85%` | 4 | 9 | 11 | 12 | 7 | 4 | 4 | 12 | `13.12` | WOT_POWER |
| `90%` | 4 | 9 | 11 | 12 | 7 | 4 | 4 | 12 | `13.12` | WOT_POWER |
| `100%` | 4 | 9 | 11 | 12 | 7 | 4 | 4 | 12 | `13.12` | WOT_POWER |

---

## 6. IGNITION TIMING Per-Row Analysis

Semua angka BTDC. Peak advance per row ditandai. Max cap: core1=32°, core2=34°.

### core1 IGN

| TPS | IGN@1000 | IGN@2500 | IGN@4000 | IGN@5500 | IGN@7000 | IGN@9000 | Row Peak | Peak @RPM |
|-----|----------|----------|----------|----------|----------|----------|----------|-----------|
| `0%` | 6.0° | 11.0° | 16.5° | 20.5° | 23.0° | 25.0° | `25.0°` | `8500` |
| `2%` | 6.0° | 11.5° | 17.5° | 21.5° | 23.0° | 25.0° | `25.0°` | `8500` |
| `5%` | 6.0° | 12.5° | 19.5° | 23.5° | 23.0° | 25.0° | `25.5°` | `6500` |
| `10%` | 6.0° | 12.5° | 19.5° | 23.0° | 22.5° | 25.0° | `25.0°` | `6500` |
| `15%` | 6.5° | 12.5° | 19.0° | 22.5° | 22.5° | 25.0° | `25.0°` | `6500` |
| `20%` | 6.5° | 13.0° | 20.0° | 23.0° | 22.5° | 25.0° | `25.0°` | `6500` |
| `25%` | 7.0° | 13.5° | 20.5° | 23.5° | 23.5° | 25.0° | `25.0°` | `6500` |
| `30%` | 7.0° | 13.0° | 20.0° | 23.5° | 25.5° | 25.0° | `25.5°` | `7000` |
| `35%` | 7.0° | 13.0° | 19.5° | 23.5° | 25.5° | 25.0° | `25.5°` | `7000` |
| `40%` | 7.0° | 13.0° | 19.5° | 24.0° | 25.5° | 25.0° | `25.5°` | `7000` |
| `45%` | 7.0° | 13.0° | 19.5° | 23.5° | 25.5° | 25.0° | `25.5°` | `7000` |
| `50%` | 6.5° | 12.0° | 18.5° | 22.5° | 25.0° | 25.0° | `25.0°` | `7000` |
| `55%` | 6.5° | 11.5° | 17.0° | 21.5° | 24.5° | 25.0° | `25.5°` | `7500` |
| `60%` | 6.5° | 11.5° | 17.0° | 21.5° | 24.5° | 25.0° | `25.5°` | `7500` |
| `65%` | 6.0° | 11.0° | 16.5° | 21.0° | 24.0° | 25.0° | `25.0°` | `7500` |
| `70%` | 6.0° | 11.5° | 17.0° | 21.0° | 24.0° | 25.0° | `25.0°` | `7500` |
| `75%` | 6.0° | 10.5° | 15.5° | 19.5° | 22.5° | 23.5° | `24.5°` | `8000` |
| `80%` | 6.0° | 10.5° | 15.5° | 19.5° | 23.0° | 22.0° | `24.5°` | `8000` |
| `85%` | 6.0° | 10.5° | 15.5° | 19.5° | 23.0° | 22.0° | `24.0°` | `7500` |
| `90%` | 6.0° | 9.5° | 14.5° | 19.0° | 22.5° | 22.0° | `24.0°` | `8000` |
| `100%` | 6.0° | 9.5° | 14.0° | 18.5° | 22.0° | 21.5° | `23.5°` | `8000` |

### core2 IGN

| TPS | IGN@1000 | IGN@2500 | IGN@4000 | IGN@5500 | IGN@7000 | IGN@9000 | Row Peak | Peak @RPM |
|-----|----------|----------|----------|----------|----------|----------|----------|-----------|
| `0%` | 9.0° | 14.0° | 19.5° | 23.5° | 26.0° | 26.5° | `26.5°` | `7500` |
| `2%` | 9.0° | 14.5° | 21.0° | 25.0° | 26.0° | 26.5° | `26.5°` | `6500` |
| `5%` | 9.0° | 15.5° | 23.0° | 27.0° | 26.5° | 26.5° | `28.5°` | `6500` |
| `10%` | 9.0° | 15.5° | 23.5° | 27.0° | 26.5° | 26.5° | `28.5°` | `6500` |
| `15%` | 9.5° | 16.0° | 23.0° | 26.5° | 26.5° | 26.5° | `28.5°` | `6500` |
| `20%` | 9.5° | 16.5° | 24.5° | 27.5° | 26.5° | 26.5° | `29.5°` | `6500` |
| `25%` | 10.5° | 17.5° | 25.5° | 28.5° | 28.0° | 26.5° | `30.0°` | `6500` |
| `30%` | 10.5° | 17.5° | 26.0° | 29.5° | 29.0° | 26.5° | `30.0°` | `6000` |
| `35%` | 10.5° | 17.5° | 26.0° | 29.5° | 29.0° | 26.5° | `30.0°` | `6000` |
| `40%` | 10.5° | 17.5° | 25.5° | 30.0° | 29.0° | 26.5° | `30.0°` | `5500` |
| `45%` | 10.5° | 17.5° | 25.5° | 30.0° | 29.0° | 26.5° | `30.0°` | `5500` |
| `50%` | 10.0° | 17.0° | 25.0° | 29.5° | 29.5° | 26.5° | `30.0°` | `6000` |
| `55%` | 10.0° | 16.5° | 23.5° | 28.0° | 28.5° | 26.5° | `29.0°` | `6500` |
| `60%` | 10.0° | 16.0° | 23.0° | 27.5° | 28.5° | 26.5° | `29.0°` | `6500` |
| `65%` | 9.5° | 15.5° | 22.0° | 26.5° | 28.0° | 27.0° | `29.0°` | `7500` |
| `70%` | 9.5° | 15.0° | 21.5° | 26.0° | 28.0° | 27.0° | `29.0°` | `7500` |
| `75%` | 9.0° | 14.5° | 20.0° | 24.0° | 26.5° | 27.0° | `27.5°` | `7500` |
| `80%` | 9.0° | 14.0° | 19.0° | 23.0° | 26.0° | 27.0° | `27.0°` | `7500` |
| `85%` | 8.5° | 13.5° | 18.5° | 22.5° | 26.0° | 26.0° | `26.5°` | `7500` |
| `90%` | 8.0° | 12.5° | 17.5° | 22.0° | 25.5° | 26.0° | `26.0°` | `7500` |
| `100%` | 8.0° | 12.5° | 16.5° | 21.5° | 25.0° | 25.5° | `25.5°` | `7500` |

---

## 7. INJECTOR TIMING Per-Row Analysis

Angka dalam ° BTDC compression. `≥280°` = closed-valve injection (fuel evap di port), `<280°` = open-valve injection (direct into valve).

### core1 INJ_T

| TPS | IT@1000 | IT@2500 | IT@4000 | IT@5500 | IT@7000 | IT@9000 | Closed-valve cells | Open-valve cells |
|-----|---------|---------|---------|---------|---------|---------|--------------------|------------------|
| `0%` | 285° | 285° | 283° | 280° | 278° | 278° | 23 | 10 |
| `2%` | 285° | 285° | 283° | 280° | 278° | 278° | 23 | 10 |
| `5%` | 285° | 285° | 283° | 280° | 278° | 278° | 23 | 10 |
| `10%` | 285° | 285° | 283° | 280° | 278° | 278° | 23 | 10 |
| `15%` | 285° | 285° | 283° | 279° | 276° | 276° | 18 | 15 |
| `20%` | 285° | 285° | 283° | 279° | 276° | 276° | 18 | 15 |
| `25%` | 283° | 283° | 283° | 277° | 273° | 273° | 17 | 16 |
| `30%` | 283° | 283° | 283° | 277° | 273° | 273° | 17 | 16 |
| `35%` | 283° | 283° | 283° | 277° | 273° | 273° | 17 | 16 |
| `40%` | 283° | 283° | 283° | 277° | 273° | 273° | 17 | 16 |
| `45%` | 283° | 283° | 283° | 277° | 273° | 273° | 17 | 16 |
| `50%` | 283° | 283° | 283° | 272° | 267° | 267° | 15 | 18 |
| `55%` | 283° | 283° | 283° | 272° | 267° | 267° | 15 | 18 |
| `60%` | 283° | 283° | 283° | 272° | 267° | 267° | 15 | 18 |
| `65%` | 283° | 283° | 283° | 272° | 265° | 265° | 15 | 18 |
| `70%` | 283° | 283° | 283° | 272° | 265° | 265° | 15 | 18 |
| `75%` | 283° | 283° | 283° | 272° | 265° | 265° | 15 | 18 |
| `80%` | 283° | 283° | 283° | 272° | 265° | 265° | 15 | 18 |
| `85%` | 283° | 283° | 283° | 272° | 265° | 265° | 15 | 18 |
| `90%` | 283° | 283° | 283° | 272° | 265° | 265° | 15 | 18 |
| `100%` | 283° | 283° | 283° | 272° | 265° | 265° | 15 | 18 |

### core2 INJ_T

| TPS | IT@1000 | IT@2500 | IT@4000 | IT@5500 | IT@7000 | IT@9000 | Closed-valve cells | Open-valve cells |
|-----|---------|---------|---------|---------|---------|---------|--------------------|------------------|
| `0%` | 285° | 285° | 283° | 280° | 278° | 278° | 23 | 10 |
| `2%` | 285° | 285° | 283° | 280° | 278° | 278° | 23 | 10 |
| `5%` | 285° | 285° | 283° | 280° | 278° | 278° | 23 | 10 |
| `10%` | 285° | 285° | 283° | 280° | 278° | 278° | 23 | 10 |
| `15%` | 285° | 285° | 283° | 279° | 276° | 276° | 18 | 15 |
| `20%` | 285° | 285° | 283° | 279° | 276° | 276° | 18 | 15 |
| `25%` | 283° | 283° | 283° | 277° | 273° | 273° | 17 | 16 |
| `30%` | 283° | 283° | 283° | 277° | 273° | 273° | 17 | 16 |
| `35%` | 283° | 283° | 283° | 277° | 273° | 273° | 17 | 16 |
| `40%` | 283° | 283° | 283° | 277° | 273° | 273° | 17 | 16 |
| `45%` | 283° | 283° | 283° | 277° | 273° | 273° | 17 | 16 |
| `50%` | 283° | 283° | 283° | 272° | 267° | 267° | 15 | 18 |
| `55%` | 283° | 283° | 283° | 272° | 267° | 267° | 15 | 18 |
| `60%` | 283° | 283° | 283° | 272° | 265° | 265° | 15 | 18 |
| `65%` | 283° | 283° | 283° | 272° | 265° | 265° | 15 | 18 |
| `70%` | 283° | 283° | 283° | 272° | 265° | 265° | 15 | 18 |
| `75%` | 283° | 283° | 283° | 272° | 265° | 263° | 15 | 18 |
| `80%` | 283° | 283° | 283° | 272° | 265° | 263° | 15 | 18 |
| `85%` | 283° | 283° | 283° | 272° | 265° | 263° | 15 | 18 |
| `90%` | 283° | 283° | 283° | 272° | 265° | 263° | 15 | 18 |
| `100%` | 283° | 283° | 283° | 272° | 265° | 263° | 15 | 18 |

---

## 8. Duty Cycle Heatmap (selected cells)

Duty cycle % per cell. Target ≤70%. Threshold indicators: `🟢<50%`, `🟡50-65%`, `🟠65-70%`, `🔴>70%`.

### core1 duty cycle (selected RPM columns)

| TPS | 1000 | 2500 | 4000 | 5500 | 6500 | 7500 | 8500 | 9000 |
|-----|------|------|------|------|------|------|------|------|
| `0%` | 🟢3% | 🟢6% | 🟢9% | 🟢7% | 🟢8% | 🟢9% | 🟢10% | 🟢10% |
| `2%` | 🟢3% | 🟢7% | 🟢9% | 🟢9% | 🟢10% | 🟢11% | 🟢12% | 🟢13% |
| `5%` | 🟢3% | 🟢9% | 🟢11% | 🟢13% | 🟢13% | 🟢12% | 🟢14% | 🟢15% |
| `10%` | 🟢4% | 🟢10% | 🟢14% | 🟢17% | 🟢17% | 🟢18% | 🟢19% | 🟢20% |
| `15%` | 🟢5% | 🟢12% | 🟢17% | 🟢20% | 🟢21% | 🟢22% | 🟢23% | 🟢25% |
| `20%` | 🟢6% | 🟢14% | 🟢19% | 🟢23% | 🟢24% | 🟢25% | 🟢27% | 🟢29% |
| `25%` | 🟢7% | 🟢17% | 🟢22% | 🟢26% | 🟢28% | 🟢30% | 🟢33% | 🟢35% |
| `30%` | 🟢7% | 🟢19% | 🟢26% | 🟢31% | 🟢32% | 🟢34% | 🟢38% | 🟢40% |
| `35%` | 🟢8% | 🟢20% | 🟢30% | 🟢36% | 🟢38% | 🟢40% | 🟢44% | 🟢47% |
| `40%` | 🟢8% | 🟢20% | 🟢32% | 🟢41% | 🟢44% | 🟢47% | 🟡51% | 🟡54% |
| `45%` | 🟢8% | 🟢21% | 🟢34% | 🟢44% | 🟢49% | 🟡52% | 🟡57% | 🟡60% |
| `50%` | 🟢8% | 🟢21% | 🟢34% | 🟢46% | 🟡52% | 🟡54% | 🟡59% | 🟡63% |
| `55%` | 🟢8% | 🟢22% | 🟢35% | 🟢47% | 🟡54% | 🟡58% | 🟡63% | 🟠66% |
| `60%` | 🟢9% | 🟢22% | 🟢35% | 🟢48% | 🟡56% | 🟡60% | 🟡65% | 🟠69% |
| `65%` | 🟢9% | 🟢23% | 🟢36% | 🟢50% | 🟡58% | 🟡62% | 🟠67% | 🟠70% |
| `70%` | 🟢9% | 🟢23% | 🟢37% | 🟡51% | 🟡60% | 🟡64% | 🟠69% | 🟠70% |
| `75%` | 🟢9% | 🟢23% | 🟢38% | 🟡53% | 🟡62% | 🟠66% | 🟠70% | 🟠70% |
| `80%` | 🟢9% | 🟢23% | 🟢38% | 🟡53% | 🟡62% | 🟠66% | 🟠70% | 🟠70% |
| `85%` | 🟢9% | 🟢23% | 🟢38% | 🟡53% | 🟡62% | 🟠66% | 🟠70% | 🟠70% |
| `90%` | 🟢9% | 🟢23% | 🟢38% | 🟡53% | 🟡62% | 🟠66% | 🟠70% | 🟠70% |
| `100%` | 🟢9% | 🟢23% | 🟢38% | 🟡53% | 🟡62% | 🟠66% | 🟠70% | 🟠70% |

### core2 duty cycle (selected RPM columns)

| TPS | 1000 | 2500 | 4000 | 5500 | 6500 | 7500 | 8500 | 9000 |
|-----|------|------|------|------|------|------|------|------|
| `0%` | 🟢3% | 🟢6% | 🟢9% | 🟢7% | 🟢8% | 🟢9% | 🟢10% | 🟢11% |
| `2%` | 🟢3% | 🟢7% | 🟢9% | 🟢9% | 🟢10% | 🟢11% | 🟢13% | 🟢14% |
| `5%` | 🟢3% | 🟢9% | 🟢11% | 🟢13% | 🟢13% | 🟢13% | 🟢15% | 🟢16% |
| `10%` | 🟢4% | 🟢10% | 🟢14% | 🟢17% | 🟢18% | 🟢18% | 🟢20% | 🟢21% |
| `15%` | 🟢5% | 🟢12% | 🟢17% | 🟢20% | 🟢21% | 🟢23% | 🟢24% | 🟢26% |
| `20%` | 🟢6% | 🟢14% | 🟢19% | 🟢23% | 🟢25% | 🟢27% | 🟢29% | 🟢30% |
| `25%` | 🟢7% | 🟢17% | 🟢23% | 🟢27% | 🟢28% | 🟢31% | 🟢33% | 🟢35% |
| `30%` | 🟢7% | 🟢19% | 🟢27% | 🟢31% | 🟢33% | 🟢36% | 🟢39% | 🟢41% |
| `35%` | 🟢8% | 🟢20% | 🟢31% | 🟢37% | 🟢39% | 🟢42% | 🟢46% | 🟢48% |
| `40%` | 🟢8% | 🟢21% | 🟢33% | 🟢42% | 🟢45% | 🟢49% | 🟡53% | 🟡56% |
| `45%` | 🟢8% | 🟢21% | 🟢34% | 🟢45% | 🟢50% | 🟡54% | 🟡59% | 🟡62% |
| `50%` | 🟢8% | 🟢22% | 🟢35% | 🟢47% | 🟡53% | 🟡56% | 🟡60% | 🟡64% |
| `55%` | 🟢9% | 🟢22% | 🟢36% | 🟢48% | 🟡55% | 🟡60% | 🟡64% | 🟠67% |
| `60%` | 🟢9% | 🟢22% | 🟢36% | 🟢49% | 🟡57% | 🟡62% | 🟠66% | 🟠70% |
| `65%` | 🟢9% | 🟢23% | 🟢37% | 🟡51% | 🟡59% | 🟡63% | 🟠68% | 🟠70% |
| `70%` | 🟢9% | 🟢23% | 🟢37% | 🟡52% | 🟡61% | 🟡65% | 🟠70% | 🟠70% |
| `75%` | 🟢9% | 🟢23% | 🟢39% | 🟡54% | 🟡63% | 🟠67% | 🟠70% | 🟠70% |
| `80%` | 🟢9% | 🟢23% | 🟢39% | 🟡54% | 🟡63% | 🟠67% | 🟠70% | 🟠70% |
| `85%` | 🟢9% | 🟢23% | 🟢39% | 🟡54% | 🟡63% | 🟠67% | 🟠70% | 🟠70% |
| `90%` | 🟢9% | 🟢23% | 🟢39% | 🟡54% | 🟡63% | 🟠68% | 🟠70% | 🟠70% |
| `100%` | 🟢9% | 🟢23% | 🟢39% | 🟡54% | 🟡63% | 🟠68% | 🟠70% | 🟠70% |

---

## 9. juken.txt Parameters Analysis

### WARMUP (fuel enrichment multiplier by ECT)

| ECT°C | Original | New | Δ | Purpose |
|-------|----------|-----|---|---------|
| `0` | `1.55` | `1.70` | `+0.15` | Cold start buffer (kompensasi FC leaner) |
| `10` | `1.35` | `1.48` | `+0.13` | Cold start buffer (kompensasi FC leaner) |
| `20` | `1.20` | `1.30` | `+0.10` | Cold start buffer (kompensasi FC leaner) |
| `30` | `1.00` | `1.08` | `+0.08` | Cold start buffer (kompensasi FC leaner) |
| `40` | `0.80` | `0.85` | `+0.05` | Warmup transition |
| `50` | `0.50` | `0.53` | `+0.03` | Warmup transition |
| `60` | `0.25` | `0.26` | `+0.01` | Warmup transition |
| `70` | `0.00` | `0.00` | `+0.00` | Merge baseline (full warm = AHM spec) |

### TIMING (ignition advance trim by ECT)

**Utuh dari baseline.** Thermal-based ignition trim bukan fungsi injector, ga perlu adjusted untuk CBR150.

| ECT°C | Value | Role |
|-------|-------|------|
| `0` | `3.0°` | Cold advance boost |
| `10` | `3.0°` | Cold advance boost |
| `20` | `2.5°` | Cold advance boost |
| `30` | `2.0°` | Cold advance boost |
| `40` | `1.5°` | Cold advance boost |
| `50` | `1.0°` | Tapering |
| `60` | `0.5°` | Tapering |
| `70` | `0.0°` | Warm baseline |

### STARTER FUEL (priming pulse by ECT)

Global `× 0.90` dari baseline untuk kompensasi flow CBR150. Confirmed: "starter sekali pencet nyala aman".

| ECT°C | Original | New | Ratio | Note |
|-------|----------|-----|-------|------|
| `0` | `3.10` | `2.80` | `0.903` | CBR150 comp |
| `10` | `2.60` | `2.35` | `0.904` | CBR150 comp |
| `20` | `2.10` | `1.90` | `0.905` | CBR150 comp |
| `30` | `1.60` | `1.45` | `0.906` | CBR150 comp |
| `40` | `1.20` | `1.10` | `0.917` | CBR150 comp |
| `50` | `0.75` | `0.70` | `0.933` | CBR150 comp |
| `60` | `0.30` | `0.28` | `0.933` | CBR150 comp |
| `70` | `0.00` | `0.00` | `0.000` | n/a |

### JET FUEL (accelerator pump enrichment)

Before: `380 pulse 2s` | After: `250 pulse 1.2s`

| Param | Original | New | Δ% |
|-------|----------|-----|----|
| Pulse | `380` | `250` | `-34%` |
| Duration | `2.0s` | `1.2s` | `-40%` |
| Total fuel mass (relative) | 100% | ~40% | -60% |
| Effective (× CBR150 flow 1.12) | n/a | ~44% | -56% |

---

## 10. AFR Trend Analysis (WOT TPS 100%)

Target pattern: AFR lean saat low-RPM (VE belum optimal), rich di peak torque zone, lean kembali di redline (thermal + duty protection).

### core1 WOT AFR progression

| RPM | Pulse (ms) | Duty % | FC | ~AFR | Zone | Assessment |
|-----|------------|--------|----|------|------|------------|
| `1500` | `10.57` | `13.2%` | 3 | `14.27` | WOT_UNREACHABLE | thermal protect |
| `2500` | `10.99` | `22.9%` | 7 | `13.74` | WOT_UNREACHABLE | transition |
| `4000` | `11.35` | `37.8%` | 9 | `13.49` | WOT_POWER | peak-torque |
| `5500` | `11.53` | `52.8%` | 10 | `13.36` | WOT_POWER | peak-torque |
| `6500` | `11.36` | `61.5%` | 10 | `13.36` | WOT_POWER | peak-torque |
| `7500` | `10.57` | `66.1%` | 5 | `14.00` | WOT_SHIFT | transition |
| `8500` | `9.88` | `70.0%` | 3 | `14.27` | WOT_REDLINE | thermal protect |
| `9000` | `9.33` | `70.0%` | 3 | `14.27` | WOT_REDLINE | thermal protect |

### core2 WOT AFR progression

| RPM | Pulse (ms) | Duty % | FC | ~AFR | Zone | Assessment |
|-----|------------|--------|----|------|------|------------|
| `1500` | `10.67` | `13.3%` | 4 | `14.13` | WOT_UNREACHABLE | thermal protect |
| `2500` | `11.19` | `23.3%` | 9 | `13.49` | WOT_UNREACHABLE | peak-torque |
| `4000` | `11.56` | `38.5%` | 11 | `13.24` | WOT_POWER | peak-torque |
| `5500` | `11.74` | `53.8%` | 12 | `13.12` | WOT_POWER | peak-torque |
| `6500` | `11.57` | `62.7%` | 12 | `13.12` | WOT_POWER | peak-torque |
| `7500` | `10.89` | `68.0%` | 6 | `13.87` | WOT_SHIFT | transition |
| `8500` | `9.88` | `70.0%` | 4 | `14.13` | WOT_REDLINE | thermal protect |
| `9000` | `9.33` | `70.0%` | 4 | `14.13` | WOT_REDLINE | thermal protect |

---

## 11. Known Acceptable Trade-offs

Items below flagged by audit tool tapi dianggap **acceptable** dengan justifikasi engineering:

- [x] **BASEMAP cliff 2-3% per step di RPM 8000-8750**: Konsekuensi matematis dari duty-cap + BASEMAP protected di zona normal. Pattern monotonic descent, bukan plunge. Match dengan natural VE rolloff mesin bore 50mm + cam 215°.
- [x] **core1 IGN peak di RPM 8500 TPS 0%/2%**: Decel zone (throttle tutup), no combustion load, zero knock risk.
- [x] **TPS 0% decel fuel drop 11.4% di RPM 4750→5000**: Inherent BASEMAP VE rolloff saat decel. Protected table, tidak boleh disentuh.
- [x] **EOI calculation flag**: Tool limitation (tidak mempertimbangkan 720° cycle wraparound). Juken manage injection wrap secara otomatis, bukan issue.

---

## 12. Flash & Test Checklist

### Pre-flash

- [ ] Backup current ECU config sebagai safety
- [ ] Inject CBR150 8-hole terpasang dan seal valid
- [ ] TPS racing terpasang, kalibrasi 0%↔100% di Juken software
- [ ] Busi CPR7EA iridium gap 0.80-0.90mm
- [ ] Filter Daytona regular terpasang (bentuk sama dengan stock)
- [ ] Fuel RON 92 atau 95 di tangki

### Flash steps

- [ ] Flash `10-05-2026/core1.txt` ke core 1 Juken
- [ ] Flash `10-05-2026/core2.txt` ke core 2 Juken
- [ ] Flash `10-05-2026/juken.txt` ke parameter section
- [ ] Verifikasi upload sukses tanpa error

### Post-flash verification

- [ ] Cold start: first crank harus hidup, idle sustain `1100-1200 rpm`
- [ ] Tunggu 3-5 menit: RPM drift turun pelan ke `1400 ± 100 rpm` (AHAS spec)
- [ ] Hot idle: stabil di `1400 rpm`, ga hunting, ga flare
- [ ] Tes cruise `2000-4000 rpm` bukaan `15-25%`: halus, ga brebet
- [ ] Tes snap WOT gear 2 dari cruise 3000 rpm: verify bog gone
- [ ] Tes mid-RPM sustained `5000-6500 rpm` bukaan `40-70%`: dengerin knock (suara pasir)
- [ ] Tes redline hati-hati di gear 3 sampai `7500-8000 rpm`: verify smooth
- [ ] Switch ke core2, repeat cruise/snap/sustained test
- [ ] Pasca pemakaian 1 hari: cek kondisi busi (coklat muda = ideal)

### Regression flags (stop test kalau terjadi)

- [ ] Knock/pasir berulang di mid-RPM (trim IGN -0.5° di zona affected)
- [ ] Idle goyang/mati setelah warm (trim WARMUP atau cek mekanis idle screw)
- [ ] AFR meter (kalau ada) baca <12.5 di cruise atau >15 di WOT (report gejala)
- [ ] Bog masih ada saat snap WOT (trim JET FUEL `250 → 220 pulse 1s`)

---

## 13. Git Audit Trail

| Commit | Description |
|--------|-------------|
| `79e872b` | docs: clarify filter is Daytona regular paper type |
| `4e891de` | docs: update filter spec to Daytona stock-replacement |
| `ed7d316` | feat: 10-05-2026 retune for CBR150 8-hole injector + TPS racing |
| `55d21dc` | chore: initial commit |

---

*Report auto-generated dari data map `10-05-2026/` dengan cross-reference spec di `agents/` folder.*