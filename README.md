# Pembuatan Judul Artikel Berita

Proyek ini bertujuan menghasilkan judul artikel berita yang ringkas dan akurat menggunakan model sequence-to-sequence. Dataset dari [NewsAPI](https://newsapi.org/) berisi artikel dan judulnya. Tiga arsitektur diimplementasikan untuk membandingkan performa:
- Encoder-Decoder berbasis LSTM
- Encoder-Decoder dengan attention
- Transformer dengan multi-head attention

Proyek mencakup pengumpulan data, pra-pemrosesan, pelatihan model, pembuatan judul, dan evaluasi.

## Persyaratan
- Dataset artikel berita dari NewsAPI
- Implementasi tiga arsitektur:
  - Encoder-Decoder LSTM
  - Encoder-Decoder dengan attention
  - Model Transformer
- Pembuatan judul berdasarkan konten
- Evaluasi judul menggunakan metrik ROUGE

## Dataset
Dataset dikumpulkan dari NewsAPI dan disimpan dalam `newsapi_articles.csv`, berisi:
- **Content**: Teks artikel berita
- **Title**: Judul artikel

## Arsitektur Model
1. **Encoder-Decoder Dasar dengan LSTM**
   - **Encoder**: LSTM mengkodekan artikel ke vektor konteks.
   - **Decoder**: LSTM menghasilkan judul kata demi kata.
   - **Parameter**:
     - Embedding: 128
     - Unit LSTM: 256
     - Panjang Artikel: 200
     - Panjang Judul: 20

2. **Encoder-Decoder dengan Attention**
   - **Encoder**: Lapisan Dense memproses artikel.
   - **Decoder**: Dense dengan attention dot-product.
   - **Parameter**:
     - Embedding: 128
     - Unit Attention: 256
     - Panjang Artikel: 200
     - Panjang Judul: 20

3. **Model Transformer**
   - **Encoder**: Multi-head self-attention dan feed-forward.
   - **Decoder**: Masked self-attention, cross-attention, dan feed-forward.
   - **Parameter**:
     - Embedding: 128
     - Jumlah Head: 4
     - Feed-Forward: 512
     - Lapisan: 2
     - Dropout: 0.1
     - Panjang Artikel: 200
     - Panjang Judul: 20

## Persiapan Data
- **Memuat Data**:
  - Ambil kolom `content` dan `title` dari `newsapi_articles.csv`.
  - Hapus baris dengan nilai kosong.
- **Pembersihan Teks**:
  - Hapus URL, tanda baca, dan spasi berlebih.
  - Ubah ke huruf kecil.
- **Tokenisasi**:
  - Gunakan NLTK `word_tokenize` dan hapus stopwords.
- **Sekuen**:
  - Tokenisasi dengan `Tokenizer` TensorFlow.
  - Pad artikel (200) dan judul (20).
  - Buat pasangan decoder untuk teacher forcing.
- **Pemisahan**:
  - 80% pelatihan, 20% pengujian.

## Pelatihan dan Pembuatan Judul
- **Pelatihan**:
  - 10 epoch, batch 64.
  - Kerugian: `sparse_categorical_crossentropy`.
  - Optimizer: Adam.
  - Validasi pada set pengujian.
- **Pembuatan Judul**:
  - Dekoding greedy untuk menghasilkan judul.
  - Tokenizer digunakan untuk pra-pemrosesan dan dekoding.

## Evaluasi
Kualitas judul dievaluasi dengan **ROUGE** (ROUGE-1, ROUGE-2, ROUGE-L) menggunakan paket `rouge` dan `rouge-score`.
