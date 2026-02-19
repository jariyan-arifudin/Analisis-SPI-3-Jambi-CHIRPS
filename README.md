# Analisis Kekeringan Meteorologis (SPI-3) Provinsi Jambi

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Research-orange)

## Deskripsi Proyek

Repositori ini berisi implementasi kode Python untuk menghitung **Standardized Precipitation Index 3-Bulanan (SPI-3)** di Provinsi Jambi menggunakan data CHIRPS v3.0.

SPI-3 digunakan untuk memonitor kekeringan jangka menengah dan musiman, yang sering dikaitkan dengan penurunan volume air waduk, debit sungai, dan risiko kebakaran lahan gambut.

## Metodologi Khusus: Temporal Buffering

Perhitungan SPI-3 membutuhkan akumulasi curah hujan selama 3 bulan berturut-turut.
* Contoh: SPI bulan **Maret** membutuhkan data hujan **Januari + Februari + Maret**.

Masalah muncul pada perhitungan bulan **Januari** dan **Februari** di awal tahun, karena membutuhkan data dari tahun sebelumnya (Desember & November tahun lalu).

Untuk mengatasi hal ini, script ini menerapkan mekanisme **Temporal Buffering**:
1.  Saat memproses tahun target (misal: `2012`), script secara otomatis memuat data tahun sebelumnya (`2011`).
2.  Perhitungan SPI dilakukan untuk rentang `2011-2012` secara utuh.
3.  Hasil akhir dipotong (*slicing*) hanya untuk tahun target (`2012`).
4.  Hasilnya: Data SPI bulan Januari & Februari 2012 **tidak kosong (NaN)** dan valid secara statistik.

## Klasifikasi Kekeringan

Nilai SPI diklasifikasikan menjadi 5 kelas tingkat kekeringan berdasarkan standar BMKG/WMO:

| Nilai SPI | Kelas Kekeringan | Label |
| :--- | :--- | :--- |
| $\geq$ -1.00 | **Tidak Ada Kekeringan** | 1 (Biru) |
| -0.99 s.d 0.99 | **Mendekati Normal** | 2 (Hijau Muda) |
| -1.00 s.d -1.49 | **Agak Kering** | 3 (Kuning) |
| -1.50 s.d -1.99 | **Kering** | 4 (Oranye) |
| $\leq$ -2.00 | **Sangat Kering** | 5 (Merah) |

*> Catatan: Dalam script ini, nilai SPI positif (Basah) disederhanakan menjadi satu kelas "Tidak Ada Kekeringan" karena fokus studi adalah bencana kekeringan.*

## Struktur Direktori Data

Script ini dirancang untuk berjalan di **Google Colab** dengan struktur folder Google Drive sebagai berikut:

```text
/My Drive/Colab Notebooks/Skripsi/
├── Batas Adm Jambi/            # Shapefile Batas Provinsi
├── CHIRPS v3/
│   └── bias_corrected_regression/
│       └── downscaled_1km_metric/  # Input: Raster CHIRPS 1km
└── SPI3-Output/                # Output: Folder hasil (Otomatis dibuat)

```

## Prasyarat Instalasi

Script membutuhkan pustaka geospasial Python:

```bash
pip install rioxarray xarray geopandas rasterio scipy leafmap

```

## Cara Penggunaan

1. Pastikan data CHIRPS 1km (termasuk tahun buffer) tersedia.
2. Jalankan script `analisis_spi3.ipynb`.
3. Script akan memproses data per periode (misal: 2012-2013) dengan memuat buffer tahun sebelumnya secara otomatis.
4. Hasil peta (`.tif` dan `.shp`) tersimpan di folder `SPI3-Output`.

## Penulis

**Jariyan Arifudin** Mahasiswa Geografi Lingkungan

Universitas Gadjah Mada (UGM)

## Lisensi & Sitasi

Kode ini didistribusikan di bawah **MIT License**.
Jika Anda menggunakan metode ini untuk penelitian, silakan sitasi repositori ini:

> Arifudin, J. (2026). *Analisis Kekeringan Meteorologis (SPI-3) Provinsi Jambi*. GitHub Repository.
