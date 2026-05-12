# Agents Reference Pack

Dokumentasi ini jadi acuan resmi untuk parsing, validasi, dan tuning map ECU
Juken 5++ Dualband pada proyek ini.

## Isi Dokumen

- `INSTRUCTION.md` — aturan kerja agent, kontrak input-output, checklist validasi
- `CONDITION.md` — kondisi motor aktual (hardware state + empirical findings)
- `REFERENCE_AXIS.md` — definisi axis tetap untuk semua tabel map
- `REFERENCE_DETAIL.md` — detail spesifikasi mesin dan constraint tuning
- `REFERENCE_JRJX_DECODE.md` — cara decode file `.Jrjx` ke data angka

## Prinsip Penggunaan

1. `REFERENCE_AXIS.md` = kontrak ukuran matriks (baris TPS, kolom RPM). Dilarang diubah.
2. `REFERENCE_DETAIL.md` + `CONDITION.md` = sumber keputusan tuning.
3. Axis atau ukuran tabel tidak boleh diubah tanpa revisi dokumen.
4. Semua perubahan map wajib konsisten dengan format tabel yang ditetapkan.
5. Juken IGN granularity = 0.5° grid, WAJIB snap sebelum tulis.

## Active Map

Map aktif ada di folder `12-05-2026/` (root repo). Detail audit metrics,
sample cells, dan flash checklist ada di `12-05-2026/README.md`.

## Status Dokumen

- Bahasa: Indonesia formal teknis
- Scope: Honda Revo FI 2016 + Juken 5++ + K15-601 injector
- Tujuan: konsistensi kerja, auditability, kolaborasi AI agent
