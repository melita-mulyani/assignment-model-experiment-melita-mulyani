# Eksperimen & Evaluasi Model AI

## 1. Gambaran Umum Proyek

Proyek ini membandingkan dua pendekatan AI yang berbeda untuk melakukan klasifikasi sentimen terhadap ulasan pelanggan:

1. Machine Learning klasik menggunakan Scikit-learn.
2. Large Language Model (LLM) menggunakan Gemini AI API dengan pendekatan zero-shot prompting.

Kedua pendekatan dievaluasi menggunakan dataset dan test set yang sama untuk membandingkan performa klasifikasi serta karakteristik teknisnya.

## 2. Pernyataan Masalah

Sebuah tim produk e-commerce ingin melakukan klasifikasi ulasan pelanggan secara otomatis ke dalam dua kategori sentimen:

* positif
* negatif

Tujuan dari eksperimen ini adalah membandingkan model Machine Learning yang dilatih secara lokal dengan pendekatan LLM berbasis API sebelum menentukan pendekatan teknis untuk pengembangan lebih lanjut.

## 3. Tujuan

Tujuan dari eksperimen ini adalah untuk:

* Memahami permasalahan klasifikasi sentimen.
* Melatih model klasifikasi Machine Learning klasik menggunakan Scikit-learn.
* Melakukan klasifikasi sentimen menggunakan Gemini AI API dengan zero-shot prompting.
* Mengevaluasi kedua pendekatan menggunakan Accuracy, Precision, Recall, dan F1-Score.
* Menganalisis trade-off teknis antara kedua pendekatan.
* Menyusun rekomendasi teknis berdasarkan hasil eksperimen.

## 4. Dataset

Eksperimen menggunakan dataset sentimen ulasan pelanggan yang diberikan oleh mentor.

Dataset asli disimpan di:

`data/customer_reviews_sentiment.csv`

Dataset asli tidak dimodifikasi secara langsung.

## 5. Experimental Setup

### 5.1 Pembagian Train-Test

Dataset dibagi menjadi:

* Training set: 160 sampel
* Test set: 40 sampel

Kedua pendekatan dievaluasi menggunakan test set yang sama.

### 5.2 Classical Machine Learning

Pipeline Machine Learning klasik terdiri dari:

1. Text input
2. TF-IDF vectorization
3. Logistic Regression
4. Sentiment prediction
5. Evaluation

### 5.3 Gemini LLM

Pendekatan LLM menggunakan Gemini AI API dengan zero-shot prompt.

Prompt memberikan instruksi kepada Gemini untuk mengklasifikasikan setiap ulasan pelanggan ke dalam salah satu dari dua kategori, yaitu `positif` atau `negatif`.

Tidak ada contoh berlabel yang diberikan di dalam prompt.

## 6. Hasil Evaluasi

Kedua pendekatan menghasilkan performa berikut pada test set yang terdiri dari 40 sampel:

| Metric    | Classic ML | Gemini LLM |
| --------- | ---------: | ---------: |
| Accuracy  |       100% |       100% |
| Precision |       100% |       100% |
| Recall    |       100% |       100% |
| F1-Score  |       100% |       100% |

Kedua pendekatan berhasil mengklasifikasikan seluruh 40 sampel pada test set dengan benar.

## 7. Analisis Trade-off

Meskipun kedua pendekatan menghasilkan performa yang sama pada test set, keduanya memiliki karakteristik teknis yang berbeda.

### Classical Machine Learning

**Keunggulan:**

* Local inference setelah proses training.
* Tidak bergantung pada external inference API.
* Memiliki kontrol yang lebih besar terhadap model yang telah dilatih dan lingkungan inference.
* Sesuai untuk melakukan prediksi lokal secara berulang.

**Pertimbangan:**

* Memerlukan proses training dan feature engineering.
* Perubahan pada model dapat memerlukan proses retraining.
* Pengembangan awal memerlukan persiapan data dan pengembangan model.

### Gemini LLM

**Keunggulan:**

* Tidak memerlukan proses training model untuk tugas klasifikasi ini.
* Dapat diimplementasikan menggunakan instruksi berbasis prompt.
* Memberikan fleksibilitas melalui prompt design.

**Pertimbangan:**

* Memerlukan konektivitas API.
* Bergantung pada ketersediaan API, quota, dan rate limit.
* Penggunaan API dapat menimbulkan biaya operasional.
* Perilaku model/API dapat berubah secara independen dari aplikasi.

Selama eksperimen ini, Gemini API mengalami error terkait rate limit dan ketersediaan layanan sementara. Mekanisme checkpoint digunakan agar hasil prediksi yang telah berhasil dapat disimpan dan eksperimen dapat dilanjutkan tanpa perlu mengulangi request yang telah selesai diproses.

## 8. Rekomendasi Teknis

Kedua pendekatan menunjukkan kemampuan untuk mengklasifikasikan test dataset dengan baik, dengan hasil 100% pada seluruh metrik yang dievaluasi.

Oleh karena itu, performa pada test set yang hanya terdiri dari 40 sampel belum cukup untuk menentukan pendekatan yang akan digunakan di lingkungan produksi.

Pendekatan Classical Machine Learning sesuai dengan skenario yang membutuhkan local inference, kontrol langsung terhadap model yang telah dilatih, serta prediksi berulang tanpa ketergantungan pada external inference API.

Pendekatan Gemini LLM sesuai dengan skenario yang mengutamakan rapid prototyping, minimnya kebutuhan untuk melakukan model training, serta fleksibilitas berbasis prompt.

Sebelum dilakukan deployment ke lingkungan produksi, validasi lebih lanjut perlu dilakukan menggunakan dataset yang lebih besar dan lebih beragam. Evaluasi tambahan juga perlu mempertimbangkan inference latency, biaya operasional, scalability, ketergantungan terhadap API, serta performa terhadap data yang belum pernah dilihat sebelumnya.

## 9. Struktur Proyek

```text
model-experiment-assignment/
├── data/
│   └── customer_reviews_sentiment.csv
├── notebook/
│   └── experiment_notebook.ipynb
├── documentation/
│   └── model_comparison_summary.png
├── README.md
└── requirements.txt
```
## 10. Cara Menjalankan

### 10.1 Membuat dan Mengaktifkan Virtual Environment

Buat virtual environment menggunakan perintah berikut:

```bash
python -m venv .venv
```

Untuk Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

### 10.2 Menginstal Dependencies

Instal seluruh dependencies yang dibutuhkan dengan menjalankan:

```bash
pip install -r requirements.txt
```

### 10.3 Mengatur Gemini API Key

Atur Gemini API key sebagai **environment variable** sebelum menjalankan eksperimen LLM.

API key tidak boleh disimpan secara langsung di dalam notebook atau di-commit ke GitHub.

### 10.4 Menjalankan Eksperimen

Buka file:

`notebook/experiment_notebook.ipynb`

Kemudian jalankan cell pada notebook secara berurutan.

## 11. Catatan

File checkpoint Gemini API digunakan untuk menyimpan hasil inference yang telah berhasil ketika terjadi masalah terkait API quota atau ketersediaan layanan selama eksperimen.

Hasil evaluasi yang dilaporkan secara khusus berlaku untuk dataset dan test set yang terdiri dari 40 sampel yang digunakan dalam eksperimen ini.
