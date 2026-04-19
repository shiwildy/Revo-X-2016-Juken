# Instruction Contract

Dokumen ini mendefinisikan aturan kerja wajib saat membaca, menulis, atau memodifikasi map ECU.

## Source of Truth

Gunakan dokumen berikut sebagai acuan utama:

- `REFERENCE_AXIS.md`
- `REFERENCE_DETAIL.md`
- `CONDITION.md`

## Parsing Rules

- Axis TPS: `21` titik (lihat `REFERENCE_AXIS.md`).
- RPM untuk `IGNITION TIMING`: `1000..3500` lalu step `500` sampai `9000`.
- RPM untuk `BASEMAP`, `FUEL CORRECTION`, `INJECTOR TIMING`: `1000..3500` lalu step `250` sampai `9000`.

## Matrix Contract

- `IGNITION TIMING`: `21 x 17`
- `INJECTOR TIMING`: `21 x 33`
- `FUEL CORRECTION`: `21 x 33`
- `BASEMAP`: `21 x 33`

## Validation Checklist

1. Ukuran matriks harus sesuai kontrak.
2. Urutan baris harus mengikuti axis TPS tetap.
3. Urutan kolom harus mengikuti axis RPM sesuai jenis tabel.
4. Nilai tuning harus mematuhi batas praktis pada `REFERENCE_DETAIL.md`.
5. Tidak boleh ada baris/kolom ekstra atau hilang.

## Output Quality Standard

- Konsisten format desimal dan delimiter tabel.
- Tidak mengubah struktur file di luar kebutuhan tuning.
- Perubahan harus dapat di-audit dan dijelaskan secara teknis.
