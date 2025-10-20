# Analisis Sentimen Komentar YouTube Terhadap Tokoh Publik

Proyek ini bertujuan untuk melakukan analisis sentimen terhadap komentar-komentar dari platform YouTube yang berkaitan dengan beberapa tokoh publik. Analisis ini mencakup beberapa tahapan, mulai dari pengambilan data secara otomatis, pra-pemrosesan teks, pelabelan sentimen berbasis leksikon, ekstraksi aspek, hingga klasifikasi sentimen menggunakan beberapa model *machine learning*.

## Daftar Isi
1.  [Struktur Proyek](#struktur-proyek)
2.  [Hasil Analisis](#hasil-analisis)
3.  [Cara Menjalankan](#cara-menjalankan)
4.  [Struktur File](#struktur-file)

## Struktur Proyek

Alur kerja dalam notebook ini dibagi menjadi beberapa tahapan utama:

### 1. Pengambilan Data (Crawling)
-   **Tujuan:** Mengambil data komentar dari beberapa video YouTube yang telah ditentukan.
-   **Metode:** Menggunakan **YouTube Data API v3** untuk mengakses data komentar.
-   **Pustaka:** `googleapiclient`, `pandas`.
-   **Proses:**
    1.  Skrip mengambil daftar ID video YouTube yang relevan.
    2.  Fungsi `scrape_comments_from_videos` dipanggil untuk mengunduh komentar. Dari target 30.000 komentar, skrip berhasil mengambil **10.778 komentar**, kemungkinan karena batasan API.
    3.  Data mentah yang terkumpul disimpan dalam file **`comments.csv`**.

    ![Data Mentah](https://storage.googleapis.com/gemini-prod/images/42c38827-017e-4028-a3f1-43542289c0b1)

### 2. Pra-pemrosesan Data (Preprocessing)
-   **Tujuan:** Membersihkan dan mempersiapkan data teks komentar agar siap untuk dianalisis. Proses ini mencakup penghapusan duplikat, pembersihan teks (URL, emoji, simbol), *case folding*, normalisasi kata baku, tokenisasi, dan penghapusan *stopword*.
-   **Visualisasi Pra-pemrosesan:**
    * **Word Cloud:** Perbandingan visual kata-kata yang paling sering muncul sebelum dan sesudah data dibersihkan. Setelah pra-pemrosesan, kata-kata inti seperti "rakyat", "DPR", dan "wakil" menjadi lebih dominan.

        ![Word Cloud](https://storage.googleapis.com/gemini-prod/images/42c38827-017e-4028-a3f1-43542289c0b1)

    * **Frekuensi Kata:** Grafik ini menunjukkan 10 kata teratas sebelum dan sesudah pra-pemrosesan. Terlihat jelas bahwa kata-kata yang tidak relevan telah berhasil dihilangkan.

        ![Frekuensi Kata](https://storage.googleapis.com/gemini-prod/images/42c38827-017e-4028-a3f1-43542289c0b1)

    * **Frekuensi N-Gram (Trigram):** Analisis trigram (frasa tiga kata) menunjukkan konteks diskusi yang lebih dalam, seperti "wakil rakyat indonesia" dan "dewan perwakilan rakyat".

        ![Frekuensi Trigram](https://storage.googleapis.com/gemini-prod/images/42c38827-017e-4028-a3f1-43542289c0b1)

### 3. Pelabelan Sentimen (Lexicon-Based)
-   **Tujuan:** Memberi label sentimen (Positif atau Negatif) pada setiap komentar.
-   **Metode:** Menggunakan pendekatan berbasis leksikon dengan **InSet (Indonesian Sentiment Lexicon)**.
-   **Hasil:** Dari total 7.249 komentar unik yang berhasil diproses, mayoritas besar menunjukkan sentimen **Negatif**.

    ![Distribusi Sentimen](https://storage.googleapis.com/gemini-prod/images/42c38827-017e-4028-a3f1-43542289c0b1)

### 4. Ekstraksi Aspek
-   **Tujuan:** Mengklasifikasikan setiap komentar ke dalam aspek atau kategori tokoh publik yang relevan.
-   **Metode:** Ekstraksi berbasis kata kunci (*keyword-based*).
-   **Hasil:** Distribusi komentar per aspek (tokoh publik) menunjukkan bahwa **Eko Patrio** menjadi tokoh yang paling banyak dibicarakan dalam sampel data ini.

    ![Distribusi Aspek](https://storage.googleapis.com/gemini-prod/images/42c38827-017e-4028-a3f1-43542289c0b1)

### 5. Klasifikasi Sentimen (Machine Learning)
-   **Tujuan:** Melatih dan mengevaluasi beberapa model *machine learning* untuk mengklasifikasikan sentimen secara otomatis.
-   **Proses:** Data dibagi menjadi **80% data latih (5.799 sampel)** dan **20% data uji (1.450 sampel)**.

    ![Distribusi Data Latih dan Uji](https://storage.googleapis.com/gemini-prod/images/42c38827-017e-4028-a3f1-43542289c0b1)

-   **Evaluasi Model:** Performa setiap model dievaluasi menggunakan matriks konfusi untuk melihat kemampuannya dalam memprediksi sentimen Positif dan Negatif.

    * **SVM & KNN:**
        <img src="https://storage.googleapis.com/gemini-prod/images/42c38827-017e-4028-a3f1-43542289c0b1" alt="Matriks Konfusi SVM" width="48%"> <img src="https://storage.googleapis.com/gemini-prod/images/42c38827-017e-4028-a3f1-43542289c0b1" alt="Matriks Konfusi KNN" width="48%">

    * **Naive Bayes & Random Forest:**
        <img src="https://storage.googleapis.com/gemini-prod/images/42c38827-017e-4028-a3f1-43542289c0b1" alt="Matriks Konfusi Naive Bayes" width="48%"> <img src="https://storage.googleapis.com/gemini-prod/images/42c38827-017e-4028-a3f1-43542289c0b1" alt="Matriks Konfusi Random Forest" width="48%">

    * **Decision Tree & Neural Network:**
        <img src="https://storage.googleapis.com/gemini-prod/images/42c38827-017e-4028-a3f1-43542289c0b1" alt="Matriks Konfusi Decision Tree" width="48%"> <img src="https://storage.googleapis.com/gemini-prod/images/42c38827-017e-4028-a3f1-43542289c0b1" alt="Matriks Konfusi Neural Network" width="48%">

## Hasil Analisis

Berdasarkan perbandingan akurasi dari semua model yang diuji, **Neural Network (MLPClassifier)** menunjukkan performa terbaik dalam mengklasifikasikan sentimen pada data uji dengan akurasi **91%**, diikuti oleh **Support Vector Machine (SVM)** dengan akurasi **90%**.

![Grafik Perbandingan Akurasi Model](https://storage.googleapis.com/gemini-prod/images/42c38827-017e-4028-a3f1-43542289c0b1)

## Cara Menjalankan

Untuk menjalankan notebook ini, ikuti langkah-langkah berikut:

1.  **Kloning Repositori (jika ada):**
    ```bash
    git clone [URL_REPOSITORI_ANDA]
    cd [NAMA_FOLDER_REPOSITORI]
    ```

2.  **Instal Dependensi:**
    Pastikan Anda telah menginstal semua pustaka yang diperlukan.
    ```bash
    pip install pandas google-api-python-client scikit-learn seaborn matplotlib wordcloud nltk requests openpyxl
    ```

3.  **Konfigurasi API Key:**
    Buka file `analisis_sentiment.ipynb`. Pada sel kode pertama di bagian "CRAWLING DATA", ganti nilai variabel `api_key` dengan kunci API YouTube Data v3 Anda.
    ```python
    api_key = "GANTI_DENGAN_API_KEY_ANDA"
    ```

4.  **Jalankan Notebook:**
    Jalankan semua sel dalam notebook `analisis_sentiment.ipynb` secara berurutan.

## Struktur File
