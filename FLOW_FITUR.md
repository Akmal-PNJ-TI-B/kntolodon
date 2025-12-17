# FLOW_FITUR.md

## High-Level Overview Alur Fitur BookEZ (PBL-Perpustakaan)

---

### 1. Login & Register (Autentikasi)
- **View:**
  - `view/login.php`, `view/register.php`
- **Controller:**
  - `LoginController.php`, `RegisterController.php`
- **Model:**
  - `AkunModel.php`
- **JS:**
  - `assets/js/auth.js`, `assets/js/regis.js`, `assets/js/reset-password.js`, `assets/js/captcha.js`
- **CSS:**
  - `main.css`, `profile.css`
- **Alur:**
  1. User akses login/register → form tampil (View)
  2. Submit → Controller validasi input, captcha, email domain
  3. Model cek database (akun, status, password hash)
  4. Jika sukses → set session, redirect dashboard
  5. Jika gagal → alert error, reload
  6. Register: validasi email PNJ, upload validasi_mahasiswa (mahasiswa), status awal "Tidak Aktif"
  7. Admin aktivasi akun via Member List

---

### 2. Dashboard (User & Admin)
- **View:**
  - `view/dashboard.php`, `view/admin/dashboard.php`
- **Controller:**
  - `DashboardController.php`, `AdminController.php`
- **Model:**
  - `BookingModel.php`, `RuanganModel.php`, `BookingListModel.php`
- **JS:**
  - `assets/js/dashboard.js`, `assets/js/admin.js`
- **CSS:**
  - `dashboard.css`, `admin-dashboard.css`
- **Alur:**
  1. User login → redirect ke dashboard sesuai role
  2. Dashboard tampilkan ringkasan booking, status ruangan, shortcut fitur
  3. Admin dashboard: grid fitur admin, akses ke kelola ruangan, laporan, dsb

---

### 3. Booking Ruangan (User)
- **View:**
  - `view/booking/index.php`, `view/booking/kode_booking.php`, `view/booking/hapus_booking.php`, `view/booking/reskedul_booking.php`
- **Controller:**
  - `BookingController.php`
- **Model:**
  - `BookingModel.php`, `ScheduleModel.php`, `RuanganModel.php`, `PengaturanModel.php`
- **JS:**
  - `assets/js/booking.js`
- **CSS:**
  - `booking.css`
- **Alur:**
  1. User buka form booking → pilih ruangan, tanggal, jam, anggota
  2. JS fetch jadwal terbooking (AJAX) → tampilkan timeline
  3. Submit → Controller validasi: waktu, kapasitas, block/suspend, hari libur, jam operasi
  4. Model insert booking, anggota_booking
  5. Redirect ke kode_booking (konfirmasi)
  6. Bisa hapus/reschedule booking (validasi status & waktu)

---

### 4. Profile Management
- **View:**
  - `view/profile/index.php`
- **Controller:**
  - `ProfileController.php`
- **Model:**
  - `AkunModel.php`
- **JS:**
  - `assets/js/profile.js`
- **CSS:**
  - `profile.css`
- **Alur:**
  1. User akses profile → tampil data diri, upload foto profil
  2. Edit data (terbatas), ganti password (modal)
  3. Admin: akses dashboard admin dari sini

---

### 5. Admin Panel
#### a. Kelola Ruangan
- **View:**
  - `view/admin/kelola_ruangan.php`
- **Controller:**
  - `AdminController.php`
- **Model:**
  - `RuanganModel.php`
- **JS:**
  - `assets/js/kelola-ruangan.js`
- **CSS:**
  - `kelola-ruangan.css`
- **Alur:**
  1. Admin akses kelola ruangan → list ruangan
  2. Tambah/edit/hapus ruangan (modal)
  3. Validasi kapasitas, status ruangan

#### b. Booking List (Admin)
- **View:**
  - `view/admin/booking_list.php`
- **Controller:**
  - `AdminController.php`
- **Model:**
  - `BookingListModel.php`, `RuanganModel.php`
- **JS:**
  - `assets/js/booking-list.js`
- **CSS:**
  - `booking.css`
- **Alur:**
  1. Admin lihat semua booking (filter, pagination)
  2. Modal check-in anggota, update status booking
  3. Auto-update status (SELESAI, HANGUS)

#### c. Member List (User/Admin Management)
- **View:**
  - `view/admin/member_list.php`
- **Controller:**
  - `AdminController.php`
- **Model:**
  - `MemberModel.php`, `AkunModel.php`
- **JS:**
  - `assets/js/member-list.js`
- **CSS:**
  - `member-list.css`
- **Alur:**
  1. Admin kelola user/admin (tab, pagination)
  2. Tambah/edit/hapus/aktivasi akun (modal)
  3. Validasi email, password, upload validasi_mahasiswa

#### d. Laporan
- **View:**
  - `view/admin/laporan.php`
- **Controller:**
  - `AdminController.php`
- **Model:**
  - `LaporanModel.php`, `FeedbackModel.php`, `RuanganModel.php`, `BookingListModel.php`
- **JS:**
  - `assets/js/laporan.js`
- **CSS:**
  - `laporan.css`
- **Alur:**
  1. Admin filter laporan (periode, ruangan, status)
  2. Statistik booking, feedback, export laporan

#### e. Pengaturan Sistem (Super Admin)
- **View:**
  - `view/admin/pengaturan.php`
- **Controller:**
  - `AdminController.php`
- **Model:**
  - `PengaturanModel.php`
- **JS:**
  - `assets/js/pengaturan.js`
- **CSS:**
  - `pengaturan.css`
- **Alur:**
  1. Super Admin atur jam operasional, hari libur (tab)
  2. Tambah/edit/hapus hari libur, update jam buka/tutup
  3. Validasi waktu, update DB

#### f. Booking Eksternal (Super Admin)
- **View:**
  - `view/admin/booking_external.php`
- **Controller:**
  - `AdminController.php`
- **Model:**
  - `BookingExternalModel.php`, `RuanganModel.php`, `PengaturanModel.php`
- **JS:**
  - `assets/js/admin.js`
- **CSS:**
  - `booking-external.css`
- **Alur:**
  1. Super Admin input booking eksternal (instansi, surat lampiran)
  2. Validasi waktu, upload file PDF, cek jam operasional/hari libur
  3. List booking eksternal (tab mendatang/histori, filter, pagination)

---

### 6. Feedback System
- **View:**
  - `view/feedback.php`
- **Controller:**
  - `BookingController.php`
- **Model:**
  - `FeedbackModel.php`
- **JS:**
  - `assets/js/feedback.js`
- **CSS:**
  - `main.css`
- **Alur:**
  1. Setelah booking selesai, user isi feedback (rating, kritik/saran)
  2. Data tersimpan, digunakan di laporan admin

---

### 7. Komponen & Utilitas
- **View:**
  - `view/components/`
    - `head.php` (head, asset helper)
    - `navbar_admin.php` (navbar admin)
    - `Footer.php` (footer)
    - `captcha.php` (captcha image)
    - `pagination.php` (pagination UI)
    - `modal_change_password.php`, `modal_reset_password.php`
- **JS:**
  - `assets/js/captcha.js`, `assets/js/main.js`
- **CSS:**
  - `main.css`
- **Alur:**
  - Komponen reusable, dipanggil di semua view sesuai kebutuhan

---

### 8. Error & Notifikasi
- **View:**
  - `view/error.php`
- **Controller:**
  - Semua controller (pattern: alert JS + redirect)
- **Alur:**
  - Jika error/akses tidak valid, tampilkan error page atau alert, redirect ke halaman sesuai role

---

### 9. Security & Validation
- **Session:**
  - Semua controller cek session & role
- **Validation:**
  - Email domain, password, kapasitas, waktu, file upload (MIME, size)
- **Role-based Access:**
  - User hanya booking, Admin hanya kelola, Super Admin fitur penuh

---

### 10. AJAX Endpoints
- **Controller:**
  - `BookingController.php`, `AdminController.php`
- **Model:**
  - Terkait fitur (Booking, Member, Ruangan)
- **JS:**
  - `booking.js`, `member-list.js`, `kelola-ruangan.js`, dsb
- **Alur:**
  - Fetch data dinamis (jadwal, user by NIM, dsb), return JSON, update UI tanpa reload

---

## Penutup
- Semua fitur mengikuti alur: **View → Controller → Model → JS → CSS**
- Validasi & security diimplementasi di semua layer
- Komponen & utilitas reusable untuk konsistensi UI/UX
- AJAX digunakan untuk interaksi dinamis, form tradisional untuk CRUD utama
- Role-based access control sangat ketat

---

> Dokumen ini merangkum alur utama seluruh fitur BookEZ, memetakan file terkait di setiap layer, dan menjelaskan flow data dari View hingga Model, serta integrasi JS & CSS.
