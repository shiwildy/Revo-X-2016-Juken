# Agents Reference Pack

Dokumentasi ini menjadi acuan resmi untuk parsing, validasi, dan tuning map ECU Juken pada proyek ini.

## Isi Dokumen

- `INSTRUCTION.md`: aturan kerja agent, kontrak input-output, dan checklist validasi.
- `CONDITION.md`: kondisi motor saat ini (konfigurasi real yang sedang dipakai).
- `REFERENCE_AXIS.md`: definisi axis tetap untuk semua tabel map.
- `REFERENCE_DETAIL.md`: detail spesifikasi mesin dan constraint tuning.
- `REFERENCE_JRJX_DECODE.md`: cara decode file `.Jrjx` ke data angka.

## Prinsip Penggunaan

1. Gunakan `REFERENCE_AXIS.md` sebagai kontrak ukuran matriks.
2. Gunakan `REFERENCE_DETAIL.md` dan `CONDITION.md` untuk keputusan tuning.
3. Jangan mengubah axis atau ukuran tabel tanpa pembaruan dokumen.
4. Semua perubahan map wajib konsisten dengan format tabel yang sudah ditetapkan.

## Status Dokumen

- Bahasa: Indonesia formal teknis.
- Scope: Honda Revo FI + ECU Juken pada konfigurasi proyek ini.
- Tujuan: konsistensi kerja, auditability, dan readiness untuk kolaborasi tim.
