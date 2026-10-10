# Catatan Pembelajaran Sistem Pakar

---

## A. Daftar Metode Inferensi Sistem Pakar & Ketentuan Tugas

### 1. Daftar Metode yang Dibahas

1. **Metode Inferensi Mamdani**
2. **Metode Inferensi Sugeno**
3. **Metode Inferensi Tsukamoto**
4. **Konsep Dasar FUZZY**
5. **Metode Inferensi Certainty Factor**
6. **Metode Inferensi Uncertainty Factor**
7. **Metode Inferensi Naïve Bayes**
8. **Metode Inferensi Forward Chaining**
9. **Metode Inferensi Backward Chaining**

### 2. Ketentuan Tugas Presentasi

- **Format:** Presentasi terdiri dari **7 – 9 Halaman**.
- **Instruksi:** Pilihlah salah satu dari metode di atas serta berikan contoh kasus aplikasinya.
- **Aturan Penting:**
  - **Tidak boleh ada kasus aplikasi yang sama** antar mahasiswa/kelompok.
  - **Peringatan Nilai:** Jika ditemukan contoh kasus aplikasi yang sama, maka **nilai akhir mata kuliah adalah E**.

---

## B. Konsep Penalaran Mesin (Inference Engine)

Di dalam Sistem Pakar, terdapat 2 cara utama mesin berpikir:

### 1. Forward Chaining (Penalaran Maju)

- **Alur:** Dari **Fakta** $\rightarrow$ ke **Kesimpulan**
- **Cara Kerja:** Mengumpulkan semua data/fakta yang ada terlebih dahulu, baru menarik kesimpulan.
- **Analogi:** _Dokter memeriksa gejala._  
  $\text{Fakta: Demam + Batuk + Sesak} \rightarrow \text{Kesimpulan: Flu / Covid}$
- **Kapan Digunakan:** Dipakai saat datanya banyak, namun tujuannya belum spesifik/jelas. Sistem mengevaluasi semua kemungkinan.

### 2. Backward Chaining (Penalaran Mundur)

- **Alur:** Dari **Kesimpulan/Hipotesis** $\rightarrow$ mencari **Fakta Pembukti**
- **Cara Kerja:** Menentukan dugaan terlebih dahulu, kemudian menelusuri ke belakang apakah bukti-buktinya terpenuhi.
- **Analogi:** _Jaksa yang sudah memiliki tersangka/terduga._  
  $\text{Dugaan: Apakah pasien terkena Covid?} \rightarrow \text{Cek: Apakah ada demam? Batuk? Jika ada, terbukti.}$
- **Kapan Digunakan:** Dipakai jika tujuannya sudah jelas dan hanya ingin membuktikan benar atau salah. Cenderung lebih cepat dan efisien.

### Rangkuman Singkat:

- **Forward:** Ada fakta apa $\rightarrow$ menghasilkan apa?
- **Backward:** Mau membuktikan apa $\rightarrow$ membutuhkan fakta apa?

---

## C. Contoh Kasus & Simulasi Penelusuran Rules

### Basis Data Kasus:

- **Fakta Awal (Working Memory / WM):** $\{A, C, D\}$
- **Target / Goal:** $G$
- **Kumpulan Rule:**
  - $R_1: A \rightarrow D$
  - $R_2: A \land C \rightarrow E$
  - $R_3: D \rightarrow K$
  - $R_4: C \land D \rightarrow B$
  - $R_5: B \land E \rightarrow K$
  - $R_6: K \rightarrow G$

---

### Skenario 1: Forward Chaining TANPA Tujuan Jelas

> **Misi:** Mengeksplorasi seluruh fakta turunan yang memungkinkan.

1. $\text{WM awal} = \{A, C, D\}$
2. **$R_1: A \rightarrow D$** (Fakta $D$ sudah ada di WM, dilewati)
3. **$R_2: A \land C \rightarrow E$** $\rightarrow$ Menghasilkan fakta baru $E$.  
   $$\text{WM} = \{A, C, D, E\}$$
4. **$R_3: D \rightarrow K$** $\rightarrow$ Menghasilkan fakta baru $K$.  
   $$\text{WM} = \{A, C, D, E, K\}$$
5. **$R_4: C \land D \rightarrow B$** $\rightarrow$ Menghasilkan fakta baru $B$.  
   $$\text{WM} = \{A, C, D, E, K, B\}$$
6. **$R_5: B \land E \rightarrow K$** $\rightarrow$ Menghasilkan $K$ kembali (fakta $K$ sudah ada di WM).
7. **$R_6: K \rightarrow G$** $\rightarrow$ Menghasilkan fakta baru $G$.  
   $$\text{WM} = \{A, C, D, E, K, B, G\}$$
8. **Siklus 2:** Evaluasi ulang, tidak ditemukan fakta baru $\rightarrow$ **STOP**.

- **Hasil:** Semua fakta turunan keluar.
- **Urutan rule aktif (_fire_):** $R_2 \rightarrow R_3 \rightarrow R_4 \rightarrow R_6$.
- _Catatan:_ $R_5$ menjadi mubazir karena fakta $K$ telah diperoleh sebelumnya dari $R_3$.

---

### Skenario 2: Forward Chaining DENGAN Tujuan Jelas ($Target = G$)

> **Misi:** Hanya mencari $G$. Begitu $G$ ditemukan, proses langsung dihentikan.

1. $\text{WM awal} = \{A, C, D\}$, $\text{Target} = G$
2. Sistem menganalisis jalur: Untuk mencapai $G$, dibutuhkan $K$ ($R_6$). Untuk mendapatkan $K$ secara langsung, cukup menggunakan fakta $D$ yang sudah ada di WM ($R_3$).
3. **$R_3$ dieksekusi:** Fakta $D$ tersedia $\rightarrow$ menghasilkan $K$.  
   _Evaluasi:_ Apakah $K == G$? (Belum).
4. **$R_6$ dieksekusi:** Fakta $K$ tersedia $\rightarrow$ menghasilkan $G$.  
   _Evaluasi:_ Apakah $G == \text{Target}$? **YA** $\rightarrow$ **STOP LANGSUNG**.

- **Urutan rule aktif (_fire_):** Hanya $R_3 \rightarrow R_6$.
- Sistem tidak perlu menjalankan $R_2$ dan $R_4$ meskipun prasyaratnya terpenuhi, karena target $G$ berhasil dicapai melalui jalur optimal $D \rightarrow K \rightarrow G$.

---

### Inti Perbandingan Eksekusi:

- **Tanpa Tujuan Spesifik:** Boros sumber daya tetapi menyeluruh (semua turunan $E, B, K, G$ dievaluasi).
- **Dengan Tujuan Spesifik:** Efisien dan terarah (hanya memproses $K$ dan $G$, lalu langsung selesai).

---

## D. Topik Eksplorasi Lanjutan

- **Tower of Hanoi** _(Eksplorasi Operator & State Space)_
- **Certainty Factor (CF)** _(Perhitungan faktor kepastian pada sistem berbasis ketidakpastian)_
