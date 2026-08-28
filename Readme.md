# Web Application for Diabetes Prediction Using Machine Learning

Aplikasi web sederhana berbasis **Streamlit** untuk mendeteksi kemungkinan diabetes pada seseorang menggunakan model **Machine Learning** (Random Forest), berdasarkan dataset **Pima Indians Diabetes**.

Pengguna memasukkan data kesehatan (jumlah kehamilan, kadar glukosa, tekanan darah, dll.) melalui slider di sidebar, lalu aplikasi menampilkan hasil klasifikasi apakah pengguna terindikasi diabetes atau tidak.

## Tech Stack

| Komponen | Teknologi |
|---|---|
| UI & Server | [Streamlit](https://streamlit.io/) `1.32.0` |
| Machine Learning | [scikit-learn](https://scikit-learn.org/) `1.2.2` (`RandomForestClassifier`) |
| Pengolahan Data | [pandas](https://pandas.pydata.org/) `2.3.3` |
| Pengolahan Gambar | [Pillow](https://python-pillow.org/) `9.5.0` |
| Bahasa | Python |

## Fitur Utama

- Menampilkan dataset mentah beserta statistik deskriptifnya (`df.describe()`) dan visualisasi bar chart.
- Input data kesehatan pengguna melalui 8 slider interaktif di sidebar (pregnancies, glucose, blood pressure, skin thickness, insulin, BMI, diabetes pedigree function, age).
- Melatih model `RandomForestClassifier` (dengan `random_state` tetap) dari dataset, di-cache (`st.cache_data`/`st.cache_resource`) agar tidak dilatih ulang setiap interaksi, dan menampilkan skor akurasinya.
- Klasifikasi/prediksi apakah data yang dimasukkan pengguna terindikasi diabetes, ditampilkan sebagai label yang mudah dipahami beserta skor probabilitasnya.
- Tampilan dengan tema dark mode.

Detail lengkap fitur ada di [`docs/FEATURES.md`](docs/FEATURES.md).

## Dataset

Aplikasi ini menggunakan dataset **Pima Indians Diabetes** (`diabetes.csv`), terdiri dari 768 baris data dengan 8 fitur (`Pregnancies`, `Glucose`, `BloodPressure`, `SkinThickness`, `Insulin`, `BMI`, `DiabetesPedigreeFunction`, `Age`) dan 1 label (`Outcome`: 0 = tidak diabetes, 1 = diabetes).

## Instalasi & Menjalankan Secara Lokal

1. Clone repository ini:
   ```bash
   git clone https://github.com/almasyriqi/web_diabetes_prediction.git
   cd web_diabetes_prediction
   ```
2. (Opsional tapi disarankan) buat virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # Windows: venv\Scripts\activate
   ```
3. Install dependency:
   ```bash
   pip install -r requirements.txt
   ```
4. Jalankan aplikasi:
   ```bash
   streamlit run WebApp.py
   ```
5. Buka browser ke `http://localhost:8501`.

## Struktur Project

```
Web_Diabetes_Prediction/
├── .streamlit/
│   └── config.toml        # Konfigurasi tema Streamlit (dark mode)
├── docs/                   # Dokumentasi project
│   ├── SRS.md              # Software Requirement Specification
│   ├── FEATURES.md         # Daftar & penjelasan fitur
│   └── FLOWMAP.md          # Alur/flow map aplikasi
├── diabetes.csv            # Dataset Pima Indians Diabetes
├── jti.jpg                 # Banner/logo yang ditampilkan di halaman
├── requirements.txt        # Daftar dependency Python
├── WebApp.py                # Entry point sekaligus seluruh source code aplikasi
└── Readme.md
```

## Dokumentasi Lengkap

- [Software Requirement Specification (SRS)](docs/SRS.md)
- [Daftar Fitur](docs/FEATURES.md)
- [Flow Map Aplikasi](docs/FLOWMAP.md)

## Catatan Keterbatasan

Project ini masih berupa prototipe/tugas kuliah. Beberapa keterbatasan awal (dependency belum ter-pin, model dilatih ulang setiap interaksi, output prediksi mentah) sudah diperbaiki, namun masih ada keterbatasan lain yang berlaku (belum ada automated test/CI, belum ada file LICENSE, model tidak dipersist ke file di disk). Detail lengkap ada di bagian *Known Issues* pada [`docs/SRS.md`](docs/SRS.md#6-known-issues--batasan-teknis-saat-ini).

## Lisensi

Belum ditentukan.
