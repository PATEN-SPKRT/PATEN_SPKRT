
## PATEN-SPKRT

### 1. Deskripsi Umum Sistem
**PATEN-SPKRT** adalah sistem informasi terintegrasi yang dirancang untuk mengontrol, mengelola, dan memantau seluruh proses pengumpulan data survei/pendataan lapangan (SPKRT). Sistem ini mencakup alokasi beban kerja wilayah sampel, manajemen akun petugas, pengunggahan kuesioner fisik/digital, pencatatan/entry data, pengujian validitas dan konsistensi data, hingga penyajian laporan statistik progres secara *real-time*.

---

## 2. Peran Pengguna (User Roles & Permissions)

Sistem ini menerapkan *Role-Based Access Control* (RBAC) dengan tingkatan hak akses sebagai berikut:

| Peran Pengguna | Deskripsi Singkat & Hak Akses Utama |
| :--- | :--- |
| **Admin Provinsi** | Pengelolaan pengguna penuh, konfigurasi master data sistem, dan manajemen hak akses di seluruh tingkatan wilayah. Monitoring progres wilayah provinsi, pengawasan rekapitulasi data regional, serta penarikan laporan komprehensif. |
| **Admin Kab/Kota** | Pengelolaan akun petugas lokal, alokasi NKS (Nomor Klaster Sampel), dan alokasi DSRT (Daftar Sampel Rumah Tangga) ke petugas. |
| **PPL (Petugas Pendataan Lapangan)** | Mengakses alokasi DSRT, mengunggah (*upload*) berkas kuesioner hasil pendataan lapangan, serta melihat status unggahan. |
| **PML (Petugas Pemeriksa Lapangan)** | Memeriksa berkas unggahan PPL, melakukan *entry* data kuesioner ke dalam sistem, dan menandai kuesioner yang selesai di-entry. Melakukan validasi akurasi, konsistensi isian data, data yang telah dientry. 
---

## 3. Kebutuhan Fungsional (Functional Requirements)

### 3.1 Modul Manajemen Pengguna (User Management)
* **REQ-UM-01:** Admin Pusat dan Admin Kab/Kota dapat membuat, mengubah, menonaktifkan, dan menghapus akun pengguna sesuai dengan wewenang wilayahnya.
* **REQ-UM-02:** Sistem harus mendukung autentikasi aman (*Login/Logout*) berbasis kata sandi dan peran (*role*).
* **REQ-UM-03:** Sistem dapat memetakan (*mapping*) akun petugas (PPL & PML) ke wilayah tugas spesifik (Kabupaten/Kota, Kecamatan, Desa/Kelurahan).

### 3.2 Modul Alokasi Sampel (NKS & DSRT)
* **REQ-AS-01:** Admin Kab/Kota dapat mengunggah atau menginput Master Data NKS (Nomor Klaster Sampel).
* **REQ-AS-02:** Admin Kab/Kota dapat mengalokasikan NKS kepada PML dan PPL tertentu.
* **REQ-AS-03:** Admin Kab/Kota dapat menetapkan dan mengalokasikan DSRT (Daftar Sampel Rumah Tangga) ke dalam NKS yang telah ditentukan.

### 3.3 Modul Upload Kuesioner Lapangan (PPL)
* **REQ-UP-01:** PPL dapat melihat daftar assignment DSRT yang ditugaskan kepadanya.
* **REQ-UP-02:** PPL dapat mengunggah berkas kuesioner hasil pendataan lapangan (format PDF/Gambar) per DSRT. (Menu SPKRT.BL, SPKRT.LK, SPKRT.S)
* **REQ-UP-03:** PPL dapat mengganti/mengunggah ulang berkas kuesioner jika terdapat permintaan perbaikan dari PML.

### 3.4 Modul Entry Data (PML)
* **REQ-ED-01:** PML dapat melihat daftar berkas kuesioner yang diunggah oleh PPL bawahannya. (Menu SPKRT.BL, SPKRT.LK, SPKRT.S)
* **REQ-ED-02:** PML dapat membuka antarmuka *entry* data yang berdampingan dengan penampil berkas kuesioner (*document viewer*). 
* **REQ-ED-03:** PML dapat menyimpan draft entrian data dan menyelesaikan entrian data (*submit*).

### 3.5 Modul Validasi Data & Kontrol Kualitas
* **REQ-VA-01:** Validator dapat menjalankan *rule engine* validasi otomatis (cek field kosong, pengecekan tipe data, nilai di luar rentang/outlier).
* **REQ-VA-02:** Validator dapat melakukan uji konsistensi antar-isian kuesioner.
* **REQ-VA-03:** Validator dapat menyetujui data (*Approve*) atau menolak data (*Reject*) disertai catatan perbaikan yang dikembalikan ke PML/PPL.

### 3.6 Modul Dashboard Monitoring & Pelaporan
* **REQ-DB-01:** Dashboard menyajikan diagram visual dan matriks status pencapaian (Unggah % $\rightarrow$ Entry % $\rightarrow$ Validasi %).
* **REQ-DB-02:** Admin Provinsi dan Kab/Kota dapat memfilter progres pendataan berdasarkan wilayah, rentang tanggal, atau nama petugas.
* **REQ-DB-03:** Sistem dapat mengekspor rekapitulasi data dan laporan progres ke dalam format Excel (XLSX) atau PDF.

---

## 4. Alur Kerja & Diagram Proses (Business Process Workflow)

Berikut adalah urutan proses bisnis utama dari alokasi awal hingga data tervalidasi:

$$\text{Alokasi NKS \& DSRT (Admin Kab/Kota)} \longrightarrow \text{Pendataan \& Upload Berkasa (PPL)} \longrightarrow \text{Pemeriksaan \& Entry Data (PML)}$$

$$\text{Pemeriksaan \& Entry Data (PML)} \longrightarrow \text{Uji Validasi \& Konsistensi (Validator)} \longrightarrow \text{Monitoring \& Pelaporan (Admin Provinsi/Kab)}$$

```text
+-----------------------+      +-----------------------+      +-----------------------+
|   Admin Kab/Kota      |      |         PPL           |      |         PML           |
|-----------------------|      |-----------------------|      |-----------------------|
| 1. Buat User & Role   | ---> | 3. Lihat Alokasi DSRT | ---> | 5. Cek Upload Berkas  |
| 2. Alokasi NKS & DSRT |      | 4. Upload Kuesioner   |      | 6. Entry Data SPKRT |
+-----------------------+      +-----------------------+      +-----------------------+
                                                                          |
                                                                          v
+-----------------------+      +-----------------------+      +-----------------------+
|  Dashboard & Laporan  |      |       Data Final      |      |       PML,Admin       |
|-----------------------|      |-----------------------|      |-----------------------|
| 8. Monitor Progress   | <--- |   [ Data Validated ]  | <--- | 7. Validasi Data      |
|    & Ekspor Laporan   |      |                       |      |                       |
+-----------------------+      +-----------------------+      +-----------------------+
```

---

## 5. Kebutuhan Non-Fungsional (Non-Functional Requirements)

1. **Keamanan (Security):**
   * Penggunaan enkripsi HTTPS/TLS untuk proteksi komunikasi data.
   * Kata sandi akun wajib dienkripsi menggunakan *hashing algorithm* yang aman (misal: bcrypt).
2. **Kinerja (Performance):**
   * Sistem mampu menangani unggahan berkas hingga ukuran 10 MB per dokumen.
   * Waktu respon antarmuka dashboard monitoring kurang dari 3 detik pada kondisi beban standar.
3. **Ketersediaan & Keandalan (Availability):**
   * *Uptime* sistem ditargetkan mencapai minimal $99.5\%$.
   * Sistem melakukan pencadangan data (*auto backup*) harian pada basis data.
4. **Usabilitas (Usability):**
   * Desain antarmuka responsif (dapat diakses melalui perangkat tablet/mobile).
