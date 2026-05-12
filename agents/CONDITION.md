# Current Vehicle Condition

Dokumen ini mencatat konfigurasi motor aktual yang menjadi konteks tuning.
Terakhir diperbarui: `2026-05-12`.

## Platform

- Model basis: Honda Revo FI / Revo X 2016
- ECU: Juken 5++ Dualband (fully configurable, dual core map)
- Engine internal: stock (piston, klep, noken, kopling, head, block belum diubah)

## Intake and Throttle

- Throttle body: `22 mm` bawaan
- TPS: racing (terkalibrasi di Juken, 0% tutup dan 100% WOT match)
- Filter: Daytona regular (paper kertas premium, flow match stock AHM)

## Fuel and Ignition Setup

- Injector: CBR150R 8-hole genuine Honda (part `16450-K15-601`, model 2019-2021,
  flow `~215 cc/min` @ 294 kPa, atomisasi halus 8-hole)
- Fuel pressure: `263-316 kPa` (stock)
- Busi terpasang: `CPR7EA` iridium
- Gap busi: `0.80-0.90 mm`
- Dwell: `3.5 ms` flat (Juken default untuk stock coil)

## Fuel Selection

- `core1` (harian halus): RON 92 atau RON 95 safe
- `core2` (tarik agresif): RON 95-98 (IGN advance lebih agresif butuh fuel premium)

## Lubrication and Electrical

- Oli: Mobil Delvac Modern 15W-40
- Kelistrikan: DC fullwave (suplai listrik stabil, support coil charge kuat)

## Empirical Calibration Findings

### K15-601 vs KZR real-world (2026-05-12)

Flash pertama map `10-05-2026` (designed untuk asumsi K15 +10-15% flow) dengan
injector K15-601 terpasang: motor banjir bensin, idle mati-mati, brebet.

User nyoba trim FC global untuk kalibrasi:

| Trim FC | Avg overfuel (simulated) | Gejala user |
|---------|--------------------------|-------------|
| `+0`    | `+25.3%`                 | Banjir total, brebet, idle mati |
| `-10`   | `+13.5%`                 | Masih rich parah |
| `-15`   | `+7.6%`                  | Lebih oke tapi masih bau bensin banget |
| `-20`   | `+1.7%`                  | Idle stable langsam, sedikit bau bensin |
| `-22`   | `~0%`                    | Target (no bau bensin) |

**Empirical finding**: K15-601 deliver net fuel `~30% lebih banyak` dari KZR
equivalent (bukan 10-15% seperti asumsi awal).

Root cause:
1. Physical flow rate: `+23%` (K15 215 cc/min vs KZR ~175 cc/min @ 294 kPa)
2. Atomization efficiency: `+5-8%` (8-hole genuine Honda spray pattern)
3. Compounded: `~28-30% effective fuel` per ms pulse

### Busi Black Fouling — Dual Root Cause

User observations sebelum map `12-05-2026`:
- Map `20-04-2026` KZR: busi hitam total (over-fuel + IGN retard)
- Map `10-05-2026` K15 as-is: banjir total (over-fuel +25%)

Root cause analysis:
1. **Fuel over-richness**: FC `+6..+13` di 20-04 dengan K15 → AFR 11-13 (rich), unburnt condense di busi
2. **IGN under-advance**: 20-04 cruise 13°, mid-load 20° terlalu retard untuk compression 9.3:1,
   burn lambat, mixture masih combusting saat exhaust buka → wet fouling

## Correction Strategy (applied in `12-05-2026`)

### Fuel — BASEMAP shift
```
BASEMAP_new = BASEMAP_20-04 × 0.78        # absorb K15 flow comp
FC_new      = zone-based [0..5] core1 / [0..7] core2
target_ms   = BASEMAP_20-04 × (1 + FC_20-04/100) × 0.78    # preserve AFR shape
```

### Ignition — zone advance dari proven floor
```
IGN_new = min( IGN_20-04 + zone_bonus(tps), zone_cap )

core1 bonus:  +4° lean / +3° mid / +2° power / +1.5° WOT   (cap 26° RON 92/95)
core2 bonus:  +3° lean / +2.5° mid / +1.5° power / +1° WOT (cap 28° RON 95-98)
```

Invariants:
- IGN never below 20-04 proven floor
- core2 BM ≥ core1 BM everywhere
- core2 IGN ≥ core1 IGN everywhere

## Juken Technical Constraints

### Ignition value granularity
- Nilai IGN **HANYA kelipatan 0.5°**. Valid: `11.0, 11.5, 12.0, ...`
- Semua proses aritmatika/interpolasi WAJIB snap ke grid 0.5° sebelum tulis
- Verifikasi: `(value × 2) == int(value × 2)` harus true

### Dwell
- Flat `3.5 ms` untuk stock coil Revo (jangan diubah tanpa ganti coil)

### Duty cycle target
- Injector WOT duty `<= 70%` absolute max
- Preferred working envelope `<= 65-67%` untuk safety margin

## Current Active Map

- Folder: `12-05-2026/`
- `core1.txt`: harian halus, FC 0..5, duty max 66.59%
- `core2.txt`: tarik agresif, FC 0..7, duty max 67.24%
- `juken.txt`: WARMUP/STARTER × 0.80, JET FUEL 180 pulse 1s

Detail audit + sample cells ada di `12-05-2026/README.md`.
