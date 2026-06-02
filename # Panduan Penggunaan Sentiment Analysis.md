# Panduan Penggunaan Sentiment Analysis (Lexicon-Based) di Google Colab

Dokumentasi ini berisi langkah-langkah terstruktur untuk menjalankan aplikasi **Sentiment Analysis** menggunakan kamus InSet dan visualisasi interaktif Plotly di lingkungan komputasi awan Google Colab.

## 📁 Persiapan File

Sebelum memulai, pastikan Anda telah menyiapkan 3 file berikut di penyimpanan lokal komputer Anda:

1. `positive.tsv` (Kamus leksikon positif dari GitHub InSet)
2. `negative.tsv` (Kamus leksikon negatif dari GitHub InSet)
3. `data_komentar.csv` (Dataset mentah komentar Anda)

---

## 🚀 Langkah 1: Membuat Project Baru di Google Colab

1. Buka _browser_ dan akses **[Google Colab](https://colab.research.google.com/)**.
2. Pastikan Anda sudah _login_ menggunakan akun Google.
3. Pada jendela _pop-up_ yang muncul, pilih tab **New Notebook** (Buku Catatan Baru) di sudut kanan bawah.
4. Ganti nama _project_ di pojok kiri atas (misalnya dari `Untitled0.ipynb` menjadi `Sentiment_Analysis_X.ipynb`).

---

## ☁️ Langkah 2: Mengunggah File ke Ruang Kerja Colab

Google Colab menyediakan sistem _file explorer_ virtual di bilah sisi (_sidebar_).

1. Di sebelah kiri layar Colab, klik ikon **Folder** (Files) untuk membuka panel _file explorer_.
2. Tunggu beberapa detik hingga Colab mengalokasikan server (muncul indikator kapasitas _Disk_ dan _RAM_ di pojok kanan atas).
3. Klik ikon **Upload ke session storage** (ikon kertas dengan tanda panah ke atas).
4. Pilih ketiga file Anda (`positive.tsv`, `negative.tsv`, dan `data_komentar.csv`) dari komputer, lalu klik **Open**.

> **⚠️ Catatan Penting:** File yang diunggah ke _session storage_ ini bersifat sementara dan akan terhapus jika Anda menutup tab Colab atau membiarkannya _idle_ terlalu lama.

---

## 💻 Langkah 3: Menempelkan Kode Program

Salin seluruh kode Python di bawah ini dan tempelkan (_paste_) ke dalam blok kode (sel/ _cell_) pertama di Google Colab Anda.

```python
import pandas as pd
import plotly.express as px

# 1. FUNGSI LOAD LEXICON
def load_inset_lexicon(pos_filepath, neg_filepath):
    df_pos = pd.read_csv(pos_filepath, sep='\t')
    df_neg = pd.read_csv(neg_filepath, sep='\t')
    lexicon_dict = {}
    for _, row in df_pos.iterrows(): lexicon_dict[row['word']] = row['weight']
    for _, row in df_neg.iterrows(): lexicon_dict[row['word']] = row['weight']
    return lexicon_dict

# 2. FUNGSI ANALISIS SENTIMEN & NEGASI
def analyze_sentiment_with_negation(comment_text, lexicon, comment_id):
    if pd.isna(comment_text) or str(comment_text).strip() == "":
        return {'id_komentar': comment_id, 'total_score': 0, 'word_count': 0, 'avg_score': 0.0, 'label': 'netral'}

    words = str(comment_text).lower().split()
    total_score = 0
    word_count = len(words)
    negation_words = {'tidak', 'tak', 'bukan', 'belum', 'jangan', 'kurang', 'enggak', 'gak', 'ga', 'ndak'}

    for i, word in enumerate(words):
        if word in lexicon:
            word_score = lexicon[word]
            if i > 0 and words[i-1] in negation_words:
                word_score = word_score * -1
            total_score += word_score

    avg_score = total_score / word_count if word_count > 0 else 0
    if total_score > 0: label = 'positif'
    elif total_score < 0: label = 'negatif'
    else: label = 'netral'

    return {'id_komentar': comment_id, 'total_score': total_score, 'word_count': word_count, 'avg_score': round(avg_score, 4), 'label': label}

# 3. FUNGSI PROSES DATASET
def process_dataset(csv_file_path, lexicon):
    df = pd.read_csv(csv_file_path, sep='\t', on_bad_lines='skip', engine='python')
    results = []
    for index, row in df.iterrows():
        result = analyze_sentiment_with_negation(row['text'], lexicon, row['No'])
        results.append(result)
    return pd.DataFrame(results), df

# 4. FUNGSI VISUALISASI PLOTLY
def plot_interactive_sentiment(df_output, df_asli):
    df_merged = df_output.merge(df_asli[['No', 'text']], left_on='id_komentar', right_on='No', how='left')
    color_map = {'positif': '#1b9e77', 'negatif': '#d95f02', 'netral': '#8e8e8e'}

    fig = px.bar(
        df_merged, x=df_merged.index, y='total_score', color='label', color_discrete_map=color_map,
        title='Sentiment Score per Document',
        labels={'index': 'Document', 'total_score': 'Score', 'label': 'Sentiment'},
        hover_data=['id_komentar', 'word_count', 'text']
    )

    fig.update_layout(
        plot_bgcolor='#EAEAEA', paper_bgcolor='white',
        xaxis=dict(showgrid=True, gridcolor='white', tickmode='array', tickvals=df_merged.index),
        yaxis=dict(showgrid=True, gridcolor='white'),
        legend_title_text='Sentiment', hovermode='closest'
    )

    fig.write_html('interactive_sentiment_chart.html')
    fig.show()

# ==========================================
# EKSEKUSI PROGRAM DI COLAB
# ==========================================
print("1. Memuat lexicon InSet...")
lexicon = load_inset_lexicon('positive.tsv', 'negative.tsv')

print("2. Memproses dataset komentar...")
nama_file_csv = 'data_komentar.csv'
df_hasil, df_mentah = process_dataset(nama_file_csv, lexicon)

print("3. Menyimpan hasil ke CSV...")
df_hasil.to_csv('output_sentiment_score.csv', index=False)
print(f"-> Berhasil memproses {len(df_hasil)} komentar.")

print("4. Membuat grafik interaktif...")
plot_interactive_sentiment(df_hasil, df_mentah)
```
