# Daftar Fitur

Dokumen ini merinci seluruh fitur yang tersedia saat ini pada aplikasi (`WebApp.py`), beserta cara kerjanya.

## 1. Tampilan Informasi Dataset

**Deskripsi**: Menampilkan dataset `diabetes.csv` yang digunakan untuk melatih model.

**Cara kerja**:
- Dataset dibaca menggunakan `pandas.read_csv()`, dibungkus fungsi yang di-cache dengan `st.cache_data` sehingga file hanya dibaca dari disk sekali per proses server.
- Ditampilkan sebagai tabel interaktif (`st.dataframe(df)`).
- Statistik deskriptif (count, mean, std, min, max, kuartil) ditampilkan melalui `df.describe()`.
- Seluruh kolom dataset divisualisasikan sekaligus dalam satu bar chart (`st.bar_chart(df)`).

## 2. Input Parameter Kesehatan Pengguna

**Deskripsi**: Pengguna memasukkan data kesehatan melalui 8 slider di sidebar untuk digunakan sebagai input prediksi.

**Daftar slider**:

| Parameter | Rentang | Default |
|---|---|---|
| pregnancies | 0 – 17 | 3 |
| glucose | 0 – 199 | 117 |
| blood_pressure | 0 – 122 | 72 |
| skin_thickness | 0 – 99 | 23 |
| insulin | 0.0 – 846.0 | 30.0 |
| BMI | 0.0 – 67.1 | 32.0 |
| DPF (Diabetes Pedigree Function) | 0.078 – 2.42 | 0.3725 |
| age | 21 – 81 | 29 |

**Cara kerja**: Nilai dari seluruh slider dikumpulkan ke dalam dictionary lalu dikonversi menjadi `pandas.DataFrame` satu baris yang digunakan sebagai input prediksi model.

## 3. Konfirmasi Input Pengguna

**Deskripsi**: Menampilkan kembali nilai-nilai yang sudah dimasukkan pengguna dalam bentuk tabel, sebagai konfirmasi sebelum melihat hasil prediksi.

## 4. Pelatihan Model & Skor Akurasi

**Deskripsi**: Model `RandomForestClassifier` dilatih menggunakan dataset yang sudah dibagi menjadi data latih (75%) dan data uji (25%) melalui `train_test_split`.

**Cara kerja**:
- Fitur (`X`) diambil dari 8 kolom pertama dataset, label (`Y`) dari kolom `Outcome`.
- Model (`RandomForestClassifier(random_state=0)`) dilatih (`fit`) pada data latih di dalam fungsi yang dibungkus `st.cache_resource`, sehingga pelatihan hanya berjalan sekali per proses server — interaksi pengguna berikutnya (menggeser slider) tidak memicu pelatihan ulang.
- Akurasi dihitung menggunakan `accuracy_score` terhadap data uji dan ditampilkan sebagai persentase. Karena model memakai `random_state` tetap, skor akurasi konsisten antar reload selama dataset sama.

## 5. Klasifikasi/Prediksi Diabetes

**Deskripsi**: Fitur inti aplikasi — memprediksi apakah data kesehatan yang dimasukkan pengguna mengindikasikan diabetes.

**Cara kerja**: Model yang sudah dilatih menjalankan `predict()` dan `predict_proba()` terhadap data input pengguna. Hasilnya diterjemahkan menjadi label yang mudah dipahami — "Terindikasi Diabetes" atau "Tidak Terindikasi Diabetes" — beserta skor probabilitasnya (mis. "Terindikasi Diabetes (probabilitas: 78.00%)").

## 6. Tampilan Dark Mode

**Deskripsi**: Aplikasi menggunakan tema gelap secara default, dikonfigurasi melalui `.streamlit/config.toml` (`theme = "dark"`).

## 7. Banner/Identitas Visual

**Deskripsi**: Aplikasi menampilkan gambar banner (`jti.jpg`) di bagian atas halaman sebagai identitas visual/institusi.

---

## Fitur yang Belum Tersedia

Bagian ini mencatat fitur yang **belum** ada pada aplikasi saat ini, sebagai referensi untuk pengembangan lanjutan:

- Riwayat/penyimpanan hasil prediksi pengguna (saat ini tidak disimpan sama sekali).
- Sistem autentikasi/login pengguna.
- Model yang dipersist ke file di disk (mis. `.pkl`/`.joblib`). Saat ini model di-cache di memori selama proses server berjalan (`st.cache_resource`), tetapi tetap dilatih ulang sekali setiap kali server di-restart.
- Multi-halaman atau navigasi antar-page.
- Unggah dataset kustom oleh pengguna (dataset saat ini statis/tetap).
