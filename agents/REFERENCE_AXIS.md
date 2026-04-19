# Reference Axis: Juken Tables

Dokumen ini adalah kontrak axis tetap untuk semua tabel map.

## Overview

- Axis TPS digunakan pada semua tabel utama.
- `IGNITION TIMING` memakai axis RPM ignition (lebih renggang).
- `BASEMAP`, `FUEL CORRECTION`, `INJECTOR TIMING` memakai axis RPM utama (lebih rapat).

## TPS Axis (`21` titik)

```text
0, 2, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 100
```

## Ignition RPM Axis (`17` titik)

```text
1000, 1500, 2000, 2500, 3000, 3500, 4000, 4500, 5000, 5500, 6000, 6500, 7000, 7500, 8000, 8500, 9000
```

## Main RPM Axis for Basemap/Fuel/Injector (`33` titik)

```text
1000, 1250, 1500, 1750, 2000, 2250, 2500, 2750, 3000, 3250, 3500, 3750, 4000, 4250, 4500, 4750, 5000, 5250, 5500, 5750, 6000, 6250, 6500, 6750, 7000, 7250, 7500, 7750, 8000, 8250, 8500, 8750, 9000
```

## Matrix Size Contract

- `IGNITION TIMING`: `21 x 17` (`TPS x RPM_ignition`)
- `INJECTOR TIMING`: `21 x 33` (`TPS x RPM_main`)
- `FUEL CORRECTION`: `21 x 33` (`TPS x RPM_main`)
- `BASEMAP`: `21 x 33` (`TPS x RPM_main`)

## Implementation Rules

1. Baris selalu mengikuti urutan axis TPS.
2. Kolom mengikuti axis RPM sesuai jenis tabel.
3. Notasi `3500 -> 9000` harus diekspansi otomatis sesuai step section terkait.
4. Dilarang menambah atau mengurangi jumlah titik axis tanpa revisi dokumen.
