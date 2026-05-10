# Current Vehicle Condition

Dokumen ini mencatat konfigurasi motor aktual yang menjadi konteks tuning.
Terakhir diperbarui: `2026-05-10`.

## Platform

- Model basis: Honda Revo FI 110
- ECU: Juken 5++ Dualband (fully configurable, dual core map)
- Engine internal: standar (piston, klep, noken, kopling belum diubah)

## Intake and Throttle

- Throttle body: `22 mm` bawaan
- TPS: racing (sudah dikalibrasi ulang di Juken, 0% tutup dan 100% WOT match)
- Filter: Daytona regular (paper kertas hijau, bentuk + flow sama dengan stock AHM, kualitas kertas lebih baik dari AHM)

## Fuel and Ignition Related Setup

- Injector: CBR150 `8 hole` (upgrade dari KZR/Vario 125, debit lebih besar dan atomisasi lebih halus)
- Bahan bakar harian: RON 92 atau RON 95 (map harus aman di RON 92)
- Busi terpasang saat ini: `CPR7EA` iridium
- Gap busi: `0.80 - 0.90 mm`

## Lubrication and Electrical

- Oli: Mobil Delvac Modern 15W-40
- Kelistrikan: DC fullwave (suplai listrik lebih stabil)

## Observed Baseline Symptoms (Before Re-tune)

- Cruise `2000 - 4000 rpm` bukaan kecil: aman, halus.
- Snap WOT di mid RPM (`4000 - 6500 rpm`): tarikan terasa kosong/bog sesaat.
- Indikasi: zona mid RPM high TPS cenderung over-rich dengan map `20-04-2026`.

## Protected Tables (DO NOT MODIFY)

- `BASEMAP` di `core1` dan `core2` sudah disusun berbasis standar ECU dan sudah terverifikasi empiris. Percobaan rombak BASEMAP di masa lalu membuat motor mati saat di-gas.
- Semua penyesuaian tuning ke depan hanya boleh dilakukan pada `FUEL CORRECTION`, `INJECTOR TIMING`, `IGNITION TIMING`, dan parameter di `juken.txt`.
- `BASEMAP` harus dianggap immutable kecuali ada instruksi eksplisit dari owner.

### Exception: Redline Duty-Cap (authorized 2026-05-10)

- Zona `RPM >= 7750` × `TPS >= 65%` di BASEMAP diperbolehkan di-cap untuk menjaga injector duty cycle tetap di bawah `70%` (mengikuti target di `REFERENCE_DETAIL.md` section 4).
- Cell yang di-cap hanya boleh diturunkan (nilai asli tetap menjadi batas atas), tidak boleh dinaikkan.
- Zona di luar batas tersebut (operasional normal) tetap immutable.

## Tuning Implication

1. Injector CBR150 `8 hole` flow ~`10-15%` lebih besar dari KZR. Semua map fuel perlu trim proporsional agar tidak over-rich global.
2. Atomisasi `8 hole` lebih halus, burn lebih cepat, jadi advance ignition tidak perlu seagresif map lama. Sisakan margin untuk RON 92.
3. Fokus patch aktif ada di zona mid RPM (`4000 - 6500 rpm`) dengan TPS `>= 50%`, karena di sinilah gejala bog muncul.
4. Low RPM cruise sudah dilaporkan aman, jadi zona `TPS <= 40%` dan `RPM <= 3500` dijaga tidak diubah drastis.
5. Tetap hindari AFR terlalu rich di low RPM untuk mencegah fuel dilution ke oli (lihat `REFERENCE_DETAIL.md`).
6. Tetap pertahankan dua profil: `core1` halus untuk harian, `core2` responsif untuk tarik, tetapi keduanya aman di RON 92.
