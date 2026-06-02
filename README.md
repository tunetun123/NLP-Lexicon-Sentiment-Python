# Lexicon-Based Sentiment Analysis untuk Data Komentar X

Repositori ini berisi alat untuk melakukan analisis sentimen berbasis leksikon pada data komentar dari platform X (sebelumnya Twitter). Sistem ini menggunakan kamus **InSet (Indonesia Sentiment Lexicon)** dan dilengkapi dengan algoritma *negation handling* untuk meningkatkan akurasi, serta menghasilkan visualisasi data interaktif.

## 📌 Fitur Utama
* **Lexicon-Based Scoring:** Menggunakan bobot kata dari repositori InSet (`positive.tsv` dan `negative.tsv`).
* **Negation Handling:** Mampu mendeteksi dan membalikkan skor kata jika didahului oleh kata negasi (contoh: "tidak", "bukan", "kurang").
* **Output Tabel Terstruktur:** Menghasilkan data CSV berisi ID komentar, skor total, jumlah kata, skor rata-rata, dan label sentimen (Positif, Negatif, Netral).
* **Visualisasi Interaktif:** Menghasilkan grafik batang interaktif (berbasis HTML) yang mendetailkan skor per dokumen, lengkap dengan fitur *hover* untuk membaca teks asli.

## 📂 Persiapan File
Sebelum menjalankan program, pastikan Anda telah menyiapkan 3 file berikut:
1. `positive.tsv` (Kamus leksikon positif dari InSet)
2. `negative.tsv` (Kamus leksikon negatif dari InSet)
3. `data_komentar.csv` (Dataset mentah milik Anda. Pastikan data sudah melalui tahap *pre-processing*)

## 🚀 Panduan Penggunaan di Google Colab

Aplikasi ini sangat ideal dijalankan melalui **Google Colab** karena tidak memerlukan instalasi lokal yang rumit. Ikuti langkah-langkah berikut:

### 1. Membuka File Notebook di Colab
1. Pastikan Anda sudah mengunduh file `Sentiment_Analysis.ipynb` dari repositori ini ke komputer Anda.
2. Buka browser dan akses [Google Colab](https://colab.research.google.com/).
3. Login menggunakan akun Google Anda.
4. Pada jendela *pop-up* yang muncul, pilih tab **Upload**.
5. Klik **Browse**, lalu cari dan pilih file `Sentiment_Analysis.ipynb` dari komputer Anda. Notebook berisi kode program akan otomatis terbuka.

### 2. Mengunggah File ke Workspace
1. Perhatikan bilah sisi (*sidebar*) di sebelah kiri layar Colab, lalu klik ikon **Folder** (Files).
2. Tunggu beberapa detik hingga sistem mengalokasikan penyimpanan untuk sesi Anda.
3. Klik ikon **Upload ke session storage** (kertas dengan panah ke atas).
4. Pilih ketiga file yang sudah disiapkan pada tahap persiapan (`positive.tsv`, `negative.tsv`, dan `data_komentar.csv`), lalu klik **Open**.
> **Catatan Penting:** File yang diunggah ke Colab bersifat sementara. Jika sesi terputus atau tab ditutup, Anda harus mengunggahnya kembali.

### 3. Menjalankan Aplikasi
1. Karena kode program sudah tersedia di dalam file notebook, Anda hanya perlu mengeksekusinya.
2. Anda bisa menjalankan program secara otomatis dengan mengklik menu **Runtime** di bilah navigasi atas, lalu pilih **Run all** (Jalankan semua). 
3. *Atau*, Anda bisa menjalankan blok kode satu per satu dengan menekan tombol **Play** (ikon segitiga) di sebelah kiri masing-masing sel kode.
4. Program akan mulai memproses data. Anda bisa melihat progresnya pada log teks yang muncul di bawah blok eksekusi.
5. Setelah proses selesai, grafik sentimen interaktif akan langsung dirender dan ditampilkan di layar Colab.

### 4. Mengunduh Hasil Analisis (Output)
Aplikasi ini secara otomatis akan menghasilkan dua file keluaran baru di dalam panel *Folder* sebelah kiri. Jika belum muncul, klik ikon **Refresh** di panel tersebut.

* **`output_sentiment_score.csv`**: File tabel berisi hasil akhir skoring sentimen untuk setiap komentar.
* **`interactive_sentiment_chart.html`**: File grafik interaktif yang bisa Anda buka menggunakan browser web apa pun.

Untuk mengunduhnya:
1. Arahkan kursor ke nama file *output* di panel *Folder*.
2. Klik ikon tiga titik vertikal (⋮) di sebelah nama file.
3. Pilih **Download** untuk menyimpannya ke penyimpanan lokal komputer Anda.

---
*Dibuat untuk kebutuhan analisis sentimen data X (Twitter).*
