# 🧠 Analisis Sentimen Teks Politik — Proyek NLP (Bahasa Indonesia)

## 📋 Ringkasan Proyek
Proyek ini menganalisis sentimen publik dari teks (misalnya tweet atau postingan media sosial) yang berkaitan dengan isu politik nasional.  
Melalui proses pra-pemrosesan teks, eksplorasi kata, hingga pembangunan model klasifikasi berbasis machine learning, proyek ini bertujuan memahami bagaimana opini publik terbentuk dan seberapa besar dominasi sentimen negatif atau positif dalam percakapan daring.

---

## 📌 Tujuan
- Mengidentifikasi **sentimen positif atau negatif** dari teks politik.
- Menemukan **pola frasa atau kata populer** (n-gram, wordcloud).
- Membangun **model klasifikasi teks** menggunakan beberapa algoritma ML.
- Mengevaluasi kinerja model berdasarkan hasil prediksi terhadap data uji.

---

## 🧾 Dataset
- Dataset berisi teks politik dari media sosial.
- Total data: ±7.300 baris teks.
- Label: **Negatif (80.28%)**, **Positif (19.72%)**.
- Pembagian data:
  - **Training**: 5.799 data (80%)
  - **Testing**: 1.450 data (20%)

---

## 🔧 Pra-Pemrosesan Teks
Langkah yang diterapkan:
1. Lowercasing (semua huruf kecil)
2. Hapus URL, mention (@username), dan hashtag
3. Hapus tanda baca & angka
4. Tokenisasi dan penghapusan stopwords
5. Normalisasi kata
6. (Opsional) Stemming atau Lemmatization  
   
**Tujuan:** Menghasilkan teks bersih dan konsisten agar representasi vektor lebih akurat untuk pemodelan.

---

## 📊 Eksplorasi Data & Visualisasi

### 🔹 1. Top 10 3-Gram
Beberapa frasa tiga kata paling sering muncul:
- `"uu perampasan aset"` — 73 kali  
- `"seluruh rakyat indonesia"` — 58 kali  
- `"perampasan aset koruptor"`, `"ruu perampasan aset"` — sering muncul

**Interpretasi:**  
Topik yang paling dominan berkaitan dengan isu hukum dan politik seperti *perampasan aset* dan *DPR*.

---

### 🔹 2. Wordcloud (Before vs After Preprocessing)
- Sebelum preprocessing: banyak noise (kata sambung dan tanda baca).  
- Setelah preprocessing: kata-kata utama yang muncul adalah:
  - `rakyat`, `dpr`, `indonesia`, `demo`, `bubarkan`, `negara`, `mahasiswa`.

**Kesimpulan:**  
Pra-pemrosesan berhasil memfokuskan konteks kata pada topik inti yang relevan secara politik.

---

### 🔹 3. Frekuensi Kata (Before vs After)
- **Sebelum:** `rakyat`, `DPR`, `di`, `dan`, `yang`.  
- **Sesudah:** `rakyat`, `dpr`, `demo`, `bubarkan`, `anggota`, `pejabat`.

**Interpretasi:**  
Kata yang muncul setelah preprocessing mencerminkan sentimen publik — cenderung kritis terhadap lembaga pemerintah.

---

## ❤️ Sentimen Publik
Distribusi sentimen dalam dataset:
- **Negatif:** 5.871 (≈ 80.28%)
- **Positif:** 1.442 (≈ 19.72%)

**Kesimpulan:**  
Mayoritas percakapan publik bersentimen negatif terhadap isu politik yang sedang dianalisis.

---

## 📈 Analisis Keyword
Frekuensi penyebutan beberapa tokoh publik:
| Nama Tokoh | Presentase (%) |
|-------------|----------------|
| Ahmad Sahroni | 21.10 |
| Eko Patrio | 20.41 |
| Uya Kuya | 19.69 |
| Adies Kadir | 19.44 |
| Nafa Urbach | 19.36 |

**Interpretasi:**  
Kelima figur publik ini memiliki keterlibatan yang relatif seimbang dalam percakapan terkait topik yang diteliti.

---

## 🤖 Pemodelan Klasifikasi

Model yang digunakan:
1. **K-Nearest Neighbors (KNN)**
2. **Naive Bayes**
3. **Random Forest**
4. **Decision Tree**

> Fitur teks direpresentasikan menggunakan **TF-IDF Vectorizer**.

### 🔹 Pembagian Data
- **Training:** 5.799 data  
- **Testing:** 1.450 data  

---

## 📉 Hasil Evaluasi Model (Confusion Matrix)

| Model | True Negative | False Positive | False Negative | True Positive | Catatan |
|--------|----------------|----------------|----------------|----------------|----------|
| **KNN** | 1104 | 58 | 200 | 88 | Baik di kelas negatif, tapi kurang akurat di kelas positif |
| **Naive Bayes** | 1126 | 36 | 212 | 76 | Cenderung memprediksi negatif terlalu dominan |
| **Random Forest** | 1086 | 76 | 128 | 160 | Lebih seimbang, performa terbaik untuk kelas positif |
| **Decision Tree** | 1039 | 123 | 114 | 174 | Performa baik tapi berisiko overfitting |

---

## 🔍 Analisis Kinerja
- **Naive Bayes & KNN:** cepat dan sederhana, tapi lemah dalam menangani imbalance.
- **Random Forest:** memberikan keseimbangan terbaik antar kelas (akurasi positif meningkat).
- **Decision Tree:** menghasilkan recall positif tertinggi, namun butuh tuning agar tidak overfit.

---

## 💡 Insight dan Kesimpulan
1. **Sentimen publik dominan negatif** (80%) terhadap topik politik.  
2. **Topik utama** berkisar pada *RUU Perampasan Aset*, *DPR*, dan *demo masyarakat*.  
3. **Random Forest dan Decision Tree** adalah kandidat model terbaik dengan prediksi paling seimbang.  
4. Model baseline seperti **Naive Bayes** tetap berguna untuk referensi awal dan interpretabilitas.  
5. Masih ada **class imbalance**, di mana data negatif jauh lebih banyak daripada positif.

---

## 🚀 Rekomendasi Pengembangan
1. **Atasi ketidakseimbangan data**  
   - Gunakan teknik seperti `SMOTE`, `class_weight`, atau `oversampling`.

2. **Gunakan fitur kontekstual yang lebih kuat**
   - Ganti TF-IDF dengan **Word2Vec**, **FastText**, atau **Transformer embeddings (IndoBERT)**.

3. **Evaluasi model lebih lengkap**
   - Tambahkan metrik: `Precision`, `Recall`, `F1-score`, dan `ROC AUC`.

4. **Lakukan analisis topik (Topic Modeling)**
   - Gunakan `LDA` atau `NMF` untuk mengetahui tema dominan di setiap kelompok sentimen.

5. **Bangun dashboard interaktif**
   - Gunakan `Streamlit` atau `Dash` agar hasil analisis bisa dieksplorasi secara real-time.

---

## 🧩 Visualisasi (Referensi Gambar)
| No | Gambar | Deskripsi |
|----|--------|------------|
| 1 | Top 10 3-Gram | Frasa 3 kata yang paling sering muncul |
| 2 | Wordcloud (Before & After) | Visual perbandingan kata sebelum dan sesudah preprocessing |
| 3 | Frekuensi Kata | Grafik top kata sebelum dan sesudah pembersihan |
| 4 | Distribusi Sentimen | Pie chart proporsi negatif vs positif |
| 5 | Keyword Analysis | Persentase tokoh publik yang disebut |
| 6 | Confusion Matrix | Matriks hasil evaluasi untuk tiap model |

---

## 🏁 Kesimpulan Akhir
Proyek ini berhasil menunjukkan bagaimana **teknik NLP klasik** dapat digunakan untuk **analisis sentimen politik** di Indonesia.  
Dengan preprocessing yang tepat dan pemilihan model yang seimbang seperti **Random Forest**, sistem dapat mendeteksi sentimen publik secara cukup akurat.  
Langkah selanjutnya adalah menggabungkan pendekatan **machine learning tradisional dengan model berbasis transformer** (seperti IndoBERT) untuk hasil yang lebih kontekstual dan presisi tinggi.

---

## 👨‍💻 Penulis
**Achmad Rivaldi Zulfah**  
_Bangkit Machine Learning Path 2025 — Submission Akhir (Analisis Sentimen)_  
Email: _[isi dengan emailmu]_  
GitHub: _[username kamu]_
