# literabook-ecommerce
Website e-commerce toko buku online modern dan responsif. Menghadirkan fitur pencarian cerdas, filter genre, pratinjau gratis 'Baca Bab 1', keranjang belanja real-time, alur checkout instan terintegrasi Stripe &amp; PayPal, serta panel manajemen inventaris stok otomatis untuk pengalaman belanja yang mulus dan terpercaya.
# Dokumentasi Sistem E-Commerce Toko Buku Online (LiteraBook)

## 1. Ikhtisar Proyek
LiteraBook adalah platform e-commerce toko buku daring yang dirancang untuk menghadirkan pengalaman berbelanja buku yang intuitif, cepat, dan terpercaya. Sistem ini mengintegrasikan katalog buku multi-genre, fitur pratinjau bacaan gratis, kalkulasi keranjang dinamis, simulasi pembayaran instan (Stripe & PayPal), serta pelacakan ketersediaan inventaris buku secara real-time.

---

## 2. Arsitektur & Spesifikasi Teknologi
* **Antarmuka Pengguna (Frontend):** HTML5 Semantik, Tailwind CSS (Responsive Utility-First), Google Web Fonts (*Plus Jakarta Sans* & *Playfair Display*).
* **State Management & Interaksi:** Vanilla JavaScript (ES6+) mandiri tanpa ketergantungan library berat, menjamin latensi rendering di bawah 0.5 detik.
* **Integrasi Pembayaran:** Stripe Checkout API (Tokenized Credit/Debit Card) & PayPal Sandbox Smart Payment Buttons.
* **Manajemen Data Inventaris:** Data master reaktif dengan sinkronisasi otomatis antara kuantitas stok di katalog dan pengurangan kuantitas pasca-transaksi (*two-way data binding*).

---

## 3. Panduan Penggunaan Pengguna (User Manual)
1. **Pencarian & Penjelajahan Buku:**
   * Pengguna dapat mencari buku dengan mengetik judul atau nama penulis pada bilah pencarian atas (hasil tersaring secara langsung).
   * Gunakan tombol pill kategori untuk memfilter buku berdasarkan genre (*Fiksi, Pengembangan Diri, Bisnis, Teknologi*).
2. **Fitur "Baca Bab 1" (Digital Preview):**
   * Klik tombol **"Baca Bab 1"** pada kartu buku untuk membuka pratinjau sampel bab pertama tanpa perlu login atau membayar.
3. **Keranjang Belanja:**
   * Klik tombol **"+ Keranjang"** untuk menambahkan buku.
   * Buka panel keranjang di pojok kanan atas untuk menambah atau mengurangi kuantitas pesanan.
4. **Proses Checkout & Pembayaran:**
   * Klik **"Lanjut ke Pembayaran"** pada panel keranjang.
   * Lengkapi data pengiriman (Nama, Email, WhatsApp, dan Alamat).
   * Pilih metode pembayaran:
     * **Stripe Card:** Masukkan nomor kartu kredit simulasi.
     * **PayPal:** Menggunakan alur verifikasi one-click checkout.
   * Klik **"Konfirmasi & Bayar"** untuk menyelesaikan pesanan dan menerima nomor resi digital.

---

## 4. Panduan Pengelolaan Inventaris (Admin Manual)
1. **Akses Panel Mode Inventaris:**
   * Klik tombol **"Mode Inventaris"** pada bagian header navigasi.
2. **Pemantauan Stok Real-Time:**
   * **Tersedia (Hijau):** Stok di atas 3 eksemplar.
   * **Menipis (Kuning/Amber):** Sisa stok 1–3 eksemplar (*Low Stock Alert*).
   * **Habis (Merah):** Stok bernilai 0. Tombol pembelian pada katalog publik otomatis dinonaktifkan untuk mencegah *overselling*.
3. **Pembaruan Stok (Restock):**
   * Admin dapat menekan tombol **"+1"**, **"-1"**, atau **"+5 Restock"** untuk menyesuaikan jumlah barang fisik yang tersedia di gudang.

---

## 5. Prosedur Pemeliharaan & Keamanan Sistem (Maintenance Guide)
1. **Verifikasi Webhook Gateway Pembayaran:**
   * Pastikan endpoint webhook Stripe (`/api/webhooks/stripe`) dan PayPal IPN terenkripsi SSL/TLS dan memverifikasi *signature header* guna menghindari manipulasi status pembayaran.
2. **Pencegahan Race Condition pada Stok:**
   * Saat volume transaksi tinggi, terapkan mekanisme *database row-level locking* (atau Redis distributed locks) saat pengguna menekan tombol bayar agar dua pembeli tidak membeli 1 sisa buku terakhir secara bersamaan.
3. **Audit Data & Pencadangan Rutin (Backup):**
   * Lakukan pencadangan database harian untuk data pesanan dan katalog produk.
   * Simpan riwayat transaksi pelanggan secara terpisah dari log sesi sementara untuk menjaga kepatuhan privasi data (GDPR/UU PDP).
4. **Optimasi Performa Web:**
   * Gunakan kompresi WebP untuk sampul buku fisik dengan rasio aspek standar 2:3.
   * Terapkan *lazy-loading* pada daftar katalog untuk mempertahankan skor Google PageSpeed di atas 90.
