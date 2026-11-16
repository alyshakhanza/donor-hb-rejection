# Perbandingan SARIMA dan LSTM dalam Peramalan Harian Penolakan Donor Darah Akibat Hemoglobin Rendah

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
| Minggu 2 | Pembagian data, baseline naif musiman, dan model SARIMA | Selesai |
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


## Ringkasan Temuan Minggu 2

Data dibagi kronologis 80:20: 194 hari latih (1 Januari – 13 Juli 2026) dan 49 hari uji (14 Juli – 31 Agustus 2026).

Model **SARIMA(1,0,0)(0,1,1)₇** dipilih berdasarkan BIC pada data latih (d = 0 dan D = 1 karena differencing musiman lag 7 sudah membuat data stasioner). Koefisien AR(1) dan MA musiman signifikan, dan residual lolos uji Ljung–Box (p > 0,05).

| Model | MAE | RMSE | MASE |
|---|---|---|---|
| Naif musiman (1 langkah) | 20,94 | 27,68 | 1,08 |
| SARIMA (1 langkah) | 15,20 | 21,00 | 0,79 |
| Naif musiman (49 langkah) | 18,65 | 24,09 | 0,97 |
| SARIMA (49 langkah) | 16,50 | 22,62 | 0,85 |

SARIMA mengungguli naif musiman pada kedua strategi prediksi, tetapi selisih pada prediksi 49 langkah tergolong kecil dan periode uji hanya 49 hari.

## Rencana Pemodelan

- **SARIMA:** differencing dan penentuan orde (orde (1,0,0)(0,1,1)₇, s = 7).
- **LSTM:** normalisasi data (MinMax) dan windowing sesuai pola mingguan.
- **Evaluasi:** MAE dan RMSE pada data uji, dengan pembagian data berurutan menurut waktu.

## Struktur Repositori

```
.
├── README.md
├── data_tolak_hb.csv
└── 01_eksplorasi_data_dan_stasioneritas.ipynb
└── Progress_Pertemuan_12_P02_pembagian_data_baseline_dan_sarima.ipynb
```

## Library

Python 3, `pandas`, `matplotlib`, `statsmodels` (STL, ACF, ADF, KPSS). Dijalankan di Google Colab.

## Penulis

Alysha Khanza Dwi Avianti — NPM: 24083010123
Program Studi Sains Data, UPN "Veteran" Jawa Timur
