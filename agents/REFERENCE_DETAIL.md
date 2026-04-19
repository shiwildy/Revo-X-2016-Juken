# Engine and Tuning Reference (Revo FI)

Dokumen ini merangkum spesifikasi mesin dan constraint tuning untuk menjaga performa serta reliability.

## 1) Engine Base

- Tipe: 4-tak, SOHC, 2-valve, pendingin udara
- Bore x stroke: `50.000 x 55.597 mm`
- Konfigurasi silinder: 1 silinder, kemiringan sekitar 80 derajat dari vertikal
- Rasio kompresi: `9.3:1`
- Pelumasan: wet sump, pompa trochoid

### Tuning Notes

- Mesin low compression dengan airflow relatif kecil.
- Respons bawah-menengah umumnya dominan untuk penggunaan real.
- Hindari pendekatan timing agresif yang tidak progresif.

## 2) Valve Train

- Intake open: `5 deg` sebelum TMA
- Intake close: `30 deg` setelah TMB
- Exhaust open: `34 deg` sebelum TMB
- Exhaust close: `0 deg` setelah TMA
- Valve clearance intake: `0.10 mm`
- Valve clearance exhaust: `0.15 mm`

### Tuning Notes

- Durasi cam cenderung pendek, overlap kecil.
- Mesin cenderung nyaman di low-mid RPM.
- Advance ignition bisa dinaikkan bertahap selama tidak terjadi knocking.

## 3) Piston and Ring

- Diameter silinder: `50.005 - 50.015 mm`
- Diameter piston: `49.980 - 49.995 mm`
- Piston clearance: `0.010 - 0.035 mm`
- Ring gap 1: `0.10 - 0.25 mm`
- Ring gap 2: `0.10 - 0.25 mm`
- Ring oli gap: `0.20 - 0.70 mm`

### Tuning Notes

- Hindari AFR terlalu rich di low RPM (risiko fuel dilution ke oli).
- Hindari over-advance yang memicu detonasi dini.

## 4) Fuel System

- Throttle body: `22 mm`
- Fuel pressure: `263 - 316 kPa`
- Injector resistance: `11 - 13 ohm` (20 deg C)
- Fuel pump minimum: `82 ml / 10 s`
- Idle speed target: `1400 +/- 100 rpm`
- Idle air screw baseline: `2 putaran` dari posisi duduk

### Tuning Notes

- Target duty injector WOT disarankan `<= 70%`.
- Target AFR WOT umum: `13.0 - 13.2`.
- Kontrol idle tidak sepenuhnya ECU-based karena ada faktor mekanis.

## 5) Ignition and Electrical

- Sistem pengapian: full transistorized
- Busi standar pabrikan: `CPR6EA-9S`
- Gap busi: `0.80 - 0.90 mm`
- CKP sensor minimum: `1.5 V`
- Base idle ignition (referensi servis): sekitar `5 deg BTDC`

### Tuning Notes

- Mid RPM sering masih bisa dimajukan `+2` sampai `+4` derajat jika bahan bakar dan temperatur aman.
- Untuk busi lebih dingin (mis. CPR7EA), tetap jaga kestabilan pembakaran area bawah.

## 6) Transmission

- Tipe: 4-speed rotary
- Primary reduction: `4.059`
- Final reduction: `2.642`
- Gear 1: `2.615`
- Gear 2: `1.555`
- Gear 3: `1.136`
- Gear 4: `0.916`

### Tuning Notes

- Untuk mesin standar, area efektif umumnya low-mid sampai sekitar 7200 rpm.
- Shift feel natural biasanya sekitar 7500-8000 rpm.
- Limiter operasional umumnya masih di sekitar 9000 rpm.

## 7) Oil Capacity

- Periodik: `800 ml`
- Overhaul: `1.0 L`
- Viskositas referensi umum: `10W-30`

## 8) Normalization Rules

1. Satuan default dianggap SI kecuali disebutkan lain.
2. Rentang `a-b` diperlakukan sebagai minimum-maksimum inklusif.
3. Simbol `+/-` berarti toleransi terhadap nilai tengah.
4. Nilai berlabel `minimum` diperlakukan sebagai batas bawah validasi.
