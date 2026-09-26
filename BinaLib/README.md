# BinaLib

BinaLib adalah sistem informasi perpustakaan untuk SMK Bina Rahayu yang dirancang untuk membantu proses peminjaman, pengembalian, pengelolaan data buku, data siswa, dan transaksi perpustakaan secara lebih terstruktur.

Project ini dikembangkan sebagai bagian dari kegiatan Pengabdian kepada Masyarakat (PkM) dengan fokus pada digitalisasi proses perpustakaan.

> **Status:** Development / Prototype

## Fitur

### Admin

- Dashboard perpustakaan
- Pengelolaan data siswa
- Registrasi dan pengelolaan RFID siswa
- Pengelolaan data buku
- Pengelolaan transaksi peminjaman
- Pengelolaan pengembalian
- Riwayat transaksi
- Laporan perpustakaan
- Pengaturan sistem

### User / Self-Service

- Identifikasi siswa menggunakan RFID
- Tampilan informasi siswa setelah RFID terdeteksi
- Peminjaman buku secara mandiri
- Pilihan penggunaan:
  - **Kegunaan Kelas** — dapat memilih lebih dari satu buku
  - **Penggunaan Pribadi** — maksimal satu buku
- Konfirmasi transaksi sebelum peminjaman
- Pengembalian buku melalui sistem

## Alur RFID

RFID digunakan untuk **mengidentifikasi siswa, bukan untuk mengidentifikasi buku**.

```text
Siswa
  ↓
Scan RFID
  ↓
BinaLib membaca UID RFID
  ↓
Sistem mencari data siswa
  ↓
Identitas siswa ditampilkan
  ↓
Pilih jenis penggunaan
  ↓
Pilih buku
  ↓
Konfirmasi
  ↓
Transaksi tersimpan
```

Reader RFID yang digunakan dapat bekerja sebagai **keyboard emulation**, sehingga UID kartu dapat diterima oleh input pada aplikasi tanpa memerlukan mikrokontroler seperti Arduino atau ESP32.

Untuk development tanpa perangkat RFID, UID dapat dimasukkan secara manual.

## Jenis Peminjaman

### Kegunaan Kelas

Digunakan ketika buku dipinjam untuk kegiatan pembelajaran di kelas. Siswa dapat memilih beberapa buku sekaligus dan menentukan jumlah buku yang diperlukan.

### Penggunaan Pribadi

Digunakan ketika siswa meminjam buku untuk kebutuhan pribadi. Dalam mode ini, siswa hanya dapat memilih maksimal satu buku.

## Teknologi

- PHP Native
- MySQL / MariaDB
- HTML5
- CSS3
- JavaScript
- PDO
- RFID USB Reader

Akses database menggunakan PDO dan konfigurasi koneksi dapat mengambil nilai dari environment variable.

## Struktur Project

```text
BinaLib/
├── assets/
│   ├── login-bg-binar.png
│   └── logo-bina-rahayu.png
├── config/
│   └── database.php
├── includes/
│   └── auth.php
├── user/
│   ├── index.php
│   ├── user.css
│   └── user.js
├── api.php
├── binalib.js
├── dashboard.php
├── data-buku.php
├── data-siswa.php
├── index.php
├── laporan.php
├── pengaturan.php
├── registrasi-rfid.php
├── riwayat.php
├── simple.css
├── simple-v4.css
├── .gitattributes
├── .gitignore
└── README.md
```

## Persyaratan

Untuk menjalankan BinaLib secara lokal, diperlukan:

- PHP 8.x atau versi yang kompatibel
- MySQL / MariaDB
- Apache
- Browser modern
- RFID USB Reader untuk pengujian RFID

Server lokal dapat menggunakan:

- XAMPP
- Laragon
- LAMP
- Apache + PHP + MySQL/MariaDB secara manual

## Instalasi Lokal

### 1. Clone repository

```bash
git clone https://github.com/USERNAME/BinaLib.git
cd BinaLib
```

Ganti `USERNAME` dengan username GitHub pemilik repository.

### 2. Letakkan project pada web server

Contoh menggunakan XAMPP:

```text
C:\xampp\htdocs\BinaLib
```

Contoh menggunakan LAMPP di Linux:

```text
/opt/lampp/htdocs/BinaLib
```

### 3. Buat database

Buat database MySQL/MariaDB untuk BinaLib, misalnya:

```sql
CREATE DATABASE binalib;
```

> Repository ini tidak menyertakan dump database produksi. Gunakan schema atau database development yang sesuai dengan environment kamu.

### 4. Konfigurasi database

`config/database.php` membaca konfigurasi dari environment variable berikut:

```text
BLS_DB_HOST
BLS_DB_NAME
BLS_DB_USER
BLS_DB_PASS
```

Contoh konfigurasi environment:

```text
BLS_DB_HOST=127.0.0.1
BLS_DB_NAME=binalib
BLS_DB_USER=root
BLS_DB_PASS=
```

Jangan commit password database production atau credential lainnya ke repository publik.

### 5. Jalankan aplikasi

Buka project melalui web server, misalnya:

```text
http://localhost/BinaLib/
```

## Pengujian RFID

Pastikan RFID reader sudah terhubung ke komputer.

Karena reader menggunakan keyboard emulation, proses pengujian dapat dilakukan dengan:

```text
1. Buka halaman self-service BinaLib.
2. Fokuskan cursor pada input RFID.
3. Tempelkan kartu RFID.
4. Reader mengirimkan UID.
5. Sistem mencari siswa berdasarkan UID.
6. Siswa dapat melanjutkan proses peminjaman.
```

## Konsep Data

Data utama yang digunakan BinaLib meliputi:

### Siswa

- ID
- NIS
- Nama
- Kelas
- RFID UID
- Status

### Buku

- ID
- Kode Buku
- Judul
- Penulis
- Tahun
- Stok

### Transaksi

- ID
- Siswa
- Buku
- Jenis Penggunaan
- Jumlah
- Waktu Peminjaman
- Waktu Pengembalian
- Status

RFID UID berfungsi sebagai identitas kartu yang terhubung dengan data siswa. Jika kartu siswa diganti, UID dapat diperbarui tanpa harus membuat data siswa baru.

## Keamanan

Untuk deployment sebenarnya, beberapa hal perlu diperhatikan:

- Gunakan password yang sudah di-hash.
- Jangan commit password database ke GitHub.
- Gunakan prepared statement untuk query database.
- Validasi input dari user.
- Batasi akses halaman admin.
- Gunakan HTTPS pada server production.
- Pisahkan konfigurasi development dan production.
- Jangan menyimpan credential atau secret di repository publik.
- Jangan mengunggah data siswa atau transaksi sekolah yang bersifat internal.

## Status Project

BinaLib saat ini berada pada tahap **development / prototype**. Beberapa bagian masih dapat disesuaikan berdasarkan kondisi server, database, autentikasi, dan perangkat RFID yang digunakan oleh sekolah.

## Pengembangan Selanjutnya

Beberapa pengembangan yang dapat dilakukan:

- Integrasi database asli sekolah
- Integrasi akun sekolah / Google Workspace jika diperlukan
- Autentikasi admin dengan 2FA
- Integrasi RFID reader secara penuh
- Penyempurnaan self-service
- Backup dan restore database
- Laporan transaksi yang dapat diekspor
- Deployment pada server sekolah
- Penyesuaian dengan kebijakan dan infrastruktur jaringan sekolah

## Kontribusi

Project ini dikembangkan untuk kebutuhan akademik dan kegiatan Pengabdian kepada Masyarakat.

Perubahan, perbaikan bug, dan pengembangan fitur dapat dilakukan sesuai kebutuhan sistem dan hasil evaluasi penggunaan di lingkungan sekolah.

## Lisensi

Repository ini belum menetapkan lisensi open-source. Penggunaan kembali, distribusi, atau modifikasi di luar kebutuhan project sebaiknya mendapatkan persetujuan dari pihak pengembang dan pihak terkait.
