# Tugas Besar ERP: Implementasi Mini ERPNext

## A. Bentuk Tugas

Mahasiswa membentuk kelompok yang terdiri dari **5 orang**. Setiap kelompok membangun dan mengimplementasikan sistem **Mini ERP** menggunakan **ERPNext**.

Instalasi dapat dilakukan melalui:

1. **Cloud** — _Lebih DIREKOMENDASIKAN_ karena mudah diakses saat presentasi dan dapat diakses bersamaan.
2. **Localhost** — Menggunakan Docker atau instalasi lokal pada komputer.

Setiap kelompok wajib memahami dan mengimplementasikan modul:

- Pembelian (_Purchasing_)
- Persediaan/Gudang (_Inventory and Warehouse_)
- Penjualan (_Sales_)
- Keuangan/Akuntansi (_Finance and Accounting_)

---

## B. Tujuan Pembelajaran

Setelah menyelesaikan tugas, mahasiswa diharapkan mampu:

1. Melakukan instalasi dan konfigurasi awal ERPNext.
2. Menjelaskan arsitektur aplikasi ERPNext.
3. Mengatur pengguna, role, dan hak akses.
4. Menghubungkan proses pembelian, persediaan, penjualan, dan akuntansi.
5. Melakukan demonstrasi transaksi ERP secara terintegrasi.
6. Membaca laporan stok, penjualan, pembelian, dan keuangan.

---

## C. Pembagian Peran Anggota

| Anggota       | Role                                      | Tanggung Jawab                                                                                    |
| :------------ | :---------------------------------------- | :------------------------------------------------------------------------------------------------ |
| **Anggota 1** | System Administrator / Solution Architect | Instalasi, konfigurasi perusahaan, user, role, dan penjelasan arsitektur.                         |
| **Anggota 2** | Procurement Officer                       | Supplier, item, permintaan pembelian, Purchase Order, Purchase Receipt, dan Purchase Invoice.     |
| **Anggota 3** | Warehouse / Inventory Officer             | Gudang, stok awal, penerimaan barang, transfer stok, Stock Ledger, dan Stock Balance.             |
| **Anggota 4** | Sales Officer                             | Customer, quotation, Sales Order, Delivery Note, dan Sales Invoice.                               |
| **Anggota 5** | Finance / Accounting Officer              | Chart of Accounts, pembayaran, General Ledger, Accounts Receivable/Payable, dan laporan keuangan. |

> **Catatan:** Setiap anggota wajib melakukan demonstrasi sesuai dengan role masing-masing.

---

## D. Skenario Mini ERP

Kelompok membuat satu perusahaan fiktif, misalnya perusahaan perdagangan atau distributor.

### Alur Minimal yang Harus Berjalan

- **Proses Pembelian:**  
  $\text{Supplier} \rightarrow \text{Purchase Order} \rightarrow \text{Purchase Receipt} \rightarrow \text{Purchase Invoice} \rightarrow \text{Payment Entry}$
- **Proses Persediaan:**  
  $\text{Purchase Receipt} \rightarrow \text{Stok Bertambah} \rightarrow \text{Stock Ledger} \rightarrow \text{Stock Balance}$
- **Proses Penjualan:**  
  $\text{Customer} \rightarrow \text{Sales Order} \rightarrow \text{Delivery Note} \rightarrow \text{Sales Invoice} \rightarrow \text{Payment Entry}$
- **Proses Akuntansi:**  
  $\text{Purchase Invoice dan Sales Invoice} \rightarrow \text{Jurnal Otomatis} \rightarrow \text{General Ledger} \rightarrow \text{Laporan Keuangan}$

### Data Minimal yang Harus Dibuat

- 1 Perusahaan
- 2 Supplier
- 2 Customer
- Minimal 5 Item/barang
- Minimal 1 Gudang
- Satuan barang
- Harga pembelian dan penjualan
- User dan role masing-masing anggota
- _Chart of Accounts_
- Transaksi pembelian dan penjualan

---

## E. Presentasi Pertama

- **Topik:** Instalasi dan Arsitektur ERPNext
- **Jadwal:** Presentasi dilakukan setiap hari satu kelompok.
- **Durasi:** Setiap anggota minimal menyampaikan bagian selama **2–3 menit**.

### Materi yang Harus Dijelaskan

1. Gambaran umum ERPNext.
2. Alasan memilih _cloud_ atau _local installation_.
3. Tahapan instalasi ERPNext.
4. Struktur arsitektur ERPNext.
5. Komponen utama sistem:
   - Web browser
   - ERPNext / Frappe application
   - Database MariaDB
   - Redis
   - Background workers
   - File storage
6. Konfigurasi awal perusahaan.
7. Pembuatan _user_ dan _role_.
8. Pembagian tugas setiap anggota.
9. Rencana implementasi Mini ERP.

---

## F. Presentasi Kedua

- **Topik:** Implementasi Mini ERP
- **Fokus:** Demonstrasi langsung sistem secara terintegrasi (menunjukkan hubungan antarproses, bukan sekadar menu terpisah).

### Urutan Demo yang Disarankan

1. Login berdasarkan role masing-masing.
2. Menampilkan _master data_.
3. Membuat transaksi pembelian.
4. Menunjukkan perubahan stok.
5. Membuat transaksi penjualan.
6. Menampilkan pengurangan stok.
7. Membuat invoice dan pembayaran.
8. Menampilkan jurnal akuntansi otomatis.
9. Menampilkan laporan:
   - Stock Balance
   - Stock Ledger
   - Purchase Summary
   - Sales Summary
   - General Ledger
   - Accounts Receivable
   - Accounts Payable
   - Trial Balance

---

## G. Dokumen yang Dikumpulkan

Setiap kelompok wajib mengumpulkan:

1. File presentasi pertama.
2. File presentasi kedua.
3. Laporan implementasi.
4. Alamat sistem _cloud_ atau panduan akses _localhost_.
5. Daftar user dan role.
6. _Screenshot_ atau rekaman proses implementasi.
7. _Backup database_ atau _export_ konfigurasi (jika menggunakan _local installation_).
8. Kesimpulan dan kendala implementasi.

---

## H. Komponen Penilaian

| Komponen                                            |  Bobot   |
| :-------------------------------------------------- | :------: |
| Instalasi dan konfigurasi sistem                    |   15%    |
| Penjelasan arsitektur ERPNext                       |   15%    |
| Kelengkapan master data                             |   15%    |
| Integrasi pembelian, stok, penjualan, dan akuntansi |   25%    |
| Demonstrasi sesuai role masing-masing               |   20%    |
| Laporan dan kualitas presentasi                     |   10%    |
| **Total**                                           | **100%** |

---

## I. Ketentuan Penting

- Setiap anggota wajib memiliki role dan tanggung jawab yang jelas.
- Setiap anggota wajib ikut presentasi.
- Data transaksi harus dibuat sendiri oleh kelompok.
- Presentasi harus menggunakan sistem yang dapat didemonstrasikan.
- _Cloud_ lebih direkomendasikan agar sistem dapat diakses langsung saat presentasi.
- Jika menggunakan localhost, kelompok wajib menyiapkan laptop dan memastikan sistem berjalan lancar.
- **Dilarang** menggunakan data pribadi atau data perusahaan asli.
- Sistem harus menunjukkan alur yang saling terhubung, bukan demonstrasi menu secara terpisah.
