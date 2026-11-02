# smelter-performance-dashboard
Power BI dashboard on synthetic smelter data: output vs target, downtime, and quality
# Smelter Performance Review: Output, Downtime dan Mutu

> \\\*\\\*Dataset sintetis.\\\*\\\* Seluruh data dibuat acak untuk latihan. Ini bukan data PT Amman Mineral atau perusahaan mana pun, dan proyek ini tidak berafiliasi dengan perusahaan tersebut.

Studi kasus pelaporan operasi smelter tembaga (1 Jan–30 Sep 2026): dashboard Power BI 4 halaman yang menjawab apakah output memenuhi target, apa penyebab selisihnya, dan di mana risiko operasional serta mutu.

!\[Ringkasan](docs/img/01\_ringkasan.png)

## Ringkasan untuk manajemen

Output katoda 156.326 t, 2,5% di atas target prorata (152.581 t), tetapi 3 dari 9 bulan meleset (April, Juni, Juli). Pendorong utama adalah jam operasi (availability) dan kadar umpan, bukan recovery (stabil sekitar 98,1%). Lima penyebab gangguan tidak terjadwal teratas menyumbang 61% jam gangguan, dengan trip kompresor udara sebagai yang terbesar. Mutu katoda 94,5% Grade A; semua pelanggaran Pb dan S berupa lonjakan tunggal. Simulasi: menekan jam gangguan top 5 sebesar 50% menambah sekitar 1.363 t katoda.

## Pertanyaan bisnis

1. Apakah output memenuhi target, dan bulan mana yang meleset?
2. Faktor apa yang menjelaskan selisihnya (jam operasi, laju umpan, kadar, recovery)?
3. Penyebab downtime mana yang layak diprioritaskan?
4. Apakah ada hari dengan recovery yang tidak wajar?
5. Apakah mutu katoda memenuhi spesifikasi, dan adakah tanda peringatan dini?

## Data

|File|Isi|Baris|
|-|-|-|
|`data/Data\\\_Harian.csv`|Produksi, kadar, jam downtime, energi per hari|273|
|`data/Downtime\\\_Log.csv`|Kejadian berhenti per unit dan penyebab|73|
|`data/QC\\\_Lab.csv`|Hasil lab katoda per batch (Cu, Pb, S, Fe)|273|

Kamus data lengkap: [docs/data\_dictionary.md](docs/data_dictionary.md).

## Metode

* **Target prorata:** 204.000 t/tahun dibagi 365, dikalikan jumlah hari (asumsi; bukan target resmi). Target flat bulanan menyesatkan karena panjang bulan berbeda.
* **Jembatan selisih:** substitusi bertahap terhadap rencana (umpan 2.400 DMT/hari, kadar Cu 25,5%, recovery 98,0%) dengan urutan availability, laju umpan, kadar, recovery.
* **Anomali recovery:** median − 3 × 1,4826 × MAD.
* **Kuadran bad actor:** frekuensi kejadian vs rata-rata durasi (MTTR) per penyebab.
* **Control chart Pb dan S:** batas spesifikasi (Pb ≤ 5 ppm, S ≤ 15 ppm, disederhanakan) dan peringatan 80%.
* **Simulasi what-if:** jam dikurangi × t katoda per jam operasi × harga (placeholder).

## Temuan utama

|Temuan|Angka|
|-|-|
|Output vs target prorata|+3.745 t (+2,5%)|
|Efek availability / laju / kadar / recovery|+2.919 / −754 / +1.342 / +237 t|
|April meleset karena availability|−1.336 t|
|Hari recovery anomali|5 hari (90–92%)|
|Total jam downtime|329,5 jam (177,5 tidak terjadwal)|
|Penyebab terbesar|Trip kompresor udara: 7 kejadian, 34 jam|
|Batch Grade A|94,5% (258 dari 273)|
|Energi per ton saat downtime > 8 jam|+11,9% (n = 12 hari)|

## Rekomendasi

1. Analisis akar masalah trip kompresor udara (sering dan lama).
2. Siapkan material dan kru untuk penggantian refractory agar lebih cepat.
3. Rekonsiliasi metal accounting pada 5 hari recovery anomali.
4. Setiap titik Pb atau S di atas spesifikasi memicu penahanan batch dan penelusuran sumber.

## Batasan

* Data sintetis; beberapa pola (misalnya energi saat downtime) sebagian dibentuk oleh cara data dibuat.
* 58 kejadian tidak terjadwal: selisih kecil antar penyebab belum signifikan secara statistik.
* Katoda dianggap terbentuk di hari yang sama dengan umpan.
* Kriteria Grade A disederhanakan; harga Cu dan hasil what-if adalah asumsi (pendapatan kotor, bukan laba).

## Dashboard

|Halaman|Isi|
|-|-|
|Ringkasan|KPI, target vs aktual, jembatan, simulasi|
|Diagnostik|Downtime per unit, Pareto, kuadran, recovery harian|
|Mutu dan efisiensi|Grid batch, control chart Pb dan S, energi vs downtime|
|Model dan metode|Model data, measure, asumsi|

PDF: [docs/dashboard.pdf](docs/dashboard.pdf)

## Cara membuka

1. Unduh `powerbi/Smelter\\\_Performance\\\_Review.pbix` (Power BI Desktop, versi terbaru).
2. Jika sumber data meminta lokasi, arahkan ke folder `data/`.

## Struktur repositori

```
smelter-performance-dashboard/
├── README.md
├── LICENSE
├── data/            # 3 file CSV
├── powerbi/         # file .pbix
├── excel/           # workbook latihan
└── docs/
    ├── img/         # screenshot dashboard
    ├── dashboard.pdf
    ├── measures.md  # daftar DAX
    └── data\\\_dictionary.md
```

## Kontak

Wahyu Akbar · http://www.linkedin.com/in/wahyu-akbar-05a16424b · @akbarparsan@gmail.com

