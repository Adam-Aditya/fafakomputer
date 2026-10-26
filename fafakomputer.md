# DOKUMEN PERANCANGAN SISTEM & ANALISIS PRE-PRODUCTION
**Proyek:** Aplikasi Web Perawatan, Diagnostik & Sparepart Laptop (**fafakomputer.**)  
**Tech Stack:** Laravel Framework & PostgreSQL  
**Gaya Desain UI/UX:** Clean Tech SaaS & Bento Grid Layout (Berdasarkan Mockup landing.png)

---

## 1. TAHAP PRE-PRODUCTION & DOKUMEN WAJIB

Sebelum masuk ke tahap penulisan kode (*coding*), tahapan *pre-production* ini memastikan aplikasi tidak melebar (*scope creep*) dan setiap komponen pada desain **fafakomputer.** memiliki fungsi bisnis yang jelas.

### A. Tahapan Pre-Production
1. **Requirement Gathering & Business Flow:** Memetakan alur penerimaan unit laptop (melalui *Kurir Antar-Jemput Safe-Box* atau *Kirim Mandiri ke Workshop*), pencatatan kondisi awal, pengerjaan oleh teknisi, hingga klaim stempel loyalti `10x = 1 FREE`.
2. **Feature Consolidation (Penyederhanaan ke 2 Fitur Utama):** Mengelompokkan elemen-elemen pada navigasi (`Katalog Layanan`, `Diagnostik`, `Loyalty 10x`, `Tracking`, dan `Booking Servis`) ke dalam 2 sistem inti agar pengguna tidak bingung.
3. **Database Modeling (PostgreSQL):** Merancang skema tabel yang mengizinkan pelanggan memesan tanpa login (*Guest*) sekaligus mendukung sistem pengumpulan *stamp* loyalti bagi pelanggan yang login (*Member*) atau terhubung via nomor WhatsApp.
4. **Design System & Component Breakdown:** Memecah desain gambar *fafakomputer.* menjadi komponen Blade/Livewire atau Inertia di Laravel.

### B. Dokumen yang Harus Disiapkan Sebelum Production
1. **PRD (Product Requirements Document):** Dokumen spesifikasi fitur, aturan main program loyalti `10x Servis Gratis 1x Perawatan Deep Clean`, serta cakupan wilayah kurir penjemputan.
2. **Flowchart / BPMN:** Diagram alur transaksi dari sisi Pelanggan (*Guest* vs *Member*) dan dari sisi Admin/Teknisi.
3. **ERD (Entity Relationship Diagram) & Kamus Data:** Struktur tabel PostgreSQL, tipe data, indeks pencarian tiket, dan relasi antar tabel.
4. **SOP Digital & Form Checklist Kondisi Unit (Wajib untuk Servis Laptop):** Karena mengandalkan layanan *Antar-Jemput Safe-Box Anti-Statis*, wajib ada berita acara digital (foto fisik laptop, status layar, kelengkapan *charger*) saat serah terima dengan kurir agar bebas sengketa.

---

## 2. KONSOLIDASI FITUR: DARI 4 MENJADI 2 FITUR UTAMA

Berdasarkan kebutuhan awal Anda (Perawatan Cleaning/Repasta, Katalog Sparepart, Konsultasi, dan Booking Antar-Jemput/Kirim) serta mengacu pada antarmuka **fafakomputer.**, seluruh layanan tersebut disatukan menjadi **2 Pilar Fitur Utama**:

### Fitur Utama 1: Katalog Solusi & Diagnostik Cerdas (*Unified Catalog & Smart Consultation*)
Menggabungkan **Layanan Perawatan (*Deep Clean & Repasta*)**, **Katalog Harga Sparepart**, dan **Konsultasi Diagnostik** ke dalam satu alur eksplorasi.
* **Implementasi dari Desain:**
  * Menampilkan kartu unggulan seperti **Deep Clean & Repasta Master** (*Thermal Grizzly & Honeywell PTM7950* dengan klaim penurunan suhu hingga $-18^\circ\text{C}$) serta kartu katalog **Penggantian Baterai Original** dan **Upgrade NVMe SSD & RAM DDR5**.
  * **Integrasi Konsultasi (Tanpa Fitur Chat Rumit di Web):** Tombol `Cek Diagnostik & Konsultasi` di bagian *Hero* maupun tombol pada kartu *sparepart* akan membuka *modal interaktif* singkat (Pilih Seri Laptop & Keluhan). Setelah diisi, pengguna diberi 2 opsi langsung: **"Konsultasi via WhatsApp Teknisi"** (otomatis mengirim teks keluhan yang sudah terformat) atau **"Lanjut Booking Servis"**.

### Fitur Utama 2: Sistem Booking Terpadu, Tracking & Loyalty (*Smart Booking & 10x Stamp Engine*)
Menggabungkan **Booking Jadwal (Antar-Jemput / Kirim Barang)**, **Tracking Status Real-time**, dan **Pencatatan Poin 10x Gratis 1x**.
* **Implementasi dari Desain:**
  * Tombol `Booking Servis ↗` di pojok kanan atas mengarahkan ke satu formulir pemesanan ringkas.
  * **Pilihan Metode Penyerahan Unit:**
    1. **Mode Antar-Jemput (*On-Site Pickup*):** Kurir khusus membawa *safe-box* anti-statis ke lokasi pelanggan.
    2. **Mode Kirim ke Workshop (*Drop-off / Ekspedisi*):** Pelanggan mengirim unit atau datang langsung ke workshop sesuai jadwal yang dipilih.
  * **Sistem Guest vs Member (Loyalty 10x = 1 FREE):**
    * **Tanpa Login (*Guest*):** Bebas melakukan *booking* dan bisa memantau progres perbaikan lewat menu `Tracking` menggunakan Nomor Tiket + Nomor WA. Namun, transaksi tidak menambah jumlah cap digital (*stamp*).
    * **Dengan Login (*Member*):** Setiap transaksi yang berstatus `Completed` (Selesai & Dibayar) otomatis menambah 1 titik pada kartu **10x = 1 FREE Official Club**. Ketika mencapai $10/10$, sistem otomatis menerbitkan *Voucher Gratis 1x Deep Clean* yang bisa dipakai saat *booking* berikutnya.

---

## 3. BEDAH UI/UX BERDASARKAN DESAIN "FAFAKOMPUTER."

Desain yang Anda lampirkan mengusung gaya **Modern Tech Minimalist & Bento Grid**. Berikut adalah panduan implementasi antarmukanya:

### A. Palet Warna & Tipografi
* **Background Utama:** Putih bersih (`#FFFFFF`) dan abu-abu sangat muda (`#F8FAFC`) untuk area latar belakang kartu luar agar terlihat bersih seperti standar laboratorium.
* **Surface Gelap (Hero, Kartu Kiri, & CTA Bawah):** *Deep Navy / Midnight Blue* (`#070D19` dengan gradasi *radial glow* biru tua `#0F2942`).
* **Aksen Utama (Kartu Tengah & Badge):** *Electric Royal Blue* (`#1E40AF` hingga `#2563EB`) untuk menonjolkan layanan *flagship* (*Deep Clean & Repasta*).
* **Indikator Status:** Hijau Emerald (`#10B981`) untuk penanda *stamp* penuh ($10/10$) dan Biru Cerah (`#3B82F6`) untuk indikator aktif (`Mode Antar-Jemput: Aktif`).
* **Tipografi:** Menggunakan *font sans-serif* geometris seperti **Plus Jakarta Sans** atau **Inter** dengan ketebalan *Bold/ExtraBold* pada judul utama dan kerapatan huruf (*letter-spacing*) sedikit rapat (`tracking-tight`).

### B. Struktur Komponen Halaman (Top-to-Bottom)
1. **Floating Pill Navbar:**
   * Kiri: Logo teks `fafakomputer.` dengan titik biru di akhir.
   * Tengah: Menu kapsul (*pill container*) abu-abu terang berisi tautan `Katalog Layanan`, `Diagnostik`, `Loyalty 10x`, dan `Tracking`.
   * Kanan: Tombol *solid* gelap `Booking Servis ↗` (dan tombol kecil `Masuk / Member` di sebelahnya).
2. **Rounded Dark Hero Container:**
   * Kontainer besar bersudut membulat (`rounded-3xl`) dengan *headline* terpusat: *"Performa Laptop Maksimal, Aman & Transparan."*
   * Tombol tengah berbingkai tipis (*glassmorphism*): `Cek Diagnostik & Konsultasi`.
   * Bagian bawah Hero: Kiri menampilkan *badge* `FK | LAB | PRO - Ditangani Teknisi Bersertifikat (Peralatan ESD-Safe & Lab Standar)`, kanan menampilkan tautan cepat sosial media (`Instagram ↗`, `WhatsApp ↗`, `TikTok ↗`).
3. **Editorial Statement Section:**
   * Terdiri dari *pill badge* `Tentang fafakomputer` di sebelah kiri dan paragraf pernyataan misi berukuran besar di sebelah kanan, dengan penekanan teks bergaris bawah warna biru pada kalimat **`10x Servis Gratis 1x Perawatan Deep Clean`**.
4. **3-Column Bento Grid (Nilai Jual Utama):**
   * **Kartu 1 (Gelap - Logistik):** Menjelaskan *Layanan Antar-Jemput & On-Site Pickup* dengan *safe-box* anti-statis dan *badge* indikator biru `Mode Antar-Jemput: Aktif`.
   * **Kartu 2 (Biru Elektrik - Layanan Inti):** Menampilkan metrik besar `-18°C Penurunan Suhu Maksimal` dengan spesifikasi *Thermal Grizzly & Honeywell PTM7950* serta garansi suhu stabil 6 bulan.
   * **Kartu 3 (Putih Bersih - Gamifikasi Loyalti):** Menampilkan visualisasi **10x = 1 FREE Official Club** lengkap dengan bar titik-titik (*dot progress*) seperti `5/10`, `8/10`, hingga `10/10` (hijau) yang memotivasi pengunjung untuk membuat akun.
5. **Social Proof / Metrics Bar:**
   * Deretan 4 angka statistik: `1,500+ Laptop Berhasil Ditangani`, `99.2% Tingkat Kepuasan Pelanggan`, `30 Hari Garansi Servis & Part Penuh`, dan `10x = Free Loyalty Stamp Reward`.
6. **Split Section Katalog Layanan & Sparepart:**
   * Kiri: Judul *Solusi Lengkap & Katalog Sparepart Berkualitas* beserta tombol `Jelajahi Solusi ↗` (membuka halaman katalog penuh dengan filter).
   * Kanan: 2 Kartu *Preview* produk utama (`Penggantian Baterai Original` dan `Upgrade NVMe SSD & RAM DDR5`).
7. **Bottom Dark CTA Banner & Minimal Footer:**
   * Banner gelap membulat *"Siap Maksimalkan Laptop Anda Hari Ini?"* dengan tombol putih kontras `Chat Teknisi Sekarang ↗`.

---

## 4. ANALISIS KELEBIHAN, KEKURANGAN & REKOMENDASI PERUBAHAN

### A. Kelebihan Konsep & Desain Ini
1. **Visual Sangat Meyakinkan (*High Trust*):** Banyak pengguna takut menyerahkan laptop karena khawatir komponen ditukar atau lecet. Penyebutan eksplisit seperti *Peralatan ESD-Safe*, *Safe-Box Anti-Statis*, dan *Honeywell PTM7950* langsung membangun kredibilitas tingkat tinggi.
2. **Tanpa Hambatan Transaksi (*Zero Friction*):** Pengunjung yang sedang panik karena laptopnya *overheat* atau mati bisa langsung klik *Booking* atau *Chat Teknisi* tanpa dipaksa mendaftar akun terlebih dahulu.
3. **Visualisasi Loyalti yang Jelas:** Kartu ke-3 pada *Bento Grid* langsung memperlihatkan cara kerja *stamp* `10x = 1 FREE` secara visual (titik-titik progres), sehingga pelanggan langsung paham keuntungan menjadi *member*.

### B. Kekurangan / Potensi Masalah Bisnis
1. **Siklus Servis Laptop Lebih Lama dari Cuci Sepatu:**
   * *Masalah:* Orang melakukan *deep clean* atau ganti *sparepart* laptop rata-rata 6 bulan sekali. Jika syarat gratis adalah 10x servis untuk 1 laptop pribadi, pelanggan butuh waktu **5 tahun** untuk mencapai 10 stempel. Hal ini bisa membuat fitur *login/member* kurang diminati oleh pengguna individu.
2. **Sinkronisasi Nomor WA (Tamu vs Member):**
   * *Masalah:* Pada kartu ke-3 tertulis *"Otomatis terhubung nomor WA"*. Jika pelanggan 3 kali servis tanpa login menggunakan nomor WA yang sama, lalu di servis ke-4 ia baru membuat akun, ia pasti menuntut agar 3 servis sebelumnya tetap dihitung.
3. **Risiko Biaya Kurir Antar-Jemput:**
   * *Masalah:* Jika tidak ada pembatasan jarak atau minimal transaksi di sistem *booking*, biaya operasional kurir penjemputan bisa menggerus margin keuntungan servis ringan.

### C. Rekomendasi Perubahan (Solusi Strategis)
1. **Ubah Aturan Akumulasi "10x = 1 FREE" Menjadi Berbasis Akun (Multi-Device) & Bobot Transaksi:**
   * Izinkan 1 akun *member* mengumpulkan *stamp* dari **banyak laptop** (laptop pribadi, teman kos, keluarga, atau inventaris kantor). Ini sangat cocok dengan ilustrasi *tier* pada gambar Anda (`Basic Tier`, `Pro Gamer`, `Workstation`).
   * **Opsi Tambahan:** Berikan **2 Stamp sekaligus** jika pelanggan mengambil paket kombo (*Deep Clean + Upgrade SSD/Baterai*), sehingga target 10 *stamp* terasa jauh lebih cepat tercapai.
2. **Fitur *Auto-Claim History by WhatsApp Number* (Sangat Direkomendasikan):**
   * Di sistem database, kita tetap mengunci aturan bahwa **Hanya Member (Login) yang bisa memakai klaim gratis**. Namun, untuk memikat *Guest* agar mau daftar akun, ketika *Guest* selesai servis, tampilkan pesan di halaman *Tracking*: *"Servis selesai! Daftar akun dengan nomor WA ini dalam 7 hari untuk mengamankan +1 Stamp Loyalty Anda."*

---

## 5. PERANCANGAN DATABASE (POSTGRESQL SCHEMA)

Berikut adalah rancangan tabel PostgreSQL yang mendukung tampilan **fafakomputer.**, sistem *Guest & Member*, katalog layanan/sparepart, serta *tracking* dan *stamp* loyalti:

### 1. Tabel `users` (Untuk Member & Admin)
Menyimpan data akun bagi pelanggan yang memilih login untuk mengumpulkan *stamp* `10x = 1 FREE`.
* `id` (BIGSERIAL, PK)
* `name` (VARCHAR)
* `email` (VARCHAR, UNIQUE)
* `phone_wa` (VARCHAR, UNIQUE) — *Kunci utama pencocokan nomor WA*
* `password` (VARCHAR)
* `role` (VARCHAR) — `customer`, `admin`, `technician`, `courier`
* `loyalty_stamps` (SMALLINT, DEFAULT 0) — *Bernilai $0$ sampai $10$*
* `free_service_vouchers` (SMALLINT, DEFAULT 0) — *Jumlah kupon gratis Deep Clean yang siap dipakai*
* `member_tier` (VARCHAR, DEFAULT 'Basic Tier') — `Basic Tier`, `Pro Gamer`, `Workstation`
* `timestamps`

### 2. Tabel `catalog_items` (Layanan Treatment & Sparepart)
Menyimpan data kartu layanan (seperti *Deep Clean PTM7950*) dan *sparepart* (seperti *Baterai Original*, *NVMe SSD*).
* `id` (BIGSERIAL, PK)
* `category` (VARCHAR) — `treatment` (Cleaning/Repasta) atau `sparepart` (Baterai, RAM, SSD, LCD)
* `name` (VARCHAR) — Contoh: *Deep Clean & Repasta Master (PTM7950)*
* `slug` (VARCHAR, UNIQUE)
* `badge_text` (VARCHAR, NULLABLE) — Contoh: *Spare Part Asli*, *Performa Ekstrem*
* `highlight_metric` (VARCHAR, NULLABLE) — Contoh: *-18°C*, *UP TO 7400 MB/S*, *ORIGINAL CELL 100%*
* `description` (TEXT)
* `price_min` (NUMERIC(12,2)) — Harga pasti atau harga mulai dari
* `price_max` (NUMERIC(12,2), NULLABLE) — Jika harga bervariasi tergantung seri laptop
* `warranty_days` (INTEGER, DEFAULT 30) — Contoh: 30 Hari / 180 Hari
* `stamp_reward` (SMALLINT, DEFAULT 1) — Jumlah *stamp* yang didapat jika layanan ini selesai
* `is_active` (BOOLEAN, DEFAULT TRUE)
* `timestamps`

### 3. Tabel `bookings` (Transaksi Utama & Tracking Servis)
Mendukung transaksi **Guest** (`user_id` bernilai `NULL`) maupun **Member** (`user_id` terisi).
* `id` (BIGSERIAL, PK)
* `ticket_code` (VARCHAR, UNIQUE, INDEX) — Contoh: `FK-202610-089` (Untuk fitur menu *Tracking*)
* `user_id` (BIGINT, FK `users.id`, NULLABLE) — *NULL jika pelanggan tidak login*
* `customer_name` (VARCHAR)
* `customer_wa` (VARCHAR, INDEX)
* `laptop_brand_series` (VARCHAR) — Contoh: *ASUS ROG Zephyrus G14 / ThinkPad T14*
* `issue_complaint` (TEXT) — Catatan keluhan / hasil form diagnostik
* `delivery_method` (VARCHAR) — `pickup_courier` (Antar-Jemput Safe-Box) atau `drop_off_delivery` (Kirim ke Workshop)
* `pickup_address` (TEXT, NULLABLE) — Wajib diisi jika memilih `pickup_courier`
* `scheduled_date` (DATE)
* `scheduled_time_slot` (VARCHAR) — Contoh: `09:00 - 12:00`, `13:00 - 16:00`
* `status` (VARCHAR) — `pending`, `picked_up`, `diagnosing`, `in_progress`, `ready_to_return`, `completed`, `cancelled`
* `initial_condition_notes` (JSONB, NULLABLE) — *Checklist fisik laptop & suhu awal sebelum dibongkar*
* `final_temp_drop` (VARCHAR, NULLABLE) — Catatan hasil penurunan suhu (misal: `89°C -> 72°C`) untuk transparansi
* `is_using_free_voucher` (BOOLEAN, DEFAULT FALSE) — Bernilai `TRUE` jika memakai klaim gratis 10x
* `total_price` (NUMERIC(12,2), DEFAULT 0)
* `timestamps`

### 4. Tabel `booking_items` (Detail Layanan/Part yang Dikerjakan)
* `id` (BIGSERIAL, PK)
* `booking_id` (BIGINT, FK `bookings.id` ON DELETE CASCADE)
* `catalog_item_id` (BIGINT, FK `catalog_items.id`)
* `item_name_snapshot` (VARCHAR)
* `agreed_price` (NUMERIC(12,2))
* `warranty_until` (DATE, NULLABLE)
* `timestamps`

### 5. Tabel `loyalty_ledgers` (Riwayat Mutasi Stamp 10x = 1 FREE)
Mencatat setiap penambahan *stamp* atau penukaran kupon gratis agar transparan dan tidak bisa dimanipulasi.
* `id` (BIGSERIAL, PK)
* `user_id` (BIGINT, FK `users.id` ON DELETE CASCADE)
* `booking_id` (BIGINT, FK `bookings.id`, NULLABLE)
* `type` (VARCHAR) — `earn` (+1 stamp saat servis selesai) atau `redeem` (-10 stamp ditukar menjadi 1 voucher gratis)
* `stamps_delta` (SMALLINT) — `+1` atau `-10`
* `description` (VARCHAR)
* `created_at` (TIMESTAMP)

---

## 6. STRUKTUR ARSITEKTUR PROYEK LARAVEL

Berikut adalah susunan direktori dan komponen utama pada Laravel (menggunakan **Blade + Tailwind CSS + Alpine.js/Livewire**) agar sesuai dengan desain **fafakomputer.**:

```text
fafakomputer-web/
├── app/
│   ├── Enums/
│   │   ├── BookingStatus.php          # Enum: Pending, PickedUp, Diagnosing, InProgress, Completed
│   │   └── DeliveryMethod.php         # Enum: PickupCourier, DropOff
│   ├── Http/
│   │   ├── Controllers/
│   │   │   ├── HomeController.php     # Render Landing Page Bento Grid & Statistik
│   │   │   ├── CatalogController.php  # Filter Layanan & Integrasi Konsultasi WA
│   │   │   ├── BookingController.php  # Form Booking (Bisa diakses Guest & Member)
│   │   │   ├── TrackingController.php # Cek Progres Servis Real-time via Kode Tiket
│   │   │   ├── LoyaltyController.php  # Halaman Dashboard Member (Progress 10x = 1 Free)
│   │   │   └── Admin/                 # Panel Teknisi & Admin (Update status & checklist suhu)
│   ├── Models/
│   │   ├── User.php
│   │   ├── CatalogItem.php
│   │   ├── Booking.php
│   │   ├── BookingItem.php
│   │   └── LoyaltyLedger.php
│   └── Services/
│       ├── LoyaltyStampService.php    # Logika otomatis tambah stamp saat status = completed
│       └── WhatsAppLinkService.php    # Generator URL pesan WA otomatis untuk konsultasi
├── database/
│   ├── migrations/                    # File migrasi PostgreSQL (5 tabel di atas)
│   └── seeders/
│       └── CatalogSeeder.php          # Data awal: Deep Clean PTM7950, Baterai Original, NVMe DDR5
└── resources/
    └── views/
        ├── layouts/
        │   └── app.blade.php          # Layout utama dengan Floating Pill Navbar & Footer
        ├── components/
        │   ├── navbar.blade.php       # Navbar fafakomputer.
        │   ├── hero-card.blade.php    # Hero Dark Navy dengan badge ESD-Safe
        │   ├── bento-grid.blade.php   # 3 Kartu Utama (Kurir, -18°C PTM7950, Loyalty 10x)
        │   ├── catalog-split.blade.php# Section Katalog & Sparepart
        │   └── diagnostic-modal.blade.php # Modal interaktif Cek Diagnostik & Konsultasi
        └── pages/
            ├── landing.blade.php
            ├── booking-form.blade.php
            ├── tracking-result.blade.php
            └── member-dashboard.blade.php
```

### Logika Inti: `LoyaltyStampService.php` (Aturan Guest vs Member)
Agar aturan bisnis Anda berjalan otomatis di belakang layar ketika teknisi menyelesaikan servis:
1. Saat Admin/Teknisi mengubah status *Booking* menjadi `completed`:
2. Sistem mengecek apakah `booking->user_id` terisi (artinya pelanggan melakukan **Login / Member**).
   * **Jika `user_id == NULL` (Guest):** Sistem hanya menyelesaikan tiket dan mengaktifkan masa garansi, **tidak ada *stamp* yang ditambahkan**.
   * **Jika `user_id != NULL` (Member):**
     * Apabila pada transaksi tersebut pelanggan tidak sedang memakai kupon gratis (`is_using_free_voucher == false`), sistem menambahkan `+1` pada `user->loyalty_stamps` dan mencatatnya di tabel `loyalty_ledgers`.
     * Jika setelah ditambah nilai `user->loyalty_stamps` mencapai `10`, sistem otomatis mereset `loyalty_stamps` menjadi `0`, menambahkan `+1` pada kolom `free_service_vouchers`, dan menaikkan level *tier* pelanggan (`Basic Tier` $\rightarrow$ `Pro Gamer` $\rightarrow$ `Workstation`).