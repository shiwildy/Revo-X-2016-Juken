# Map 12-05-2026 — K15-601 Calibrated (BASEMAP Rebuild)

## Purpose

Retuning komplit untuk injector CBR150R K15-601 genuine setelah empirical
finding bahwa flow real K15 `~30% lebih banyak` dari asumsi map `10-05-2026/`.

Pendekatan berubah dari `-20 FC flat emergency` menjadi **BASEMAP scaled × 0.78
plus FC positif kecil (3..12)**, sehingga FC tetap bisa dipakai untuk trim
zone-based tanpa saturasi di sisi negatif.

## Hardware Context

- Motor: Honda Revo FI 110 + Juken 5++ Dualband
- Injector: **K15-601 genuine** (CBR150R 2019-2021, part `16450-K15-601`)
- TPS: racing (terpasang 2026-05-12, WAJIB kalibrasi di Juken sebelum flash)
- Filter: Daytona regular (paper premium, flow = stock AHM)
- Busi: CPR7EA iridium, gap 0.80-0.90mm
- Fuel: RON 92 / RON 95

## Root Cause Analysis

### Pengamatan map 10-05-2026 + K15-601

| Test | Result |
|------|--------|
| Flash map 10-05 as-is | Banjir bensin, idle mati-mati, brebet |
| FC -20 flat emergency | Idle stable, ga bau bensin, tapi FC saturated |
| FC -15 flat | Masih rich |

### Math

- Map 10-05 average FC = ~+6 → effective fuel × 1.06
- Empirical need: effective fuel × 0.86 (FC avg -14)
- Overfuel ratio: 1.06 / 0.86 = **1.23x** → K15 real flow ~30% > KZR assumption

### Correction decision

FC flat -20 bikin map jadi "satu angka" dan hilang resolusi zone-based tuning.
Lebih bersih scale BASEMAP langsung supaya FC balik ke range positif kecil
(3..12) yang matching filosofi core1 baseline 20-04-2026.

Formula final:
```
BASEMAP_new = BASEMAP_20-04 × 0.78
FC_new      = FC_20-04 zone-adjusted (3..10 core1, 4..12 core2)
```

## Strategy Per Core

### core1 (harian halus)

- Target AFR: cruise 13.8-14.0, mid-load 13.3-13.5, redline 13.5+
- FC range: `3..10` floor 3 di cruise, cap 10 di power
- INJ_T: closed-valve cruise (~285°), mild open-valve WOT (~265-280°)
- IGN: MBT konservatif 6-25.5°, cocok RON 92

### core2 (tarik agresif)

- Target AFR: peak torque 12.8-13.0, cruise tetap aman 13.6-13.8
- FC range: `4..12` floor 4 (clean decel), cap 12 di power band 4500-7500 rpm
- INJ_T: lebih agresif open-valve (~260° di WOT, range 260-285°)
- IGN: reuse 20-04 core2 pattern, snap 0.5° grid (8-27°)
- Redline (RPM>=8500 TPS>=70%): FC di-taper 12→10 untuk duty safety

## Changes Summary

| Section | 20-04-2026 (KZR) | 12-05-2026 (K15) | Rationale |
|---------|------------------|------------------|-----------|
| BASEMAP core1 | Original shape | × 0.78 | K15 flow +30% comp |
| BASEMAP core2 | Original shape | × 0.78 | Same |
| FC core1 | 6..13 | 3..10 | Less needed after BASEMAP scale |
| FC core2 | 7..16 | 4..12 | Capped per user rule |
| INJ_T | Zoned baseline | Smooth gradient 258-285° | K15 8-hole tolerance |
| IGN core1 | Baseline | 0.5° grid snapped | Juken constraint |
| IGN core2 | Baseline | 0.5° grid snapped | Juken constraint |
| WARMUP | +10% cold | -10% cold end | K15 overfuel cold start |
| STARTER | × 0.90 | × 0.80 (total) | K15 cranking oversupply |
| JET FUEL | 380 pulse 1.2s | 180 pulse 1s | Snap over-fuel reduction |

## Audit Metrics

### core1
- Duty max: `63.77%` @ TPS 80% RPM 9000 ✓ (<70%)
- IGN 0.5° violations: `0` ✓
- IGN TPS step >=3°: `0` ✓
- IGN RPM step >=3°: `0` ✓
- INJ_T RPM step >=5°: `0` ✓
- AFR: idle 14.27, cruise 13.87, mid-load 13.36, peak-torque 13.36, redline 13.61

### core2
- Duty max: `66.08%` @ TPS 100% RPM 9000 ✓ (<70%)
- IGN 0.5° violations: `0` ✓
- FC range: `4..12` ✓ (within user rule)
- INJ_T range: `260..285` ✓
- IGN range: `8.0..27.0` ✓
- All smoothness checks: PASS

## Flash & Test Checklist

### Pre-flash

- [ ] Backup current FC config (lu set -20 flat) sebagai safety net
- [ ] Confirm K15-601 injector terpasang dengan seal benar
- [ ] **Kalibrasi TPS racing di Juken** (CRITICAL — closed → WOT → save)
- [ ] Verify live data: idle TPS ~0%, WOT TPS ~100%

### Flash steps

- [ ] Flash `12-05-2026/core1.txt` ke core 1
- [ ] Flash `12-05-2026/core2.txt` ke core 2
- [ ] Flash `12-05-2026/juken.txt` ke parameter section
- [ ] Verify upload sukses tanpa error

### Post-flash verification

- [ ] Cold start: first crank nyala, idle sustain `1100-1200 rpm`
- [ ] Warm idle: stabil `1400 ± 100 rpm` (AHAS spec)
- [ ] Cruise `2000-4000 rpm` bukaan `15-25%`: halus, ga brebet, ga bau bensin
- [ ] Snap WOT gear 2 dari cruise 3000 rpm: verify tarikan clean (no bog)
- [ ] Mid-RPM sustained `5000-6500 rpm` bukaan `40-70%`: dengerin knock
- [ ] Redline hati-hati gear 3 sampai `7500-8000 rpm`: verify smooth
- [ ] Switch core1 ↔ core2 di jalan: verify transition ga hentak

### Regression flags

- [ ] Masih bau bensin berat warm: trim FC `-1` global di zone affected
- [ ] Masih brebet idle: cek TPS calibration ulang (paling sering biang kerok)
- [ ] Susah nyala cold: naikkan STARTER FUEL back 5%
- [ ] Knock di WOT mid-RPM: trim IGN `-0.5°` di zona affected
- [ ] Core2 terasa terlalu rich di cruise: pakai core1 untuk harian

## References

- `agents/CONDITION.md` — hardware state + empirical calibration findings
- `agents/REFERENCE_DETAIL.md` — engine spec + tuning constraints
- `agents/REFERENCE_AXIS.md` — axis contract (21 TPS × 33 RPM main)
- `20-04-2026/` — KZR baseline (BASEMAP shape reference)
- `10-05-2026/README.md` — previous iteration (negative-FC attempt, superseded)
