# Software Requirement Specification (SRS)
## Web Application for Diabetes Prediction Using Machine Learning

## 1. Pendahuluan

### 1.1 Tujuan Dokumen
Dokumen ini menjelaskan kebutuhan fungsional dan non-fungsional dari aplikasi web deteksi diabetes berbasis machine learning, sebagai acuan pemahaman sistem bagi pengembang maupun pihak lain yang ingin melanjutkan pengembangan project ini.

### 1.2 Ruang Lingkup Project
Aplikasi ini adalah aplikasi web single-page berbasis Streamlit yang memprediksi kemungkinan seseorang menderita diabetes berdasarkan 8 parameter kesehatan, menggunakan model `RandomForestClassifier` yang dilatih dari dataset **Pima Indians Diabetes**. Aplikasi berjalan sebagai satu script Python (`WebApp.py`) tanpa backend/frontend terpisah, tanpa database, dan tanpa sistem autentikasi.

### 1.3 Definisi & Istilah
| Istilah | Penjelasan |
|---|---|
| Streamlit | Framework Python untuk membangun aplikasi web data science secara cepat |
| RandomForestClassifier | Algoritma machine learning berbasis ensemble decision tree, digunakan untuk klasifikasi |
| Outcome | Label pada dataset: `0` = tidak diabetes, `1` = diabetes |
| DPF | Diabetes Pedigree Function, skor riwayat diabetes keluarga |
| Rerun | Perilaku Streamlit yang menjalankan ulang seluruh script setiap kali ada interaksi pengguna (mis. slider digeser) |

## 2. Deskripsi Umum Sistem

### 2.1 Perspektif Produk
Aplikasi berdiri sendiri (standalone), tidak terintegrasi dengan sistem lain, dan ditujukan sebagai alat bantu edukatif/demonstratif untuk memahami penerapan machine learning dalam deteksi dini diabetes — bukan alat diagnosis medis resmi.

### 2.2 Fungsi Utama Sistem
1. Menampilkan dataset yang digunakan beserta statistik deskriptifnya.
2. Menerima input parameter kesehatan dari pengguna.
3. Melatih model klasifikasi dari dataset.
4. Menampilkan skor akurasi model.
5. Menampilkan hasil klasifikasi/prediksi terhadap input pengguna.

### 2.3 Karakteristik Pengguna
Pengguna umum (mahasiswa, dosen, atau siapa pun yang tertarik dengan demonstrasi machine learning) tanpa perlu keahlian teknis khusus. Tidak ada perbedaan peran/role pengguna — semua pengguna memiliki akses yang sama terhadap seluruh fitur.

### 2.4 Batasan Umum
- Tidak ada sistem login/autentikasi.
- Tidak ada penyimpanan riwayat prediksi (data hanya berlaku selama sesi browser berjalan).
- Model tidak disimpan (tidak ada file `.pkl`/`.joblib`); model dilatih ulang dari nol setiap kali script dijalankan ulang oleh Streamlit.
- Aplikasi hanya memiliki satu halaman (tidak ada multi-page routing).

## 3. Kebutuhan Fungsional

| ID | Kebutuhan | Deskripsi |
|---|---|---|
| RF-01 | Menampilkan data mentah | Sistem menampilkan seluruh isi `diabetes.csv` dalam bentuk tabel. |
| RF-02 | Menampilkan statistik data | Sistem menampilkan ringkasan statistik (`count`, `mean`, `std`, `min`, `max`, dsb.) dari dataset. |
| RF-03 | Menampilkan visualisasi data | Sistem menampilkan bar chart dari seluruh kolom dataset. |
| RF-04 | Input parameter kesehatan | Sistem menyediakan 8 slider di sidebar untuk input: pregnancies, glucose, blood_pressure, skin_thickness, insulin, BMI, DPF, dan age, masing-masing dengan rentang nilai dan default sesuai karakteristik dataset. |
| RF-05 | Menampilkan input pengguna | Sistem menampilkan kembali nilai yang telah dimasukkan pengguna dalam bentuk tabel sebagai konfirmasi. |
| RF-06 | Pelatihan model | Sistem membagi dataset menjadi data latih (75%) dan data uji (25%), lalu melatih `RandomForestClassifier` (dengan `random_state` tetap) pada data latih. Proses load data dan training di-cache (`st.cache_data`/`st.cache_resource`) sehingga hanya dijalankan sekali per proses server, bukan pada setiap interaksi pengguna. |
| RF-07 | Menampilkan akurasi model | Sistem menghitung dan menampilkan akurasi model terhadap data uji. |
| RF-08 | Klasifikasi/prediksi | Sistem menjalankan prediksi model terhadap data yang dimasukkan pengguna dan menampilkan hasilnya sebagai label yang mudah dipahami ("Terindikasi Diabetes" / "Tidak Terindikasi Diabetes") beserta skor probabilitasnya. |

## 4. Kebutuhan Non-Fungsional

| Kategori | Kebutuhan |
|---|---|
| Performa | Load dataset dan pelatihan model di-cache (`st.cache_data`/`st.cache_resource`), sehingga hanya dijalankan sekali per proses server — interaksi pengguna berikutnya (menggeser slider) tidak lagi memicu pelatihan ulang, hanya menjalankan ulang langkah input & prediksi. |
| Usability | Antarmuka harus tetap sederhana dan dapat digunakan tanpa panduan khusus, memanfaatkan komponen bawaan Streamlit (slider, tabel, chart). |
| Portabilitas | Aplikasi harus dapat dijalankan di lingkungan mana pun yang memiliki Python dan dependency pada `requirements.txt` terinstall, tanpa konfigurasi tambahan (database, environment variable, dsb.). |
| Kompatibilitas | Aplikasi harus kompatibel dengan versi package yang tertera di `requirements.txt` (Streamlit 1.32.0, scikit-learn 1.2.2, Pillow 9.5.0, pandas 2.3.3). |
| Keamanan & Privasi Data | Tidak ada kebutuhan khusus karena aplikasi tidak menyimpan data pengguna secara permanen — seluruh input hanya berada di memori sesi browser. |

## 5. Kebutuhan Data

- **Sumber data**: dataset Pima Indians Diabetes (`diabetes.csv`), disimpan langsung dalam repository.
- **Volume**: 768 baris data.
- **Fitur (independent variable)**: `Pregnancies`, `Glucose`, `BloodPressure`, `SkinThickness`, `Insulin`, `BMI`, `DiabetesPedigreeFunction`, `Age` (8 kolom).
- **Label (dependent variable)**: `Outcome` (0 = tidak diabetes, 1 = diabetes).
- Dataset harus tersedia di direktori project yang sama dengan `WebApp.py` agar dapat dimuat oleh aplikasi.

## 6. Known Issues / Batasan Teknis Saat Ini

### 6.1 Sudah Diperbaiki

Poin-poin berikut sebelumnya tercatat sebagai gap teknis dan **sudah diperbaiki**:

1. ~~Dependency `pandas` belum ter-pin di `requirements.txt`~~ — sudah ditambahkan (`pandas==2.3.3`).
2. ~~Model tidak diberi `random_state`~~ — `RandomForestClassifier` kini memakai `random_state=0`, sehingga akurasi konsisten antar reload.
3. ~~Model dilatih ulang dari nol pada setiap interaksi pengguna~~ — load dataset dan training model kini di-cache masing-masing dengan `st.cache_data` dan `st.cache_resource` (mensyaratkan `streamlit>=1.18`, di-pin ke `1.32.0`), sehingga hanya dijalankan sekali per proses server.
4. ~~Hasil prediksi ditampilkan dalam bentuk mentah (`0`/`1`)~~ — kini ditampilkan sebagai label ramah pengguna ("Terindikasi Diabetes" / "Tidak Terindikasi Diabetes") disertai skor probabilitas dari `predict_proba`.

### 6.2 Masih Berlaku

Bagian ini mencatat gap teknis yang **masih ada** pada kondisi kode saat ini, sebagai referensi untuk pengembangan lanjutan:

1. **Model tidak dipersist ke file di disk** (mis. `.pkl`/`.joblib`). `st.cache_resource` hanya menyimpan model di memori selama proses server Streamlit berjalan — begitu server di-restart, model dilatih ulang sekali dari awal.
2. **Tidak ada file `LICENSE`** pada repository.
3. **Tidak ada automated test maupun CI/CD** untuk memvalidasi perubahan kode.

Rekomendasi perbaikan untuk poin-poin di atas dijelaskan lebih lanjut pada penutup [`Readme.md`](../Readme.md).
