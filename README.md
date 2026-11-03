# Mexico Real Estate: Analisis Harga Properti

Proyek analisis data harga properti (rumah dan apartemen) di Meksiko menggunakan Python (pandas, matplotlib, seaborn). Proyek terdiri dari 4 notebook yang dijalankan berurutan.

## Dataset

Tiga file CSV mentah (`mexico-real-estate-1.csv`, `-2.csv`, `-3.csv`) dibersihkan dan digabung menjadi `mexico-real-estate-combined-clean.csv`.

| Kolom | Keterangan |
|---|---|
| `property_type` | Tipe properti (`house` / `apartment`) |
| `state` | Negara bagian |
| `lat`, `lon` | Koordinat lokasi |
| `area_m2` | Luas bangunan (m²) |
| `price_usd` | Harga dalam USD |

Hasil akhir: **1.736 baris, 6 kolom, tanpa nilai kosong**.

## Alur Notebook

### 1. Data Wrangling (pembersihan dan penggabungan)
- Dataset 1: hapus baris kosong, bersihkan simbol `$` dan `,` pada harga lalu ubah ke angka (583 baris).
- Dataset 2: konversi harga peso ke USD dengan asumsi kurs 19 MXN/USD (kurs 2014), hapus baris kosong (571 baris).
- Dataset 3: pisahkan `lat-lon` menjadi `lat` dan `lon`, ambil `state` dari `place_with_parent_names` (582 baris).
- Ketiganya digabung dengan `pd.concat` dan disimpan sebagai CSV bersih.
- Rata-rata harga 115.332 USD (median 99.262 USD), rata-rata luas 170 m².

### 2. Visualisasi dan Eksplorasi Data
- Histogram harga dan luas, boxplot per tipe properti dan per negara bagian (3 teratas), serta scatter plot luas vs harga.
- Pencilan dibuang (harga di luar persentil 5 sampai 90, luas di atas persentil 90): 1.736 menjadi **1.365 baris**.
- Garis tren (seaborn `regplot`) dan rata-rata harga per negara bagian.

### 3. Analisis Korelasi
- Korelasi luas dan harga secara keseluruhan: **0,59** (positif sedang). Koordinat (lat/lon) hampir tidak berkorelasi dengan harga.
- Korelasi per tipe properti: rumah **0,72**, apartemen **0,52**.
- Korelasi per negara bagian bervariasi. Nilai ekstrem (misalnya Zacatecas 1,00 dan Oaxaca -1,00) kemungkinan karena datanya sedikit.

### 4. Harga per Meter Persegi
- Fitur baru `price_per_m2 = price_usd / area_m2`: rata-rata 733 USD/m², median 614 USD/m².
- Termahal: Distrito Federal (median 959 USD/m²). Termurah: Colima (median 334 USD/m²).
- Variasi harga lebih banyak terjadi di dalam satu negara bagian (sekitar 78%) daripada antar negara bagian (sekitar 12%).
- Per kombinasi negara bagian dan tipe, apartemen mendominasi posisi termahal (Nuevo León apartemen 1.425 USD/m²).

## Kesimpulan

Luas bangunan adalah faktor terkuat yang berhubungan dengan harga, dan hubungannya lebih kuat pada rumah daripada apartemen. Lokasi (negara bagian) dan tipe properti juga memengaruhi harga per m², tetapi variasi di dalam satu negara bagian tetap besar. Faktor luas, tipe, dan lokasi layak dipakai sebagai fitur untuk model prediksi harga.

## Catatan dan Keterbatasan
- Harga pada dataset 2 memakai kurs tetap 19 MXN/USD, sehingga bersifat perkiraan.
- Pembuangan pencilan (sekitar 21% data) memengaruhi pola yang terlihat pada grafik.
- Hasil per negara bagian dengan jumlah data kecil tidak boleh disimpulkan terlalu kuat.

## Cara Menjalankan

```bash
pip install pandas matplotlib seaborn
```

Jalankan notebook secara berurutan (1 sampai 4). Pastikan file CSV mentah berada di folder `data/`.
