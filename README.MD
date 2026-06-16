# 📘 RAG Parameter Exploration Assignment

Pada tugas ini kamu akan bereksperimen dengan **6 parameter utama Retrieval-Augmented Generation (RAG)** menggunakan **RAG Playground**.

### 🎯 Tujuan Pembelajaran

Kamu akan:

✅ Memahami **apa fungsi** masing-masing parameter RAG

✅ Mengamati **pengaruh** tuning terhadap kualitas jawaban vs kecepatan

✅ Belajar **trade-off** antara retrieval & generation

✅ Menulis **insight praktis** dari eksperimen yang kamu lakukan

> **Tools sudah disediakan.** Kamu **tidak perlu ngoding**, hanya ubah nilai parameter di file `app.py` & restart server.

### ⚠️ Notes Penting

- **Ikuti video di LMS jika kamu kebingungan.**
- **Sediakan penyimpanan at least 2GB**, akan ada download model yang cukup besar ke laptopmu.
- **Hapus proyek ini kalau sudah selesai**, biar laptop-mu tidak tercekik.
- **Ketika proyek ini sudah diisi & dikerjakan di bawah, upload ke GitHub.**

---

## ⚙️ Parameter yang Diuji

Semua parameter berikut dapat ditemukan di file `app.py`:

```python
# Buka app.py, cari bagian KONFIGURASI INTI
CHUNK_SIZE = 300
TOP_K_INITIAL = 20
FINAL_TOP_M = 5
USE_RERANKER = True
TEMPERATURE = 0.2
GROQ_MODEL = "llama-3.1-8b-instant"
```

---

### 📘 RAG Parameter Exploration Assignment (FULL FILLED)
📏 1. CHUNK_SIZE

Fungsi CHUNK_SIZE

CHUNK_SIZE digunakan untuk menentukan jumlah kata dalam setiap potongan teks (chunk) saat dokumen PDF diproses. Parameter ini mengatur bagaimana dokumen dipecah sebelum masuk ke proses embedding dan retrieval.

Jawaban:
CHUNK_SIZE berfungsi untuk mengatur ukuran pemecahan teks PDF menjadi chunk kecil agar dapat diproses oleh sistem RAG. Semakin kecil chunk, informasi menjadi lebih spesifik, sedangkan semakin besar chunk, konteks yang dibawa semakin luas.

Pengaruhnya:

Range Nilai	Efek
Kecil (100–200)	Lebih presisi dan detail, tetapi konteks sering terputus sehingga jawaban bisa kurang lengkap
Sedang (250–400) ⭐ Recommended	Keseimbangan terbaik antara konteks dan presisi, hasil jawaban lebih stabil dan relevan
Besar (500–1000)	Konteks lebih luas tetapi bisa menurunkan relevansi karena terlalu banyak informasi dalam satu chunk

Refleksi:

Chunk yang paling tepat untuk dokumen saya adalah chunk sedang (300) karena hasilnya paling seimbang antara kelengkapan dan relevansi.
Jika jawaban terasa “terpotong” atau kurang konteks, maka parameter yang harus diubah adalah CHUNK_SIZE (ditingkatkan).
🔍 2. TOP_K_INITIAL

Fungsi TOP_K_INITIAL

TOP_K_INITIAL menentukan jumlah kandidat chunk awal yang diambil dari vector database sebelum dilakukan proses filtering atau reranking.

Jawaban:
TOP_K_INITIAL berfungsi sebagai jumlah awal hasil retrieval yang diambil sistem untuk diproses lebih lanjut sebelum dipilih yang paling relevan.

Pengaruhnya:

Range Nilai	Efek
Kecil (5–10)	Lebih cepat, tetapi risiko kehilangan konteks relevan lebih tinggi
Sedang (15–25) ⭐ Recommended	Keseimbangan antara kecepatan dan cakupan informasi
Besar (30–50)	Lebih lengkap (recall tinggi), tetapi lebih lambat

Refleksi:

Top-k kecil tidak selalu cukup untuk dokumen panjang karena bisa melewatkan konteks penting.
TOP_K_INITIAL sangat bergantung pada USE_RERANKER, karena tanpa reranker hasil awal sangat menentukan kualitas akhir.
📋 3. FINAL_TOP_M

Fungsi FINAL_TOP_M

FINAL_TOP_M menentukan jumlah chunk akhir yang dikirim ke LLM setelah proses retrieval dan reranking.

Jawaban:
FINAL_TOP_M berfungsi untuk membatasi jumlah konteks terbaik yang digunakan model dalam menghasilkan jawaban.

Pengaruhnya:

Range Nilai	Efek
Sedikit (2–3)	Jawaban lebih fokus tetapi berisiko kehilangan konteks penting
Sedang (4–6) ⭐ Recommended	Keseimbangan terbaik antara kelengkapan dan fokus jawaban
Banyak (7–10)	Konteks lebih lengkap tetapi bisa membuat model bingung atau jawaban terlalu panjang

Refleksi:

Tidak benar bahwa semakin banyak konteks selalu lebih baik.
Pada eksperimen, final_top_m = 5 memberikan hasil paling stabil.
Jika konteks saling bertentangan, model akan menghasilkan jawaban campuran atau kurang konsisten.
🎯 4. USE_RERANKER

Fungsi USE_RERANKER

USE_RERANKER mengaktifkan model tambahan (reranker) untuk mengurutkan ulang hasil retrieval berdasarkan tingkat relevansi yang lebih akurat.

Jawaban:
Reranker berfungsi untuk meningkatkan kualitas hasil pencarian dengan mengevaluasi ulang relevansi antara query dan chunk, sehingga konteks yang masuk ke LLM lebih akurat dibanding hanya cosine similarity.

Pengaruhnya:

Mode	Efek
True	Relevansi lebih tinggi, jawaban lebih akurat, sedikit lebih lambat
False	Lebih cepat, tetapi kualitas konteks lebih rendah karena hanya similarity dasar

Refleksi:

Reranker paling terasa manfaatnya pada query ambigu atau dokumen panjang.
Contoh: pertanyaan kebijakan, definisi panjang, atau multi-paragraf.
Latency bertambah sedikit tetapi masih worth it untuk kualitas jawaban.
🌡️ 5. TEMPERATURE

Fungsi TEMPERATURE

Temperature mengatur tingkat kreativitas model dalam menghasilkan jawaban.

Jawaban:
Temperature menentukan seberapa deterministik atau acak output dari model LLM.

Pengaruhnya:

Range Nilai	Efek
0.0–0.2 ⭐ Recommended	Jawaban sangat stabil, faktual, dan sesuai konteks
0.3–0.5	Masih cukup stabil tetapi sedikit lebih fleksibel
0.6–0.8	Lebih kreatif tetapi mulai tidak konsisten
0.9–1.0	Sangat acak, berisiko halusinasi tinggi

Refleksi:

Untuk RAG berbasis dokumen, temperature ideal adalah 0.2.
Saat dinaikkan ke 0.9, jawaban menjadi lebih bebas tetapi sering muncul informasi yang tidak ada di dokumen (hallucination).
🤖 6. GROQ_MODEL

Fungsi GROQ_MODEL

Menentukan model LLM yang digunakan untuk menghasilkan jawaban dari hasil retrieval.

Jawaban:
Model menentukan kualitas reasoning, kecepatan, dan akurasi output sistem RAG.

Model	Ukuran	Pengaruh
llama-3.1-8b-instant	8B	Sangat cepat, cukup akurat, cocok untuk testing
openai/gpt-oss-120b	120B	Sangat akurat tetapi lebih lambat
llama-3.3-70b-versatile	70B	Keseimbangan terbaik antara kualitas dan performa

Refleksi:

Model besar meningkatkan kualitas jawaban tetapi juga meningkatkan latency.
Untuk eksperimen ini, llama-3.1-8b-instant paling efisien untuk testing.
Untuk hasil terbaik, llama-3.3-70b-versatile lebih stabil dalam reasoning.
⚙️ Parameter Tambahan
Parameter	Fungsi
CHUNK_OVERLAP	Menjaga overlap antar chunk agar informasi tidak terputus
SCORE_THRESHOLD	Menyaring chunk dengan similarity rendah agar tidak masuk konteks

Penjelasan:

CHUNK_OVERLAP terlalu kecil → informasi bisa terputus
CHUNK_OVERLAP terlalu besar → redundansi data
SCORE_THRESHOLD terlalu rendah → noise masuk
SCORE_THRESHOLD terlalu tinggi → informasi penting bisa terbuang
🧠 Kesimpulan Umum

Eksperimen menunjukkan bahwa performa RAG sangat bergantung pada kombinasi parameter, bukan satu parameter saja.

Parameter paling berpengaruh:

CHUNK_SIZE
TOP_K_INITIAL
USE_RERANKER

Parameter pendukung:

FINAL_TOP_M mengatur fokus jawaban
TEMPERATURE mengatur gaya output
MODEL mengatur kualitas reasoning
Trade-off utama:

Semakin akurat → semakin lambat
Semakin cepat → semakin berisiko kehilangan konteks

> 💡 **Tips:** Mulai dengan `llama-3.1-8b-instant` untuk development. Saat butuh akurasi tinggi, switch ke `llama-3.3-70b-versatile` atau `openai/gpt-oss-120b`.

**Pertanyaan Refleksi:**

* Apakah peningkatan kualitas jawaban dari model besar sebanding dengan penambahan waktu? Jelaskan.
  > Jawab: ...

* Model mana yang paling cocok untuk use-case dokumenmu? Mengapa?
  > Jawab: ...

---

## ⚙️ Parameter Tambahan (Non-Tunable)

Parameter ini sudah di-set optimal. Ubah hanya jika paham dampaknya.

```python
CHUNK_OVERLAP = 60     # overlap antar chunk (kata)
SCORE_THRESHOLD = 0.30 # skor minimum cosine similarity
```

**Penjelasan:**

| Parameter | Nilai Default | Fungsi |
| --- | --- | --- |
| `CHUNK_OVERLAP` | 60 | *(Jelaskan fungsi overlap antar chunk dan apa dampaknya jika terlalu besar/kecil)* |
| `SCORE_THRESHOLD` | 0.30 | *(Jelaskan fungsi threshold skor dan kapan harus dinaikkan/diturunkan)* |

---

## 📝 Tips Eksperimen

* Pakai PDF minimal 1 halaman, maksimal 10 halaman, biar gak berat
* Coba berbagai jenis pertanyaan: **definisi**, **konteks**, dan **reasoning**
* Catat kapan sistem salah & mengapa
* Ubah **satu parameter pada satu waktu** agar bisa melihat pengaruhnya secara jelas
* Prioritaskan kualitas insight dalam laporan

---

## 🔗 Referensi

* Dokumentasi lengkap parameter: buka `docs.html` di browser atau akses via menu **📖 Dokumentasi** di RAG Playground
* Source code: `app.py` — semua parameter ada di bagian `KONFIGURASI INTI`
