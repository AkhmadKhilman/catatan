# MATERI TAMBAHAN SLIDE PRESENTASI: STUDI KASUS NAIVE BAYES

_(Panduan untuk dimasukkan ke Slide 5, Slide 6, Slide 7, dan Slide 8)_

---

## SLIDE 5: SOAL 1 (Data Knowledge Base Sistem Pakar)

### Basis Pengetahuan (Knowledge Base) Klinik

Sebuah Sistem Pakar memiliki data rekam medis untuk **4 penyakit** beserta nilai Probabilitas Awal $P(H)$ dan 3 Gejala Utama $P(E|H)$ sebagai berikut:

| Kode & Nama Penyakit ($H$)      | Probabilitas Awal $P(H)$ | 3 Gejala Utama & Nilai $P(E\|H)$                                                                                     |
| :------------------------------ | :----------------------: | :------------------------------------------------------------------------------------------------------------------- |
| **$H_1$: Flu (Influenza)**      |         **0,4**          | • $E_1$ (Batuk) = **0,8**<br>• $E_2$ (Pilek) = **0,7**<br>• $E_3$ (Sakit Tenggorokan) = **0,5**                      |
| **$H_2$: Demam Berdarah (DBD)** |         **0,2**          | • $E_4$ (Demam Tinggi) = **0,8**<br>• $E_5$ (Bintik Merah) = **0,8**<br>• $E_6$ (Nyeri Sendi) = **0,7**              |
| **$H_3$: Maag (Gastritis)**     |         **0,25**         | • $E_7$ (Nyeri Ulu Hati) = **0,9**<br>• $E_8$ (Perut Kembung) = **0,8**<br>• $E_9$ (Sering Bersendawa) = **0,7**     |
| **$H_4$: Muntaber**             |         **0,15**         | • $E_{10}$ (Diare / BAB Cair) = **0,9**<br>• $E_{11}$ (Mual & Muntah) = **0,8**<br>• $E_{12}$ (Kram Perut) = **0,7** |

> **Aturan Preventif Sistem (Laplace Smoothing):**
> Apabila pasien mengeluhkan gejala yang **tidak terdaftar (bernilai 0)** pada penyakit yang sedang diuji, maka gejala tersebut otomatis diberi **nilai bantuan terkecil sebesar 0,25** agar perhitungan perkalian tidak menghasilkan angka 0.

---

## SLIDE 6: JAWABAN SOAL 1 (Kasus Pasien & Penyaringan Kandidat)

### Studi Kasus Pasien

Seorang pasien datang ke klinik dan mengeluhkan **5 gejala** sekaligus:

1. **Batuk ($E_1$)**
2. **Pilek ($E_2$)**
3. **Sakit Tenggorokan ($E_3$)**
4. **Demam Tinggi ($E_4$)**
5. **Bintik Merah ($E_5$)**

### Tahap 1: Analisis & Penyaringan Kandidat Penyakit

- **Eliminasi Penyakit Tidak Relevan:** Dari 5 gejala yang dialami pasien, tidak ada satu pun yang merujuk pada gejala **Maag ($H_3$)** maupun **Muntaber ($H_4$)**. Sistem mengeliminasi $H_3$ dan $H_4$, sehingga hanya menghitung 2 kandidat penyakit yang relevan: **Flu ($H_1$)** dan **DBD ($H_2$)**.
- **Penerapan Nilai Bantuan (0,25):** Pasien mengalami 3 gejala milik Flu ($E_1, E_2, E_3$) dan 2 gejala milik DBD ($E_4, E_5$), sedangkan gejala Nyeri Sendi ($E_6$) tidak dialami pasien sehingga diabaikan. Gejala yang bernilai 0 pada masing-masing penyakit diubah menjadi **0,25**:

| Komponen yang Dihitung         | Nilai pada **Flu ($H_1$)** | Nilai pada **DBD ($H_2$)** |
| :----------------------------- | :------------------------: | :------------------------: |
| **Probabilitas Awal / $P(H)$** |          **0,4**           |          **0,2**           |
| $E_1$: Batuk                   |            0,8             |    **0,25** _(bantuan)_    |
| $E_2$: Pilek                   |            0,7             |    **0,25** _(bantuan)_    |
| $E_3$: Sakit Tenggorokan       |            0,5             |    **0,25** _(bantuan)_    |
| $E_4$: Demam Tinggi            |    **0,25** _(bantuan)_    |            0,8             |
| $E_5$: Bintik Merah            |    **0,25** _(bantuan)_    |            0,8             |

---

## SLIDE 7: SOAL 2 (Perhitungan Probabilitas Tiap Kelas)

### Tahap 2: Menghitung Nilai $P(E|H) \times P(H)$ untuk Setiap Kelas

Sistem mengalikan nilai peluang ke-5 gejala pasien dengan peluang awal masing-masing penyakit:

**1. Perhitungan Kandidat Penyakit Flu ($H_1$):**
$$\text{Skor Flu} = (P(E_1|H_1) \times P(E_2|H_1) \times P(E_3|H_1) \times P(E_4|H_1) \times P(E_5|H_1)) \times P(H_1)$$
$$\text{Skor Flu} = (0,8 \times 0,7 \times 0,5 \times 0,25 \times 0,25) \times 0,4$$
$$\text{Skor Flu} = 0,0175 \times 0,4 = \mathbf{0,007}$$

**2. Perhitungan Kandidat Penyakit DBD ($H_2$):**
$$\text{Skor DBD} = (P(E_1|H_2) \times P(E_2|H_2) \times P(E_3|H_2) \times P(E_4|H_2) \times P(E_5|H_2)) \times P(H_2)$$
$$\text{Skor DBD} = (0,25 \times 0,25 \times 0,25 \times 0,8 \times 0,8) \times 0,2$$
$$\text{Skor DBD} = 0,01 \times 0,2 = \mathbf{0,002}$$

**3. Menghitung Total Probabilitas Evidence $P(E)$:**
$$P(E) = \text{Skor Flu} + \text{Skor DBD}$$
$$P(E) = 0,007 + 0,002 = \mathbf{0,009}$$

---

## SLIDE 8: JAWABAN SOAL 2 (Persentase Akhir & Keputusan Pakar)

### Tahap 3: Normalisasi Persentase & Penentuan Kelas Tertinggi

Membagi skor masing-masing kandidat penyakit dengan nilai total $P(E)$ menggunakan rumus utuh Naive Bayes:

$$P(H|E) = \frac{P(E|H) \times P(H)}{P(E)}$$

- **Persentase Akhir Penyakit Flu ($P(H_1|E)$):**
  $$P(H_1|E) = \frac{0,007}{0,009} = 0,778 \rightarrow \mathbf{77,8\%}$$

- **Persentase Akhir Penyakit DBD ($P(H_2|E)$):**
  $$P(H_2|E) = \frac{0,002}{0,009} = 0,222 \rightarrow \mathbf{22,2\%}$$

- **Total Persentase Keseluruhan:**
  $$77,8\% + 22,2\% = \mathbf{100\%}$$

### Keputusan Akhir Sistem Pakar

Berdasarkan perbandingan nilai probabilitas di atas, penyakit **Flu memperoleh nilai tertinggi sebesar 77,8%**, sedangkan **DBD memperoleh nilai 22,2%**. Dengan demikian, Sistem Pakar memutuskan bahwa pasien tersebut terdiagnosis menderita penyakit **Flu (Influenza)**.
