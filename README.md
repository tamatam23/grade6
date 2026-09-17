🖥️ Ujian Komputer – Portal Pengumpulan Tugas Siswa
Aplikasi web berbasis Google Apps Script (GAS), Google Sheets, dan Google Drive yang dirancang khusus untuk mempermudah pelaksanaan ujian/asesmen berbasis tugas (project-based assessment) di lingkungan sekolah (SD / SMP / SMK).
Aplikasi ini mengintegrasikan portal pengumpulan tugas bagi siswa dan dashboard monitoring interaktif bagi guru pengawas dalam satu sistem yang responsif dan ringan.
---
📑 Daftar Isi
Fitur Utama
Arsitektur & Struktur File
Struktur Database (Google Sheets)
Panduan Instalasi & Setup
Akun Default & Hak Akses
Alur Penggunaan Aplikasi
Teknologi yang Digunakan
Keamanan & Batasan Sistem
---
✨ Fitur Utama
👨‍🎓 Portal Siswa
Login Cepat & Autocomplete Instan: Siswa cukup memilih kelas, lalu mengetikkan nama tanpa perlu mengingat kata sandi rumit (data siswa di-cache di browser untuk pencarian instan).
Ringkasan Status Tugas: Kartu indikator metrik tugas (Total Tugas, Belum Dikumpulkan, Selesai, dan Progress Bar persentase).
Detail & Petunjuk Tugas: Menampilkan nama project, batas waktu (deadline), format file yang diizinkan, dan ukuran maksimum.
Upload File Tugas Praktis: Drag-and-drop file atau unggah langsung ke Google Drive sistem (otomatis mengubah kepemilikan dan hak akses berkas).
Proteksi File Video: Mencegah siswa mengunggah format video berat guna menghemat kuota Google Drive.
Riwayat & Preview Pengumpulan: Melihat bukti pengumpulan, mengunduh file yang telah dikirim, atau menghapus/mengganti file (jika diizinkan sistem).
👨‍🏫 Panel Pengawas Guru
Autentikasi Aman: Login menggunakan username dan password terenkripsi (SHA-256) serta proteksi brute-force rate-limiting (penguncian akun sementara jika salah 5 kali).
Dashboard Analitik & Donut Chart: Visualisasi persentase penyelesaian tugas keseluruhan dan grafik progres pengumpulan per tingkatan kelas.
Tabel Hasil Project Siswa: Monitoring real-time pengumpulan tugas dengan filter pencarian instan berdasarkan nama, kelas, dan status (Submitted / Not Submitted).
Manajemen Data Siswa (CRUD Lengkap):
Tambah data siswa mandiri beserta upload avatar/foto.
Edit profil dan kelas siswa.
Hapus satu per satu, hapus massal (Bulk Delete), atau Delete All dengan konfirmasi ketik pengaman.
Import & Export Template Excel:
Fitur unduh template data siswa berbasis XML Excel/CSV.
Fitur preview dan validasi instan data siswa sebelum diimpor ke spreadsheet.
Pengaturan Sistem & Profil:
Pengaturan judul tema ujian/tugas, instruksi tugas, serta periode asesmen (UTS, UAS, Daily Assessment).
Pembaruan profil pengajar (nama lengkap, foto guru, dan daftar kelas yang diampu).
---
📁 Arsitektur & Struktur File
Proyek ini menggunakan arsitektur bawaan Google Apps Script HTML Service:
```text
├── Code.gs             # Backend Apps Script (API, Controller, DB Handlers, Auth & Security)
├── Index.html          # Frontend SPA (HTML5, Tailwind CSS, JavaScript UI Controller)
└── README.md           # Dokumentasi teknis dan panduan penggunaan proyek
```
Penjelasan File:
`Code.gs`:
Menangani routing `doGet(e)` dengan mode `ALLOWALL` (dapat di-embed via `iframe`).
Inisialisasi otomatis lembar kerja Spreadsheet dan folder Google Drive (`setupDatabase()`).
Manajemen sesi berbasis `CacheService` dengan masa kedaluwarsa 8 jam.
Enkripsi kata sandi `SHA-256`, sanitasi input, dan pencatatan riwayat (`AuditLog`).
`Index.html`:
Antarmuka Single Page Application (SPA) modern berbasis Tailwind CSS CDN dan font Plus Jakarta Sans.
Menggunakan `google.script.run` yang dibungkus dalam Promise JavaScript untuk interaksi asinkron tanpa reload halaman.
---
🗄️ Struktur Database (Google Sheets)
Fungsi `setupDatabase()` pada `Code.gs` akan secara otomatis membuat dan menata lembar kerja (sheet) berikut:
Nama Sheet	Kolom Header	Fungsi & Deskripsi
`Users`	`user_id`, `username`, `password_hash`, `fullname`, `role`, `photo`, `teaching_classes`, `created_at`	Menyimpan kredensial akun guru pengawas/admin.
`Students`	`student_id`, `name`, `grade`, `photo`, `created_at`	Menyimpan daftar siswa, nomor induk/ID, dan foto profil.
`Tasks`	`task_id`, `task_name`, `description`, `deadline`, `deadline_label`, `allowed_file_types`, `max_file_size`, `is_active`	Menyimpan konfigurasi tugas ujian aktif.
`Submissions`	`submission_id`, `student_id`, `student_name`, `grade`, `task_id`, `task_name`, `file_name`, `file_url`, `file_id`, `file_type`, `file_size`, `uploaded_at`, `status`	Menyimpan log berkas tugas siswa yang telah diunggah.
`Settings`	`key`, `value`	Pengaturan fleksibel aplikasi (izin replace file, periode project, dll).
`AuditLog`	`timestamp`, `user`, `action`, `details`	Catatan aktivitas penting sistem (login, upload, update, hapus).
Folder Google Drive yang Dibuat Otomatis:
`UJIAN_DIGITAL_UPLOADS`: Menyimpan file tugas yang diunggah siswa.
`UJIAN_DIGITAL_STUDENT_PHOTOS`: Menyimpan foto profil siswa dan foto guru.
---
🚀 Panduan Instalasi & Setup
Buka Google Spreadsheet Baru:
Buka Google Sheets di browser Anda.
Beri nama spreadsheet, misalnya: `Database Ujian Komputer`.
Buka Apps Script Editor:
Pada menu atas spreadsheet, klik menu Ekstensi > Apps Script.
Salin Kode Program:
Tempelkan kode backend ke file `Code.gs`.
Klik tanda tambah (+) di sebelah kiri editor > pilih HTML > beri nama file `Index` (sehingga menjadi `Index.html`).
Tempelkan seluruh isi file `Index.html`.
Inisialisasi Database:
Pada toolbar atas Script Editor, pilih fungsi `setupDatabase`.
Klik tombol Jalankan (Run).
Berikan izin otorisasi (Review Permissions) yang diminta oleh Google.
Tunggu log eksekusi selesai. Lembar kerja dan folder Drive otomatis terbuat beserta data sampel.
Deploy sebagai Web App:
Klik tombol Terapkan (Deploy) di pojok kanan atas > pilih Penerapan Baru (New Deployment).
Pilih jenis: Aplikasi Web (Web App).
Atur konfigurasi:
Deskripsi: `Rilis Versi 1.0`
Jalankan sebagai (Execute as): `Saya (email Anda)`
Yang memiliki akses (Who has access): `Siapa saja (Anyone)`
Klik Terapkan (Deploy) dan salin URL Web App yang dihasilkan.
---
🔐 Akun Default & Hak Akses
Setelah menjalankan `setupDatabase()`, sistem menyiapkan akun default guru:
Role: Guru (Admin Pengawas)
Username: `admin`
Password Default: `Admin@2026`
Catatan Keamanan: Disarankan untuk segera mengubah nama dan memperbarui data profil setelah login pertama kali.
---
🔄 Alur Penggunaan Aplikasi
```text
[Siswa Masuk]
   │──> Pilih Kelas
   │──> Cari Nama (Autocomplete)
   │──> Klik 'Masuk'
   └──> Halaman Tugas ──> Upload File (Non-Video, Maks 10MB) ──> Selesai

[Guru Masuk]
   │──> Masukkan Username & Password
   │──> Panel Dashboard (Lihat Statistik Pengumpulan)
   │──> Panel Hasil Project (Periksa & Unduh Karya Siswa)
   │──> Kelola Data Siswa (Input / Impor Excel)
   └──> Pengaturan (Ganti Judul & Periode Tugas)
```
---
🛠️ Teknologi yang Digunakan
Backend: Google Apps Script (GAS)
Database & Penyimpanan: Google Sheets & Google Drive API
Caching & Sesi: `CacheService` & `LockService` (menangani konkurensi upload)
Styling & UI: Tailwind CSS CDN
Tipografi: Plus Jakarta Sans by Google Fonts
---
🛡️ Keamanan & Batasan Sistem
Ukuran File Maksimum: Dibatasi maksimal 10 MB per berkas tugas untuk menjaga performa transfer data base64 pada Google Apps Script.
Filter Ekstensi: Sistem memblokir unggahan ekstensi video (`.mp4`, `.mkv`, `.avi`, `.mov`, `.webm`, dll.).
Konkurensi Database: Menggunakan `LockService.getScriptLock()` pada fungsi penulisan penting untuk mencegah konflik data baris Spreadsheet saat banyak siswa mengumpulkan tugas bersamaan.
Validasi Sesi: Token sesi bersifat stateless sementara di memori cache Google dengan durasi aktif maksimum 8 jam.
---
👨‍💻 Kontributor & Lisensi
Author / Developer: Farhan Alfaizi (`@farhanalfaizi`)
Lisensi: Bebas digunakan dan dimodifikasi untuk keperluan institusi pendidikan dan pembelajaran.
