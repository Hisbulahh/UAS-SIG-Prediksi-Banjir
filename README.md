# Sistem Prediksi Daerah Rawan Banjir Berbasis SIG-AI di Provinsi Sumatera Selatan

## 1. Deskripsi Proyek

Proyek ini merupakan sebuah rancangan sistem prediksi daerah rawan banjir berbasis Sistem Informasi Geografis (SIG) dan Kecerdasan Buatan (AI). Sistem ini bertujuan untuk memprediksi tingkat kerawanan banjir di setiap kabupaten/kota di Provinsi Sumatera Selatan berdasarkan data penggunaan lahan.

Output dari sistem ini adalah sebuah model klasifikasi yang telah dilatih dan sebuah peta visual yang memetakan hasil prediksi, memberikan gambaran daerah mana yang berpotensi rawan banjir. Proyek ini dikembangkan sebagai pemenuhan tugas Ujian Akhir Semester (UAS) individu.

## 2. Model AI dan Justifikasi Pemilihan

Model machine learning yang digunakan dalam proyek ini dipilih setelah melalui tahap perbandingan untuk mendapatkan performa terbaik pada dataset yang tersedia.

* **Model yang Dibandingkan**:
    * Random Forest Classifier
    * Gradient Boosting Classifier
    * Support Vector Machine (SVM)
    * XGBoost Classifier

* **Model Terpilih dan Justifikasi**:
    (Di bagian ini, Anda jelaskan model mana yang menjadi pilihan akhir setelah perbandingan dan tuning).
    **Contoh Justifikasi**: *"`RandomForestClassifier` (atau model lain yang menjadi terbaik) dipilih karena kemampuannya yang unggul dalam menangani hubungan non-linear antar fitur penggunaan lahan. Sebagai model ensemble, ia juga lebih robust terhadap overfitting dibandingkan dengan model tunggal seperti Decision Tree. Hasil tuning hyperparameter menggunakan `GridSearchCV` juga menunjukkan bahwa model ini memberikan akurasi dan F1-score tertinggi pada data validasi.`"*

## 3. Desain Sistem Integrasi SIG-AI

Sistem ini dirancang menggunakan Python dengan library **GeoPandas** sebagai platform utama untuk integrasi data spasial dan non-spasial.

* **Data Non-Spasial**: Data tabular (`.csv`) yang berisi informasi penggunaan lahan dan riwayat banjir per kabupaten/kota.
* **Data Spasial**: Data vektor (`.shp` atau shapefile) yang berisi poligon batas administrasi setiap kabupaten/kota di Sumatera Selatan.
* **Proses Integrasi**: Data non-spasial digabungkan (di-*merge*) dengan data spasial menggunakan GeoPandas. Kunci penggabungannya adalah nama unik kabupaten/kota. Hasilnya adalah sebuah GeoDataFrame di mana setiap poligon peta (wilayah administrasi) memiliki atribut data yang lengkap (fitur penggunaan lahan dan label kerawanan banjir).

## 4. Alur Kerja Sistem

Alur kerja sistem ini dirancang secara sistematis dari pengumpulan data hingga visualisasi.

1.  **Pengumpulan & Pemuatan Data**:
    * Memuat data penggunaan lahan dari `penggunaan_lahan_sumsel.csv`.
    * Memuat data historis banjir dari `riwayat_banjir_sumsel.csv`.
    * Menggabungkan kedua dataset menjadi satu DataFrame utama.

2.  **Preprocessing & Feature Engineering**:
    * Membuat fitur-fitur baru berupa persentase luas dari setiap jenis lahan (contoh: `%_Hutan`, `%_Permukiman`) untuk menormalisasi data dan meningkatkan relevansi fitur.

3.  **Eksplorasi Data (EDA)**:
    * Menganalisis korelasi antar fitur menggunakan heatmap untuk memahami hubungan antar variabel.

4.  **Pelatihan Model**:
    * Membagi dataset menjadi data latih dan data uji (`train_test_split`).
    * Melatih beberapa model klasifikasi untuk perbandingan performa.
    * Melakukan optimasi pada model terbaik menggunakan `GridSearchCV`.

5.  **Evaluasi Model**:
    * Mengevaluasi model final menggunakan metrik yang relevan.

6.  **Visualisasi Hasil**:
    * Menggunakan model final untuk memprediksi status kerawanan pada seluruh dataset.
    * Menggabungkan hasil prediksi dengan data shapefile.
    * Membuat peta koroplet (choropleth map) yang memvisualisasikan daerah rawan banjir dengan gradasi warna.

## 5. Metrik Evaluasi Model

Performa model prediksi dievaluasi menggunakan metrik-metrik standar untuk masalah klasifikasi:

* **Accuracy**: Persentase total prediksi yang benar.
* **Precision**: Kemampuan model untuk tidak salah melabeli sebuah daerah sebagai 'Rawan Banjir'. Penting untuk menghindari alarm palsu.
* **Recall (Sensitivity)**: Kemampuan model untuk menemukan semua daerah yang benar-benar 'Rawan Banjir'. Penting untuk tidak melewatkan kasus risiko.
* **F1-Score**: Rata-rata harmonik dari Precision dan Recall, memberikan gambaran performa yang seimbang.
* **Confusion Matrix**: Memberikan rincian visual mengenai performa klasifikasi (True Positives, True Negatives, False Positives, False Negatives).

## 6. Fitur Pengembangan Lanjutan (Ide Kreatif)

Sistem ini memiliki potensi untuk dikembangkan lebih lanjut menjadi solusi yang lebih komprehensif:

* **Dashboard Interaktif**:
    * Membangun dashboard web menggunakan **Streamlit** atau **Dash**. Dashboard ini memungkinkan pemerintah daerah untuk melihat peta secara interaktif, memfilter berdasarkan kabupaten, dan melihat detail data penggunaan lahan untuk setiap wilayah.

* **Integrasi API Data Cuaca Harian**:
    * Menghubungkan sistem dengan API dari **BMKG** untuk mendapatkan data prediksi curah hujan harian.
    * Model dapat diperkaya dengan fitur cuaca, sehingga prediksi tidak hanya statis berdasarkan penggunaan lahan, tetapi juga dinamis berdasarkan kondisi cuaca terkini.

* **Sistem Peringatan Dini (Early Warning System)**:
    * Merancang sistem notifikasi otomatis. Jika suatu daerah yang diprediksi memiliki kerawanan tinggi (`Prediksi=1`) juga diprediksi akan menerima curah hujan ekstrem (dari API BMKG), sistem dapat mengirimkan email atau notifikasi SMS kepada pihak terkait (misalnya BPBD) sebagai peringatan dini.

## 7. Cara Menjalankan Notebook

1.  **Lingkungan**: Buka file `Prediksi-Banjir-Sumsel.ipynb` di Google Colaboratory.
2.  **Eksekusi Sel**: Jalankan sel-sel kode secara berurutan.
3.  **Upload Data**: Saat diminta, unggah file-file yang diperlukan:
    * `penggunaan_lahan_sumsel.csv`
    * `riwayat_banjir_sumsel.csv`
    * `sumsel_admin.zip` (Shapefile batas administrasi)

---
