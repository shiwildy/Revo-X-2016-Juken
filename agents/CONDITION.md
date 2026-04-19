# Current Vehicle Condition

Dokumen ini mencatat konfigurasi motor aktual yang menjadi konteks tuning.

## Platform

- Model basis: Honda Revo FI 110
- ECU: Juken (fully configurable)
- Engine internal: standar (piston, klep, noken, kopling belum diubah)

## Fuel and Ignition Related Setup

- Injector: tipe KZR / Vario 125 (karakter debit lebih besar dari injektor bawaan Revo FI)
- Bahan bakar harian: RON tinggi (umumnya RON 95)
- Busi terpasang saat ini: `CPR7EA` iridium

## Lubrication and Electrical

- Oli: Mobil Delvac Modern 15W-40
- Kelistrikan: DC fullwave (suplai listrik lebih stabil)

## Tuning Implication

1. Karena debit injector lebih besar, fuel correction perlu dijaga agar tidak over-rich di low RPM.
2. Karena mesin basic masih standar dan kompresi rendah, timing harus progresif dan natural, terutama area idle sampai mid RPM.
3. Dengan RON tinggi, advance mid RPM masih bisa dimanfaatkan, tetapi tetap hindari over-advance di beban tinggi.
4. Fokus utama tuning: halus untuk harian (`core1`) dan responsif untuk tarik (`core2`) tanpa mengorbankan reliability.
