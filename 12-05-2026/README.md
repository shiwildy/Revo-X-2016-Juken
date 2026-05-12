# Map 12-05-2026 — K15-601 Calibrated (Final)

## Purpose

Map final untuk injector CBR150R K15-601 genuine + racing TPS. Dirombak
total dari 20-04-2026 (KZR baseline) dengan dua perbaikan utama:

1. **Fuel**: BASEMAP × 0.78 absorb K15 flow comp (+28% real flow)
2. **Ignition**: advance bertambah `+4° lean, +3° mid, +2° power, +1.5° WOT`
   dari 20-04 baseline untuk fix busi hitam basah

## Hardware Context

- Motor: Honda Revo FI 110 + Juken 5++ Dualband
- Injector: **K15-601 genuine** (CBR150R 2019-2021, part `16450-K15-601`)
- TPS: racing (terpasang 2026-05-12, WAJIB kalibrasi di Juken)
- Filter: Daytona regular (paper premium, flow = stock AHM)
- Busi: CPR7EA iridium, gap 0.80-0.90mm
- Fuel: RON 92 / RON 95

## Problem Analysis — Dual Root Cause

### Observation history

| Map state | Gejala |
|-----------|--------|
| 20-04 KZR + KZR | Busi hitam total (over-fuel + IGN retard) |
| 10-05 + K15 as-is | Banjir total, brebet (+25% over-fuel) |
| 10-05 + FC -20 flat | Idle stable, sedikit bau bensin (+1.7% rich) |

### Root cause breakdown

**Fuel over-richness** (main culprit 20-04 KZR):
- FC `+6..+13` di map 20-04 bikin AFR 11-13 (rich)
- K15 real flow `+28%` bikin 10-05 map over-fuel `+25%` rata-rata
- **Fix**: BASEMAP × 0.78 + FC kompres ke 0..5 → AFR target stoich cruise

**IGN under-advanced** (contributing cause):
- 20-04 IGN cruise 13°, mid-load 20° — terlalu retard untuk compression 9.3:1
- Burn lambat → mixture masih combusting saat exhaust buka
- Unburnt fuel droplets condense di busi → wet fouling + carbon
- **Fix**: advance `+2..+4°` dari 20-04 floor, per-zone disesuaikan knock margin

## Design

### Fuel math

```
target_ms[i][j] = BM_20-04[i][j] × (1 + FC_20-04[i][j]/100) × 0.78
BM_new[i][j]    = target_ms[i][j] / (1 + FC_new[i][j]/100)
FC_new          = zone-based [0..5] core1, [0..7] core2
```

`0.78` = 1/1.282, empirical K15 flow compensation.

### Ignition math

```
IGN_new[i][j] = min( IGN_20-04[i][j] + zone_bonus(tps), zone_cap(tps) )
zone_bonus(tps) =
  +4.0° if tps ≤ 25  (lean, flame slow, no knock)
  +3.0° if tps ≤ 60  (mid knock-monitor)
  +2.0° if tps ≤ 85  (power knock-limited)
  +1.5° if tps > 85  (full WOT RON 92 safe)

zone_cap(tps) = 28.5 / 27.5 / 25.5 / 25.0 per zone
```

Hard invariant: **IGN_new ≥ IGN_20-04 never**. 20-04 = proven floor.

## Strategy Per Core

### core1 (harian halus)

- Target AFR: cruise 13.8-14.0, mid-load 13.3-13.5, redline 13.5+
- FC: `0..5` (floor 0 idle/decel, cap 5 di power 4500-7500)
- BM: `1.11..8.74`
- INJ_T: `265..285°` smooth bilinear
- IGN: `7.5..28.5°` (20-04 floor + zone advance)

### core2 (tarik agresif)

- Target AFR: cruise 13.6-13.8, peak torque 13.0-13.2, redline 13.5+
- FC: `0..7` (headroom lebih lebar)
- BM: `1.13..8.86` (≥ core1 di setiap cell)
- INJ_T: `258..285°` (core2 open-valve lebih agresif)
- IGN: `8.5..29.0°` (20-04 core2 floor + zone advance)
- Hard rule: core2 BM ≥ core1 BM, core2 IGN ≥ core1 IGN

## Audit Metrics

### core1
- Duty max: `66.59%` @ TPS 100% RPM 9000 ✓ (<70%)
- FC: `0..5` ✓
- BM: `1.11..8.74`
- INJ_T: `265..285`
- IGN: `7.5..28.5°`
- IGN 0.5° violations: `0` ✓
- IGN zigzag: `0` ✓
- BM TPS-descending: `0` ✓
- FC RPM dips: `0` ✓

### core2
- Duty max: `67.24%` @ TPS 100% RPM 9000 ✓
- FC: `0..7`
- BM: `1.13..8.86` (≥ core1 everywhere)
- INJ_T: `258..285`
- IGN: `8.5..29.0°` (≥ core1 everywhere)
- IGN 0.5° violations: `0` ✓
- IGN zigzag: `0` ✓

## Sample Cells

### core1 vs core2 comparison

| Zone | TPS | RPM | c1 BM | c1 FC | c1 IGN | c2 BM | c2 FC | c2 IGN |
|------|-----|-----|-------|-------|--------|-------|-------|--------|
| idle        | 0   | 1500 | 2.65 | +0 | 11.5° | 2.67 | +0 | 12.5° |
| cruise      | 15  | 3000 | 4.53 | +1 | 17.0° | 4.53 | +1 | 19.5° |
| load        | 30  | 4000 | 6.32 | +2 | 20.5° | 6.32 | +3 | 24.5° |
| mid-load    | 50  | 5500 | 7.86 | +4 | 23.0° | 7.92 | +5 | 28.0° |
| power       | 75  | 6500 | 8.67 | +5 | 23.0° | 8.67 | +7 | 25.5° |
| peak-torque | 100 | 6500 | 8.67 | +5 | 22.5° | 8.74 | +7 | 24.0° |
| redline     | 100 | 9000 | 8.62 | +3 | 25.0° | 8.62 | +4 | 25.5° |

### Differensiasi

- **BM**: core2 ≥ core1, gap max +0.24ms di peak torque
- **FC**: core2 headroom lebih luas (0..7 vs 0..5)
- **IGN**: core2 advance +1..+5° vs core1 (paling terasa di mid-load 50%)
- **INJ_T**: core2 -7° WOT (more open-valve, better atomization)

## Flash & Test Checklist

### Pre-flash
- [ ] Backup current state (FC -20 flat di 10-05) sebagai safety net
- [ ] Confirm K15-601 injector terpasang dengan seal benar
- [ ] **Kalibrasi TPS racing di Juken** (CRITICAL — closed → WOT → save)
- [ ] Verify live data: idle TPS `~0%`, WOT TPS `~100%`

### Flash steps
- [ ] Flash `12-05-2026/core1.txt` ke core 1
- [ ] Flash `12-05-2026/core2.txt` ke core 2
- [ ] Flash `12-05-2026/juken.txt` ke parameter section
- [ ] Verify upload sukses tanpa error

### Post-flash verification (core1 dulu)
- [ ] Cold start: first crank nyala, idle sustain `1100-1200 rpm`
- [ ] Warm idle: stabil `1400 ± 100 rpm`, **ga bau bensin**
- [ ] Cruise `2000-4000 rpm` bukaan `15-25%`: halus, ga brebet
- [ ] Snap WOT gear 2 dari cruise 3000 rpm: tarikan clean (no bog)
- [ ] Mid-RPM sustained `5000-6500 rpm` bukaan `40-70%`: **dengerin knock seksama**
- [ ] Redline hati-hati gear 3 sampai `7500-8000 rpm`

### Post-flash core2
- [ ] Switch ke core2 di jalan: verify ga hentak transisi
- [ ] Feel: core2 harus lebih responsif mid-RPM TPS 50%+
- [ ] Knock watch: core2 advance lebih agresif, monitor cermat di RON 92

### Busi check (penting buat verify IGN)
- [ ] Setelah 50-100km cruise: cek warna busi
- [ ] Target: coklat keabuan (stoich clean burn)
- [ ] Kalau masih hitam basah: IGN masih kurang, atau fuel masih rich
- [ ] Kalau abu putih pecah: IGN over-advance atau lean, turunkan IGN `-1°`

### Regression flags (gunakan headroom FC + IGN)
- [ ] Masih bau bensin: FC `-1` di zone affected
- [ ] Tarikan kurang: FC `+1` di zone affected
- [ ] Knock di mid-load: IGN `-0.5°` di zona affected
- [ ] Knock di WOT: turunkan IGN WOT `-1°`
- [ ] Brebet idle: cek TPS calibration ulang

## References

- `agents/CONDITION.md` — hardware state + empirical calibration findings
- `agents/REFERENCE_DETAIL.md` — engine spec + tuning constraints (9.3:1, RON 92)
- `agents/REFERENCE_AXIS.md` — axis contract (21 TPS × 33 RPM main)
- `20-04-2026/` — KZR baseline (ANCHOR untuk BM shape & IGN floor)
- `10-05-2026/` — K15+10-15% assumption era (superseded, banjir)
