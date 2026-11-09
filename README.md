# SARIMA vs LSTM: Peramalan Penolakan Donor Darah Berdasarkan Kadar Hemoglobin

Perbandingan model SARIMA dan Long Short-Term Memory (LSTM) untuk meramalkan jumlah harian calon pendonor yang ditolak karena kadar hemoglobin (HB) tidak memenuhi syarat di UDD PMI Kota Surabaya.

Proyek ini dikerjakan untuk mata kuliah **Analisis Deret Waktu**, Program Studi Sains Data, UPN "Veteran" Jawa Timur.

## Latar Belakang

Jumlah penolakan donor akibat HB berubah dari hari ke hari dan tidak konstan sepanjang waktu. Dengan meramalkannya, pihak UDD dapat memperkirakan beban kerja dan kebutuhan calon pendonor. Proyek ini menguji apakah model deep learning (LSTM) mampu menangkap pola data lebih baik daripada model statistik klasik (SARIMA).

## Data

| Keterangan | Isi |
|---|---|
| Sumber | Data operasional UDD PMI Kota Surabaya |
| Periode | 1 Januari – 31 Agustus 2026 |
| Frekuensi | Harian (243 observasi, tanpa nilai kosong) |
| Kolom | `TglPeriksa` (tanggal), `Jumlah_Pendonor` (jumlah pendonor ditolak karena HB) |

> Catatan: data digunakan dengan izin dari UDD PMI Kota Surabaya untuk keperluan tugas perkuliahan. Data berupa jumlah agregat harian dan tidak memuat identitas pendonor.

## Status Pengerjaan

| Tahap | Isi | Status |
|---|---|---|
| Minggu 1 | Pengumpulan data dan pembuktian non-stasioneritas | Selesai |
| Berikutnya | Pembagian data latih/uji dan pembangunan model SARIMA |
| Berikutnya | Pembangunan model LSTM | 
| Berikutnya | Evaluasi dan perbandingan model |

## Ringkasan Temuan Minggu 1

Data **non-stasioner** dan memiliki **pola musiman mingguan**.

- **Rata-rata tidak konstan.** Rata-rata bulanan turun dari sekitar 53 (Januari) ke sekitar 37 (Maret), lalu naik ke sekitar 64 (Mei).
- **Dekomposisi STL** (periode 7 hari) menunjukkan tren yang berubah dan komponen musiman yang berulang teratur.
- **ACF** signifikan pada kelipatan lag 7 (7, 14, 21, 28, 35), dengan nilai tertinggi sekitar 0,43 pada lag 7.
- **Uji formal:**

| Uji | Statistik | p-value | Kesimpulan |
|---|---|---|---|
| ADF (H0: tidak stasioner) | -1,8998 | 0,3322 | Gagal tolak H0, non-stasioner |
| KPSS (H0: stasioner) | 0,9274 | ≤ 0,01 | Tolak H0, non-stasioner |

## Rencana Pemodelan

- **SARIMA:** differencing dan penentuan orde (p, d, q)(P, D, Q) dengan s = 7.
- **LSTM:** normalisasi data (MinMax) dan windowing sesuai pola mingguan.
- **Evaluasi:** MAE dan RMSE pada data uji, dengan pembagian data berurutan menurut waktu.

## Struktur Repositori

```
.
├── README.md
├── data_tolak_hb.csv
└── 01_eksplorasi_data_dan_stasioneritas.ipynb
```

## Library

Python 3, `pandas`, `matplotlib`, `statsmodels` (STL, ACF, ADF, KPSS). Dijalankan di Google Colab.

## Penulis

Alysha Khanza Dwi Avianti — NPM: 24083010123
Program Studi Sains Data, UPN "Veteran" Jawa Timur
