# Instruction Contract

Dokumen ini mendefinisikan aturan kerja wajib saat membaca, menulis, atau
memodifikasi map ECU.

## Source of Truth

Gunakan dokumen berikut sebagai acuan utama:

- `REFERENCE_AXIS.md` — kontrak axis & ukuran matriks
- `REFERENCE_DETAIL.md` — spec mesin & constraint tuning
- `CONDITION.md` — state hardware aktual + empirical findings

## Parsing Rules

- Axis TPS: `21` titik (lihat `REFERENCE_AXIS.md`)
- RPM untuk `IGNITION TIMING`: `1000..9000` step `500` (17 titik)
- RPM untuk `BASEMAP`, `FUEL CORRECTION`, `INJECTOR TIMING`: `1000..9000` step `250` (33 titik)

## Matrix Contract

| Table | Size (TPS × RPM) |
|-------|------------------|
| `IGNITION TIMING` | `21 × 17` |
| `INJECTOR TIMING` | `21 × 33` |
| `FUEL CORRECTION` | `21 × 33` |
| `BASEMAP`         | `21 × 33` |

## Value Constraints

### Ignition (Juken 5++ constraint)
- Nilai HANYA kelipatan `0.5°` (valid: `11.0, 11.5, 12.0, ...`)
- Verifikasi: `(value × 2) == int(value × 2)` harus true untuk setiap cell
- Non-compliance → flash error saat upload ke ECU

### Duty Cycle
- Max duty WOT absolute: `70%`
- Preferred envelope: `<= 65-67%` untuk margin safety
- Formula: `duty = BM × (1 + FC/100) × RPM / 120000`

### Smoothness (no cliff)
- BM: RPM step `<= 0.5 ms`, TPS step `<= 1.5 ms`
- FC: RPM step `<= 2`, TPS step `<= 2`
- INJ_T: RPM step `<= 3°`, TPS step `<= 3°`
- IGN: RPM step `<= 2.5°`, TPS step `<= 2.0°`
- Zero zigzag sign-flip >= 0.5°

### Monotonicity
- BM: non-decreasing across TPS axis (higher TPS → BM >= previous row)
- Core2 BM >= Core1 BM everywhere
- Core2 IGN >= Core1 IGN everywhere
- IGN new >= IGN floor (e.g. 20-04 baseline)

## Validation Checklist

1. Ukuran matriks sesuai kontrak
2. Urutan baris mengikuti axis TPS tetap
3. Urutan kolom mengikuti axis RPM sesuai jenis tabel
4. Nilai tuning mematuhi batas praktis di `REFERENCE_DETAIL.md`
5. IGN snap 0.5° grid (zero violations)
6. Duty max `<= 70%`
7. Smoothness & monotonicity rules pass
8. No extra/missing row/column

## Output Quality Standard

- Konsisten format desimal dan delimiter (tab-separated)
- BM: 2 desimal (`8.67`), FC: integer (`+5`), INJ_T: integer (`268`), IGN: 1 desimal (`22.5`)
- Tidak mengubah struktur file di luar kebutuhan tuning
- Perubahan harus auditable dan dijelaskan secara teknis
- Generator script disimpan untuk reproducibility
