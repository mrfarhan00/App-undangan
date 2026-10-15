# PRD: Platform Self-Service Undangan Digital Pernikahan

**Status:** Draft untuk review  
**Versi:** 0.1  
**Tanggal:** 15 Oktober 2026  
**Stack database:** Prisma ORM (provider/database TBD)  
**Stack aplikasi, hosting, dan tenggat:** TBD

## 1. Executive Summary

**Problem Statement**  
Pasangan yang ingin mengundang tamu secara digital membutuhkan cara yang mudah untuk membuat, mengatur, dan membagikan undangan tanpa proses desain atau teknis yang rumit. Informasi acara dan pengalaman tamu juga perlu tampil rapi di satu halaman yang dapat dibuka melalui tautan publik.

**Proposed Solution**  
Bangun platform self-service yang memungkinkan pasangan memilih template, mengisi konten pernikahan, lalu menerbitkan undangan pada slug publik. Platform menyediakan akun, paket berlangganan, pembayaran Midtrans Snap, katalog dan editor undangan, panel admin, serta fitur untuk tamu.

**Success Criteria**  
Target berikut adalah usulan awal untuk divalidasi setelah baseline dan periode pengukuran MVP ditentukan.

- Sedikitnya 70% pengguna baru yang memulai pembuatan undangan berhasil menerbitkan satu undangan dalam 24 jam.
- Sedikitnya 90% percobaan publish yang memenuhi validasi berhasil menghasilkan halaman publik yang dapat dibuka.
- Sedikitnya 95% halaman undangan yang telah diterbitkan merespons dengan status sukses pada pemeriksaan ketersediaan bulanan.
- Sedikitnya 80% pengguna yang memulai checkout paket berbayar menyelesaikan pembayaran atau menerima status transaksi final yang dapat dijelaskan.

## 2. User Experience & Functionality

### User Personas

- **Pasangan/penyelenggara:** ingin membuat undangan yang sesuai acara, mengubah konten sendiri, dan membagikan satu tautan kepada tamu.
- **Tamu undangan:** ingin melihat detail acara dengan jelas di ponsel atau desktop serta mengirim ucapan tanpa harus membuat akun.
- **Admin platform:** mengelola katalog template dan kategori agar pilihan yang tersedia tetap akurat dan layak digunakan.

### User Stories dan Acceptance Criteria

#### A. Akun dan akses

**Story:** Sebagai pasangan, saya ingin mendaftar dan masuk dengan email dan kata sandi agar undangan saya tersimpan dan hanya dapat dikelola oleh saya.

- Pengguna dapat mendaftar, masuk, dan keluar menggunakan email dan kata sandi.
- Sistem mengirim verifikasi email; status verifikasi terlihat di akun.
- Pengguna dapat meminta tautan reset kata sandi melalui email. Token reset bersifat sekali pakai dan kedaluwarsa.
- Akses edit dibatasi pada pemilik undangan dan admin yang berwenang.
- Perilaku produk bila email belum diverifikasi (misalnya batasan publish) ditentukan sebelum implementasi: **TBD**.

#### B. Paket dan pembayaran

**Story:** Sebagai pasangan, saya ingin melihat pilihan paket dan membayar dengan Midtrans Snap agar batas undangan aktif pada akun saya sesuai paket yang dipilih.

- Halaman paket menjelaskan harga, periode, serta jumlah maksimum undangan aktif dengan jelas.
- Pengguna dapat memulai checkout menggunakan Midtrans Snap.
- Status pembayaran diperbarui melalui notifikasi server-to-server Midtrans yang diverifikasi; redirect browser saja tidak dianggap bukti pembayaran.
- Pemrosesan notifikasi idempoten sehingga notifikasi berulang tidak menggandakan aktivasi paket.
- Setiap perubahan status transaksi dicatat dan dapat ditelusuri oleh admin.
- Paket, harga, periode, aturan perpanjangan, masa tenggang, dan refund: **TBD**.
- Definisi “aktif” dan perilaku saat batas paket terlampaui (termasuk downgrade atau kedaluwarsa): **TBD**.

#### C. Katalog, pembuatan, pengeditan, dan publikasi

**Story:** Sebagai pasangan, saya ingin memilih template, mengisi konten acara, melihat hasilnya, lalu menerbitkan undangan pada tautan publik agar dapat segera dibagikan.

- Katalog menampilkan template yang tersedia dan dapat disaring berdasarkan kategori.
- Pengguna dapat membuat undangan dari template dan menyimpan perubahan sebagai draft.
- Editor menyediakan preview yang mencerminkan halaman publik pada desktop dan ponsel.
- Sebelum publish, sistem memvalidasi field wajib dan ketersediaan slug.
- Slug publik unik dan URL undangan dapat dibuka tanpa login.
- Pengguna dapat mengubah konten undangan dan menerbitkan pembaruan.
- Status undangan (minimal draft/published) ditampilkan kepada pemilik.
- Pengguna tidak dapat mengubah konfigurasi tema milik template, termasuk warna. Kontrol editor tidak menampilkan opsi untuk mengubah themeConfig.
- Aturan perubahan slug, unpublish, penghapusan, dan dampak terhadap kuota: **TBD**.

#### D. Konten undangan

**Story:** Sebagai pasangan, saya ingin menyajikan detail acara dan cerita kami dalam satu undangan agar tamu mendapatkan informasi yang dibutuhkan.

- Model acara mendukung sekurangnya dua agenda terpisah: akad dan resepsi; setiap agenda memiliki tanggal, waktu, nama acara, lokasi, dan tautan peta opsional.
- Undangan mendukung galeri foto, cerita pasangan, informasi hadiah digital, dan countdown menuju tanggal acara.
- Countdown menggunakan tanggal dan zona waktu acara yang dikonfigurasi; sebelum zona waktu menjadi configurable, gunakan Asia/Jakarta sebagai default yang harus disetujui produk.
- Field konten dapat disimpan sebagai draft sebelum publish.
- Batas jumlah dan ukuran foto, tipe file, format hadiah digital, serta batas panjang cerita: **TBD**.

#### E. Halaman publik dan Wall of Wishes

**Story:** Sebagai tamu, saya ingin membuka undangan dan mengirim ucapan tanpa membuat akun agar saya dapat merespons dengan mudah.

- Halaman publik responsif di viewport desktop dan ponsel, serta memuat detail acara dan konten yang diterbitkan.
- Informasi inti (nama pasangan, tanggal, agenda, lokasi) tetap terbaca tanpa bergantung pada animasi.
- Tamu dapat mengirim ucapan dengan nama dan pesan tanpa login.
- Ucapan yang berhasil dikirim dapat ditampilkan pada Wall of Wishes sesuai kebijakan moderasi.
- Sistem memiliki perlindungan spam dasar, batas frekuensi kirim, dan kemampuan admin untuk menyembunyikan ucapan.
- Apakah ucapan langsung tampil atau menunggu moderasi, batas panjang pesan, dan aturan privasi: **TBD**.

#### F. Panel admin

**Story:** Sebagai admin, saya ingin mengelola template dan kategori agar katalog publik tetap terawat.

- Admin terotorisasi dapat membuat, membaca, mengubah, dan mengarsipkan kategori.
- Admin terotorisasi dapat membuat, membaca, mengubah, dan mengarsipkan template serta menetapkan kategorinya.
- Admin dapat menetapkan dan mengubah themeConfig template.
- themeConfig hanya dapat diubah oleh admin dan tidak dapat ditimpa melalui payload editor pengguna maupun endpoint publik.
- Tindakan admin penting (perubahan template/kategori, pengarsipan) memiliki catatan audit.
- Arsip kategori/template yang sedang digunakan oleh undangan tidak merusak data undangan yang sudah diterbitkan; aturan fallback katalog: **TBD**.

### Non-Goals

- Aplikasi native iOS/Android; MVP berupa web responsif.
- RSVP, pengelolaan daftar tamu, seating chart, atau pengiriman undangan massal melalui WhatsApp/SMS/email.
- Pembuat desain bebas atau unggah template oleh pengguna.
- Multi-bahasa dan mata uang selain rupiah.
- Integrasi payment gateway selain Midtrans Snap pada MVP.
- Moderasi otomatis berbasis AI.

### Alur Pengguna Utama

1. Pasangan membuat akun dan menyelesaikan verifikasi email sesuai kebijakan produk.
2. Pasangan memilih paket bila diperlukan, memeriksa kuota, dan menyelesaikan checkout.
3. Pasangan memilih kategori dan template.
4. Pasangan mengisi agenda, galeri, cerita, informasi hadiah, dan slug; perubahan disimpan sebagai draft.
5. Pasangan melihat preview, memperbaiki validasi, lalu publish.
6. Sistem menyediakan URL publik untuk dibagikan.
7. Tamu membuka URL, membaca detail, dan mengirim ucapan.
8. Pasangan melihat ucapan di Wall of Wishes; admin dapat memoderasi sesuai kebijakan.

## 3. AI System Requirements (If Applicable)

Tidak berlaku untuk MVP. Tidak ada fitur AI yang termasuk dalam ruang lingkup saat ini.

## 4. Technical Specifications

### Architecture Overview

Usulan arsitektur tingkat tinggi; Prisma ORM digunakan sebagai lapisan akses database. Pilihan provider/database dan penyedia hosting tetap **TBD**.

- Aplikasi web responsif menyediakan pengalaman katalog, editor, akun, checkout, panel admin, dan halaman publik.
- Backend/API mengautentikasi pengguna, memeriksa kepemilikan dan kuota, memvalidasi konten, serta mengelola draft dan publikasi.
- Prisma ORM menjadi lapisan akses aplikasi ke database relasional. Prisma schema dan migration mengelola model akun, paket dan entitlement, transaksi, kategori, template beserta themeConfig, undangan, agenda, galeri, serta ucapan.
- Penyimpanan objek digunakan untuk foto; akses unggah dibatasi berdasarkan tipe, ukuran, dan kepemilikan.
- Integrasi Midtrans Snap membuat transaksi dan menerima webhook terverifikasi untuk memperbarui entitlement.
- Render publik membaca versi undangan yang diterbitkan. Perubahan draft tidak mengubah halaman live hingga publish berikutnya.

Entitas minimum yang disarankan: `User`, `SubscriptionPlan`, `Subscription`, `PaymentTransaction`, `TemplateCategory`, `Template`, `Invitation`, `EventSchedule`, `GalleryAsset`, `Wish`, dan `AuditLog`.

### Integrasi Points

- **Email:** layanan untuk verifikasi akun dan reset kata sandi; penyedia dan kebijakan retry TBD.
- **Midtrans Snap:** pembuatan transaksi dan webhook status pembayaran. Secret disimpan di server; signature/status diverifikasi sebelum entitlement diperbarui.
- **Penyimpanan foto:** object storage dengan URL akses terkontrol atau URL publik yang tidak mengungkap kredensial.
- **Peta:** tautan lokasi opsional, tanpa ketergantungan pada API peta untuk render inti.
- **Database:** akses melalui Prisma ORM; provider/database (misalnya PostgreSQL atau MySQL), versi Prisma, dan strategi deployment migration TBD.
- **Auth:** pilihan vendor/teknologi TBD. Hindari menyimpan kata sandi dalam bentuk plaintext; gunakan mekanisme hash yang disediakan sistem auth terpilih.

### Security & Privacy

- Terapkan kontrol akses berbasis peran untuk pengguna dan admin serta pemeriksaan kepemilikan pada setiap operasi edit.
- Otorisasi server menjadi sumber kebenaran untuk kuota dan entitlement; pembatasan UI saja tidak cukup.
- themeConfig template hanya dapat ditulis melalui operasi admin. API pengguna menerima konten undangan yang diizinkan dan mengabaikan/menolak field themeConfig.
- Validasi dan sanitasi konten ucapan dan teks pengguna untuk mencegah XSS; gunakan proteksi CSRF sesuai pola autentikasi.
- Verifikasi webhook pembayaran, dukung idempotensi, dan jangan mempercayai status dari parameter redirect klien.
- Batasi unggahan foto menurut MIME sebenarnya, ukuran, jumlah, dan kepemilikan; jangan menerima file executable.
- Sediakan kebijakan retensi dan penghapusan akun/data, persetujuan pemrosesan data, serta informasi privasi yang sesuai peraturan Indonesia yang berlaku: detail dan tinjauan legal TBD.
- Catat aktivitas keamanan dan administratif yang relevan tanpa memasukkan kata sandi, token, atau rahasia pembayaran ke log.

## 5. Risks & Roadmap

### Phased Rollout

#### MVP

- Akun email/kata sandi, verifikasi email, dan reset kata sandi.
- Satu atau lebih paket dengan batas undangan aktif yang dikonfigurasi; checkout Midtrans Snap.
- Katalog kategori/template, editor berbasis field, draft, preview, slug, dan publish.
- Konten agenda akad/resepsi, galeri, cerita pasangan, hadiah digital, dan countdown.
- Halaman publik responsif dan Wall of Wishes dengan moderasi dasar.
- Admin CRUD template/kategori dan pengaturan themeConfig.
- Instrumentasi funnel dari pendaftaran hingga publish.

#### v1.1

- Penguatan operasional: alat dukungan transaksi, retry email/webhook, laporan penggunaan paket, serta peningkatan moderasi dan perlindungan spam berdasarkan data MVP.
- Penyempurnaan editor dan aksesibilitas berdasarkan umpan balik pengguna.

#### v2.0

- Kandidat eksplorasi setelah validasi kebutuhan: RSVP, daftar tamu, variasi paket/fitur, analitik kunjungan, dan integrasi distribusi. Tidak termasuk komitmen MVP.

### Technical Risks

- **Webhook pembayaran terlambat atau berulang:** dapat menyebabkan kuota salah. Mitigasi: idempotensi, rekonsiliasi transaksi, status pembayaran eksplisit, dan alat admin.
- **Kuota race condition:** beberapa proses publish dapat melampaui limit. Mitigasi: penegakan kuota atomik di backend/database.
- **Slug bentrok atau berubah:** tautan yang dibagikan dapat gagal. Mitigasi: constraint unik dan kebijakan perubahan slug yang jelas.
- **Foto berukuran besar atau tidak aman:** dapat menaikkan biaya dan mengganggu performa. Mitigasi: batas upload, validasi, dan optimasi gambar.
- **Konten publik berisi spam/penyalahgunaan:** dapat merusak pengalaman tamu. Mitigasi: rate limit, moderasi, pelaporan, dan kontrol admin.
- **Tema bocor ke kontrol pengguna:** dapat menghilangkan konsistensi template. Mitigasi: validasi allowlist di API dan pemisahan field themeConfig dari payload edit pengguna.
- **Ambiguitas paket dan status aktif:** tanpa aturan periode, kedaluwarsa, dan unpublish, perilaku limit tidak konsisten. Mitigasi: putuskan definisi paket sebelum implementasi pembayaran.

### Keputusan yang Masih TBD

- Stack aplikasi, hosting, provider/database untuk Prisma, versi Prisma, strategi deployment migration, dan penyedia email.
- Harga, periode paket, kuota tiap paket, aturan pembaruan, grace period, refund, serta perilaku saat paket habis.
- Batas upload foto dan pilihan layanan penyimpanan.
- Kebijakan moderasi ucapan, retensi data, dan penghapusan akun.
- Kebijakan slug, draft/published/unpublish, dan definisi undangan aktif.
- Target metrik final, periode pengukuran, serta nilai baseline.
- Tenggat dan kapasitas tim.
