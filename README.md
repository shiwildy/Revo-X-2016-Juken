# Revo X 2016 — Juken 5++ Tuning

Repository tuning map ECU Juken 5++ Dualband untuk Honda Revo FI / Revo X 2016
dengan konfigurasi K15-601 (CBR150R genuine injector) + racing TPS.

## Informasi Motor

- Tipe: Honda Revo FI / Revo X 2016
- Mesin: 4-tak, SOHC, 2-klep, pendingin udara, 109.17 cc
- Bore × stroke: `50.000 × 55.597 mm`
- Rasio kompresi: `9.3:1`
- Silinder: 1, miring ~80° dari vertikal
- Sistem bahan bakar: PGM-FI
- Transmisi: 4-speed rotary

## Setup Aktual (2026-05-12)

### Fuel & Ignition
- ECU: Juken 5++ Dualband
- Injector: **K15-601 genuine** (CBR150R 2019-2021, part `16450-K15-601`, ~215 cc/min @ 294 kPa)
- Throttle body: 22mm bawaan
- TPS: racing (terkalibrasi di Juken)
- Filter: Daytona regular (paper premium, flow match stock AHM)
- Busi: `CPR7EA` iridium, gap `0.80-0.90 mm`
- Bahan bakar: RON 92/95 harian, RON 95-98 untuk core2 (power profile)

### Electrical & Lubrication
- Kelistrikan: DC fullwave
- Oli: Mobil Delvac Modern 15W-40
- Dwell: 3.5 ms flat (Juken default, stock coil)

### Engine Internal
- Piston, ring, klep, head, block: STOCK (belum dibore/ported)
- Clearance, valve timing: stock service manual

## Struktur Repository

```
.
├── 12-05-2026/           # Map aktif K15-601
│   ├── core1.txt         # Profile harian halus (FC 0..5, IGN cap 26°)
│   ├── core2.txt         # Profile tarik agresif (FC 0..7, IGN cap 28°)
│   ├── juken.txt         # WARMUP, STARTER, JET FUEL parameters
│   └── README.md         # Dokumentasi detail map + audit metrics
├── agents/               # Reference dokumen untuk tuning agent
│   ├── CONDITION.md      # Hardware state + empirical findings
│   ├── INSTRUCTION.md    # Rules untuk parsing & edit map
│   ├── REFERENCE_AXIS.md # Axis contract (TPS × RPM)
│   ├── REFERENCE_DETAIL.md  # Engine spec + tuning constraints
│   └── REFERENCE_JRJX_DECODE.md  # Decode .Jrjx format
└── template/             # Template blank map
```

## Catatan

- Tuning dilakukan collaborative dengan Agentic AI (lihat `agents/`)
- Semua map output dalam format plain text tab-separated
- Matrix contract: TPS 21 titik × RPM 33 titik (fuel tables) atau 17 titik (ignition)
