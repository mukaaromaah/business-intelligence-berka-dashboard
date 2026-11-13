# Business Intelligence Dashboard - Berka Bank Dataset

Tugas Akhir Mata Kuliah Business Intelligence  
Program Studi S1 Teknik Informatika, Universitas Negeri Malang  
Pengembang: Mukaromah dan Sevira

---

## 1. Deskripsi Proyek

Proyek ini bertujuan untuk membangun dashboard Business Intelligence (BI) berbasis web statis (HTML, CSS, dan JavaScript murni tanpa server) yang di-deploy ke GitHub Pages. Dashboard ini mengolah dan memvisualisasikan data transaksi perbankan historis dari **Berka Dataset** (bank di Republik Ceko, periode 1993 - 1998).

Cakupan materi BI yang diimplementasikan:
- **Key Performance Question (KPQ) ke Key Performance Indicator (KPI)**: Menjawab kebutuhan informasi strategis manajemen bank.
- **OLTP vs OLAP**: Transformasi dari struktur transaksional relasional menjadi model analitis (OLAP Cube).
- **Extract, Transform, Load (ETL)**: Pembersihan data, standarisasi tanggal, penanganan data yatim (orphan data), dan pengayaan data.
- **Data Warehouse (Star Schema)**: Pemodelan dimensional yang terdiri dari tabel fakta dan dimensi dengan surrogate key.
- **Data Quality and Governance**: Dokumentasi profil kualitas data, isu kelengkapan, konsistensi, validitas, serta audit perbaikan data.
- **Customer Segmentation (RFM)**: Segmentasi nasabah berdasarkan Recency, Frequency, dan Monetary.
- **Data Mining / Predictive Modeling**: Klasifikasi risiko gagal bayar pinjaman (loan status) menggunakan Decision Tree.

---

## 2. Kontrak Data (Output ETL di Folder `data/`)

Dashboard membaca data agregasi berukuran ringan secara asinkron (fetch) melalui path relatif:

| Nama File | Format | Deskripsi Isi |
|---|---|---|
| `kpi_summary.json` | JSON | Ringkasan metrik utama: total rekening, klien, transaksi, total uang masuk/keluar, rata-rata saldo, dan persentase pinjaman bermasalah |
| `monthly_trend.csv` | CSV | Tren bulanan: tahun-bulan, jumlah transaksi, volume uang masuk/keluar, dan rata-rata saldo |
| `olap_cube.csv` | CSV | Kubus data OLAP multi-dimensi: tahun, kuartal, wilayah, gender nasabah, tipe transaksi, frekuensi, dan total nominal |
| `rfm_segmen.csv` | CSV | Agregat segmentasi nasabah (Champions, Loyal, At-Risk, Low Value / New) beserta metrik rata-rata R, F, M |
| `rfm_sampel.csv` | CSV | Sampel representatif data nasabah untuk eksplorasi sebaran skor RFM |
| `loan_model.json` | JSON | Hasil evaluasi model Decision Tree: akurasi, confusion matrix, feature importance, dan aturan keputusan (rules) |
| `data_quality.json` | JSON | Laporan dimensi kualitas data sebelum dan sesudah data cleansing |

---

## 3. Pembagian Tugas

Proyek ini mengintegrasikan dua alur kerja utama:

### A. Jalur Data (ETL and Modeling)
- Profiling data mentah dan dokumentasi isu kualitas data (`etl/01_profiling.py`).
- Cleansing data, penanganan format tanggal YYMMDD, penerjemahan kode bahasa Ceko, dan pembuatan Star Schema (`etl/02_star_schema.py`).
- Perhitungan kuantil dan segmentasi nasabah RFM (`etl/03_rfm.py`).
- Pelatihan model prediktif Decision Tree pinjaman bermasalah (`etl/04_loan_model.py`).
- Ekspor seluruh file agregasi ke folder `data/` sesuai kontrak data.

### B. Jalur Dashboard (Frontend and Visualization)
- Struktur antarmuka web statis responsive (`index.html`, `css/style.css`).
- Logika navigasi 6 halaman utama:
  1. Overview KPI
  2. Eksplorasi OLAP (drill-down, roll-up, slicing)
  3. Segmentasi RFM
  4. Prediksi Pinjaman
  5. Kualitas Data (sebelum vs sesudah cleansing)
  6. Arsitektur BI (arsitektur OLTP, ETL, DW, Star Schema)
- Integrasi visualisasi interaktif menggunakan pustaka Plotly.js via CDN.
- Pengujian lokal dan konfigurasi deployment GitHub Pages pada branch `main`.

---

## 4. Struktur Direktori

```text
berka-dashboard/
├── .gitignore             # Mengabaikan data mentah, warehouse, venv, cache
├── README.md              # Dokumentasi proyek, kontrak data, pembagian tugas
├── index.html             # Halaman utama aplikasi web dashboard
├── berka/                 # Data mentah 8 berkas .asc (diabaikan git)
├── etl/                   # Skrip Python pemrosesan data dan machine learning
├── warehouse/             # Tempat penyimpanan lokal star schema (diabaikan git)
├── data/                  # File agregat JSON & CSV hasil ETL untuk dashboard
├── css/                   # Berkas penataan gaya (CSS)
└── js/                    # Logika interaktivitas dan visualisasi (JavaScript)
```
