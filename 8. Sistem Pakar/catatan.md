Di Sistem Pakar ada 2 cara mesin berpikir:

1. FORWARD CHAINING (Maju)
   Alur: Dari Fakta -> ke Kesimpulan
   Cara kerja: Kumpulin semua data yang ada dulu, baru tarik kesimpulan.

_Analogi: Dokter lihat gejala._
_Fakta: Demam + Batuk + Sesak = Kesimpulan: Flu / Covid._

**Dipakai kalau: Datanya banyak, tujuannya belum jelas. Sistem coba semua kemungkinan.**

2. BACKWARD CHAINING (Mundur)
   Alur: Dari Kesimpulan -> cari Fakta pembuktinya
   Cara kerja: Tentuin dugaan dulu, baru cek apakah buktinya ada.

_Analogi: Jaksa punya terduga._
_Dugaan: Apakah ini Covid? -> Cek: Ada demam? Ada batuk? Kalau iya, terbukti._

**Dipakai kalau: Tujuannya sudah jelas, mau buktikan benar / salah. Lebih cepat & hemat.**

## Beda 1 kalimat:

- Forward = ada fakta apa, jadi apa?
- Backward = mau buktikan apa, butuh fakta apa?

### CONTOHNYA;

```md
Fakta awal: {A, C, D} - Target: G
Rule:
R1: A -> D
R2: A & C -> E
R3: D -> K
R4: C & D -> B
R5: B & E -> K
R6: K -> G

#### 1. Forward TANPA tujuan jelas

Misi: cari semua yang mungkin.

- _WM = {A, C, D}_
- R1: A -> D (D sudah ada)
- R2: A & C -> dapat _E_. WM = {A, C, D, E}
- R3: D -> dapat _K_. WM = {A, C, D, E, K}
- R4: C & D -> dapat _B_. WM = {A, C, D, E, K, B}
- R5: B & E -> dapat K lagi (sudah ada)
- R6: K -> dapat _G_. WM = {A, C, D, E, K, B, G}
- Siklus 2: cek lagi, tidak ada fakta baru. STOP.

Hasil: Semua turunan keluar. Urutan fire: _R2 -> R3 -> R4 -> R6_. R5 jadi mubazir karena K sudah didapat dari R3.

#### 2. Forward DENGAN tujuan jelas = G

Misi: cuma mau G. Begitu G ketemu, stop.

- _WM = {A, C, D}_, Target = G
- Sistem lihat: Untuk G butuh K (R6). Untuk K paling cepat butuh D (R3).
- R3: D ada -> dapat _K_. Apakah K == G? Belum.
- R6: K ada -> dapat _G_. Apakah G == Target? YA. STOP LANGSUNG.

Urutan fire: _Cuma R3 -> R6._

Dia _tidak akan pernah_ menjalankan R2 dan R4, padahal faktanya memungkinkan. Kenapa? Karena dia sudah tahu tujuannya G, dan G sudah bisa dicapai lewat jalur tercepat D -> K -> G. Dia tidak peduli ada E dan B yang sebenarnya bisa disimpulkan juga.

Itu inti perbedaannya:

- _Tanpa tujuan = boros tapi lengkap._ Semua fakta turunan E, B, K, G ditemukan.
- _Dengan tujuan = hemat tapi fokus._ Cuma K dan G yang ditemukan, langsung selesai.
```

## Cari Tahu Mengenai;

- Tower of Hanoi (Operator)
- Certainty Factor (Cari tau)
