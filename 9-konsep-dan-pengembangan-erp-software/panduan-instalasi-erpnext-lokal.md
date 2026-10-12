# Panduan Lengkap Instalasi & Penggunaan ERPNext (Lokal + Cloudflare Tunnel)

Dokumen ini berisi panduan langkah demi langkah untuk menginstal ERPNext secara lokal menggunakan Docker di OS Windows, serta cara mempublikasikannya agar dapat diakses oleh anggota kelompok lain melalui internet.

---

## 1. Spesifikasi Minimum & Rekomendasi (Host/Tuan Rumah)
Laptop yang akan dijadikan *server* (host) harus memenuhi kriteria berikut agar ERPNext berjalan lancar tanpa membuat laptop *hang*:
* **Prosesor (CPU):** Minimal Intel Core i3 (Gen 8 ke atas) atau AMD Ryzen 3. Direkomendasikan Intel Core i5/Ryzen 5 ke atas (seri H/P lebih baik).
* **RAM:** Minimal 8 GB. **Sangat direkomendasikan 16 GB** untuk pengerjaan kelompok yang intensif.
* **Penyimpanan:** Wajib SSD (Solid State Drive) dengan ruang kosong minimal **20 GB** di partisi sistem (Drive C:).
* **Sistem Operasi:** Windows 10 (versi 2004 ke atas) atau Windows 11 (64-bit).

## 2. Informasi Penting & Pengecekan Pra-Instalasi
Sebelum mengunduh apa pun, pastikan 3 hal ini sudah terpenuhi:
1. **Virtualisasi Aktif (Krusial):** Masuk ke Task Manager Windows -> tab *Performance* -> klik *CPU*. Lihat di bagian kanan bawah, pastikan status **Virtualization: Enabled**. Jika *Disabled*, Anda harus masuk ke BIOS/UEFI laptop saat *restart* dan mengaktifkan fitur *Virtualization Technology (VT-x / AMD-V)*.
2. **Koneksi Internet:** Dibutuhkan kuota sekitar 4-5 GB. Pastikan koneksi sangat stabil terutama saat Tahap 2 agar unduhan *image* tidak *corrupt*.
3. **Konflik Port:** Pastikan tidak ada aplikasi *web server* lokal yang sedang berjalan (seperti XAMPP, Apache, MySQL lokal, atau Skype) karena akan bertabrakan memperebutkan akses *port* jaringan.

---

## 3. Tahapan Instalasi Detail

### Tahap 1: Persiapan Sistem & Perangkat (Infrastruktur)
*Estimasi Kuota: ~700 MB*

1. **Aktivasi WSL 2 (Windows Subsystem for Linux)**
   * Buka *Start Menu* Windows, ketik **Turn Windows features on or off**, lalu tekan *Enter*.
   * Centang kotak **Virtual Machine Platform** dan **Windows Subsystem for Linux**.
   * Klik **OK**, tunggu prosesnya, lalu **Restart** laptop.
2. **Instalasi Docker Desktop**
   * **Unduh:** [Docker Desktop for Windows (x86_64)](https://docs.docker.com/desktop/install/windows-install/)
   * **Instalasi:** Jalankan *installer*. Pastikan opsi **Use WSL 2 instead of Hyper-V** tercentang. Selesaikan instalasi.
3. **Instalasi Git**
   * **Unduh:** [Git for Windows (64-bit)](https://git-scm.com/download/win)
   * **Instalasi:** Jalankan dan klik *Next* terus hingga selesai (gunakan pengaturan *default*).
4. **Proteksi RAM (Wajib Dilakukan!)**
   * Buka *File Explorer*, masuk ke folder profil: `C:\Users\NamaAkunLaptop`
   * Klik kanan -> *New* -> *Text Document*. Ubah namanya menjadi tepat **`.wslconfig`** (hapus akhiran `.txt`). *Catatan: Jika Windows menyembunyikan ekstensi file, aktifkan opsi 'File name extensions' di tab View*.
   * Buka `.wslconfig` dengan Notepad, ketik kode berikut, lalu simpan:
     ```text
     [wsl2]
     memory=8GB
     ```
     *(Ubah menjadi `memory=4GB` jika total RAM laptop hanya 8GB).*
5. **Kloning Repositori ERPNext**
   * Buka **Command Prompt (CMD)**, ketik perintah berikut lalu *Enter*:
     ```cmd
     git clone https://github.com/frappe/frappe_docker.git
     ```

**Pengecekan Tahap 1:** Buka CMD baru, jalankan `docker --version` dan `git --version`. Jika memunculkan angka versi, tahap ini berhasil.

---

### Tahap 2: Perakitan ERPNext (Docker Pull & Build)
*Estimasi Kuota: ~2,5 GB hingga 3 GB*

1. **Konfigurasi Lingkungan (Environment)**
   * Di CMD, masuk ke folder unduhan tadi:
     ```cmd
     cd frappe_docker
     ```
   * Salin file konfigurasi standar:
     ```cmd
     copy example.env .env
     ```
2. **Proses Perakitan & Unduh Image**
   * Di CMD yang sama, jalankan perintah rahasia pembuatannya:
     ```cmd
     docker compose -f pwd.yml up -d
     ```
   * *Tunggu dengan sabar. Proses ini bisa memakan waktu 15-45 menit tergantung kecepatan internet.*
3. **Inisialisasi Situs & Database ERPNext**
   * Setelah proses di atas selesai dan CMD bisa diketik kembali, jalankan perintah ini untuk membuat *database* (ubah kata `sandiadmin` dengan *password* yang kelompok Anda sepakati):
     ```cmd
     docker compose -f pwd.yml exec backend bench new-site tugaserp.localhost --admin-password "sandiadmin"
     ```
   * Buka *browser* di laptop tersebut, ketik: **`http://tugaserp.localhost:8080`**
   * *Login* menggunakan *username*: **Administrator** dan password yang baru Anda buat. Selesaikan *setup wizard* ERPNext.

**Pengecekan Tahap 2:** Di CMD, ketik `docker ps`. Jika muncul daftar aplikasi dengan status *Up*, instalasi berhasil.

**Solusi Error Umum Tahap 2:**
* **Error `Unexpected EOF` / *Timeout*:** Internet sempat terputus. Tekan panah atas di *keyboard* untuk memanggil ulang perintah `docker compose -f pwd.yml up -d`, lalu *Enter*. Docker akan melanjutkan sisa unduhan.
* **Error `port is already allocated`:** Ada aplikasi lain yang memakai *port* 8080. Matikan aplikasi tersebut (biasanya XAMPP).

---

### Tahap 3: Publikasi via Cloudflare Tunnel (Aksesibilitas)
*Estimasi Kuota: ~30 MB*

1. **Unduh Cloudflared**
   * **Unduh:** [cloudflared-windows-amd64.exe](https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-windows-amd64.exe)
2. **Menjalankan Terowongan**
   * Pindahkan file `.exe` tersebut ke Drive D (contoh: buat folder `D:\Cloudflare`).
   * Buka CMD baru, arahkan ke folder tersebut:
     ```cmd
     cd D:\Cloudflare
     ```
   * Jalankan perintah ini untuk membuka akses:
     ```cmd
     cloudflared-windows-amd64.exe tunnel --url http://localhost:8080
     ```
3. **Bagikan Tautan**
   * Perhatikan teks di CMD. Cari baris yang mengandung tulisan **`https://xxxxx.trycloudflare.com`**.
   * Salin URL acak tersebut dan bagikan ke grup WhatsApp kelompok Anda.

---

## 4. Panduan Penggunaan Harian (SOP Kelompok)

### A. Untuk Pemilik Laptop (Tuan Rumah / Host)
Anda bertugas sebagai penyedia *server*. Anda tidak perlu mengulang Tahap 1 dan Tahap 2 setiap hari. 

**Cara Menyalakan Server (Saat Kelompok Ingin Mengerjakan Tugas):**
1. Buka aplikasi **Docker Desktop** dari *Start Menu*. Biarkan aplikasi menyala di latar belakang.
2. Buka CMD, masuk ke folder Cloudflare (`cd D:\Cloudflare`).
3. Jalankan ulang perintah *tunnel*: `cloudflared-windows-amd64.exe tunnel --url http://localhost:8080`
4. Berikan URL `.trycloudflare.com` yang baru kepada teman Anda (URL ini akan selalu berubah setiap kali CMD dijalankan ulang).
5. Biarkan CMD tersebut tetap terbuka selama teman-teman Anda sedang bekerja.

**Cara Mematikan Server (Saat Selesai Mengerjakan Tugas):**
1. Tutup jendela CMD Cloudflare tersebut (tanda *X* di sudut kanan atas). Akses internet akan langsung terputus.
2. Buka aplikasi Docker Desktop, masuk ke menu **Containers**. Cari grup *frappe_docker*, lalu tekan tombol **Stop** (ikon kotak).
3. Anda sekarang boleh mematikan (*shut down*) laptop dengan aman. Data otomatis tersimpan.

### B. Untuk Anggota Kelompok (Klien)
1. Anda hanya perlu menunggu tautan Cloudflare terbaru dari teman yang menjadi Host.
2. Buka tautan tersebut di *browser* (Chrome/Edge/Firefox) di laptop Anda masing-masing.
3. *Login* menggunakan kredensial yang sudah disepakati (misal *username*: Administrator).
4. **Jika Anda menemui error `502 Bad Gateway` atau *Loading* abadi:** Ini berarti laptop Host sedang mati, Docker belum dinyalakan, atau CMD Cloudflare di laptop Host tidak sengaja tertutup. Hubungi teman Anda (Host) untuk menyalakannya kembali.