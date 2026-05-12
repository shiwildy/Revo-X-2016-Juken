# Map 12-05-2026 — K15-601 Calibrated

## Purpose

Retuning map untuk injector CBR150R K15-601 genuine Honda setelah empirical finding bahwa flow real K15 `~30% lebih banyak` dari asumsi awal map `10-05-2026/`.

## Hardware Context

- Motor: Honda Revo FI 110 + Juken 5++ Dualband
- Injector: **K15-601 genuine** (upgrade dari KZR, terpasang 2026-05-12)
- TPS: racing (terpasang 2026-05-12, wajib kalibrasi di Juken)
- Filter: Daytona regular (paper premium, flow = stock AHM)
- Busi: CPR7EA iridium, gap 0.80-0.90mm
- Fuel: RON 92 / RON 95

## Empirical Justification

### Pengamatan dari map 10-05-2026 + K15-601

| Test | Result |
|------|--------|
| Flash map 10-05-2026 as-is | Banjir bensin, idle mati-mati, brebet parah |
| FC set -20 flat (user emergency) | Idle stable, starter OK, ga bau bensin |
| FC set -15 flat | Masih bau bensin (rich) |
| Sweet spot | Antara -15 dan -20 |

### Math analysis

- Map 10-05-2026 average FC = ~6 → effective fuel × 1.06
- Empirical finding: need effective fuel × 0.86 (FC avg -14 empiric)
- Overfuel ratio: 1.06 / 0.86 = **1.23x** (23% too much from map design)
- K15 real flow ~30% more than KZR assumption

### Correction formula

```
FC_new = round(FC_old_kzr × 0.78) - 22
```

Applied to all cells. Final FC range:
- core1: `-20..-14`
- core2: `-19..-13`

## Changes Summary

| Section | 10-05-2026 | 12-05-2026 | Rationale |
|---------|------------|------------|-----------|
| BASEMAP | Redline capped, rest stock | Unchanged from 10-05 | Proven, no reason to touch |
| FUEL CORRECTION | 3..12 positive | -20..-13 negative | K15 flow correction |
| INJECTOR TIMING | Zoned baseline | -2° extra at WOT power | K15 genuine allows more aggressive open-valve |
| IGNITION TIMING | MBT pattern, 0.5° grid | Unchanged | Injector-independent |
| WARMUP | Tapered +10% cold | -10% cold end | K15 overfuel at cold start |
| TIMING | Unchanged | Unchanged | Thermal, not injector-dependent |
| STARTER FUEL | × 0.90 | × 0.80 (total) | K15 cranking oversupply |
| JET FUEL | 250 pulse 1.2s | **180 pulse 1s** | Accelerator pump cut, K15 snap over-fuel |

## Flash & Test Checklist

### Pre-flash

- [ ] Backup current FC config (lu set -20 flat) sebagai safety
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
- [ ] Snap WOT gear 2 dari cruise 3000 rpm: verify tarikan clean
- [ ] Mid-RPM sustained `5000-6500 rpm` bukaan `40-70%`: dengerin knock
- [ ] Redline hati-hati gear 3 sampai `7500-8000 rpm`

### Regression flags

- [ ] Masih bau bensin berat setelah warm: trim FC tambah -2 global
- [ ] Masih brebet di idle: cek TPS calibration ulang
- [ ] Susah nyala cold: naikkan STARTER FUEL back 5%
- [ ] Knock di WOT mid-RPM: trim IGN -0.5° di zona affected

## References

- `agents/CONDITION.md` — hardware state + empirical calibration findings
- `agents/REFERENCE_DETAIL.md` — engine spec + tuning constraints
- `agents/REFERENCE_AXIS.md` — axis contract
- `10-05-2026/README.md` — previous iteration (KZR-optimized, superseded)
