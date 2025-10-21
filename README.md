# Centralized License Management System



Sebuah sistem manajemen lisensi terpusat yang modern, aman, dan mudah digunakan, dibangun di atas platform Vercel. Sistem ini memungkinkan developer dan pemilik produk digital untuk melindungi script/website mereka dengan mudah, mengelola semua lisensi dari satu dashboard, dan memberikan pengalaman instalasi "plug-and-play" yang sangat sederhana untuk klien.

## ✨ Fitur Utama

### 👑 Dashboard Admin
- **Manajemen Terpusat:** Tambah, perbarui, dan hapus lisensi untuk semua klien Anda dari satu antarmuka yang intuitif.
- **Desain Responsif:** Didesain dengan pendekatan *Mobile-First*, memastikan Anda dapat mengelola lisensi dari mana saja, baik di desktop maupun di ponsel.
- **Indikator Status Visual:** Lacak status setiap lisensi dengan mudah melalui indikator denyut berwarna (hijau untuk aktif, merah untuk kedaluwarsa).
- **Interaksi Modern:** Proses manajemen lisensi ditangani secara *asynchronous*, memberikan feedback instan tanpa perlu refresh halaman.

### 🔐 Proteksi Sisi Klien
- **Instalasi Super Mudah:** Klien Anda hanya perlu menempelkan **satu baris kode JavaScript** di website mereka. Tidak ada konfigurasi rumit, tidak ada file yang perlu diunggah.
- **Lisensi Terikat ke Domain/Folder:** Lisensi dapat diikat ke domain utama (`domain.com`) atau folder spesifik (`domain.com/produk-anda`), memberikan kontrol akses yang presisi.
- **Verifikasi Real-Time:** Setiap kali pengunjung membuka website klien, script akan secara otomatis menghubungi server Anda untuk memvalidasi lisensi secara *real-time*.
- **Blokir Konten Cerdas:** Script secara efektif memblokir seluruh konten website sebelum verifikasi berhasil, mencegah kebocoran konten.

### 🚀 Arsitektur & Teknologi
- **Serverless & Skalabel:** Dibangun di atas **Vercel Functions**, mampu menangani lalu lintas verifikasi dari ribuan website tanpa perlu mengelola server.
- **Database Persisten:** Menggunakan **Vercel KV (Redis)** untuk penyimpanan data lisensi yang cepat, andal, dan persisten. Data Anda tidak akan hilang saat redeploy.
- **Keamanan:** Menggunakan sesi berbasis cookie (`express-session`) untuk melindungi dashboard admin dan CORS untuk mengamankan API verifikasi.

## 🛠️ Panduan Instalasi & Penggunaan

Sistem ini terdiri dari dua bagian: **Server Lisensi** (yang Anda deploy) dan **Script Klien** (yang Anda berikan ke klien).

### Langkah 1: Deploy Server Lisensi ke Vercel

1.  **Fork atau Clone Repositori Ini:**
    Dapatkan kode proyek ini ke akun GitHub/GitLab Anda.

2.  **Buat Proyek Baru di Vercel:**
    - Buka dashboard Vercel Anda dan klik "Add New... -> Project".
    - Impor repositori yang baru saja Anda fork/clone. Vercel akan secara otomatis mendeteksi bahwa ini adalah proyek Node.js.
    - Vercel akan menanyakan "Root Directory". Biarkan default, lalu klik "Deploy". Proses build pertama mungkin gagal, ini normal karena kita belum mengatur database dan variabel.

3.  **Hubungkan Database Vercel KV (Redis):**
    - Setelah proyek dibuat, buka proyek tersebut di Vercel.
    - Pergi ke tab **"Storage"**.
    - Klik "Connect Store" dan pilih **"KV (new)"** atau **"Upstash (Redis)"**.
    - Ikuti petunjuk untuk membuat database baru dan menghubungkannya ke proyek Anda.
    - Setelah terhubung, Vercel akan otomatis menambahkan variabel lingkungan yang diperlukan.

4.  **Ubah Nama Environment Variables:**
    - Vercel terkadang membuat variabel dengan nama Bahasa Indonesia (`PENYIMPANAN_...`). Kita perlu mengubahnya ke nama standar.
    - Pergi ke tab **"Settings" -> "Environment Variables"**.
    - Ganti nama variabel berikut:
        - `PENYIMPANAN_KV_URL` → `KV_URL`
        - `PENYIMPANAN_KV_REST_API_URL` → `KV_REST_API_URL`
        - `PENYIMPANAN_KV_REST_API_TOKEN` → `KV_REST_API_TOKEN`
    - Klik "Save" setelah setiap perubahan.

5.  **Atur Kredensial Admin:**
    - Masih di halaman "Environment Variables", tambahkan variabel berikut untuk mengamankan login Anda:
      - `ADMIN_USER`: Username yang Anda inginkan (misal: `admin`).
      - `ADMIN_PASS`: Password yang kuat dan aman.
      - `SESSION_SECRET`: Sebuah string acak yang sangat panjang untuk keamanan sesi. Anda bisa membuatnya di [situs generator secret](https://www.grc.com/passwords.htm).
    - Klik "Save".

6.  **Redeploy Proyek:**
    - Pergi ke tab **"Deployments"**.
    - Klik ikon tiga titik (`...`) di samping deployment teratas dan pilih **"Redeploy"**.
    - **Penting:** Pastikan kotak "Use existing Build Cache" **tidak dicentang**.
    - Klik "Redeploy".

Setelah selesai, server lisensi Anda siap digunakan! Buka URL Vercel Anda (misal: `https://nama-proyek-anda.vercel.app`) dan login dengan kredensial yang baru saja Anda atur.

### Langkah 2: Berikan Script ke Klien Anda

1.  **Dapatkan URL Script:**
    URL untuk script klien adalah URL Vercel Anda ditambah dengan nama file script.
    Contoh: Jika Anda menempatkan file `license-caller.js` di dalam folder `public`, maka URL-nya adalah `https://nama-proyek-anda.vercel.app/license-caller.js`. *Pastikan untuk mengupdate URL di dalam file `license-caller.js` itu sendiri terlebih dahulu.*

2.  **Instruksi untuk Klien:**
    Berikan instruksi sederhana ini kepada klien Anda:
    > "Untuk mengaktifkan proteksi lisensi, cukup tambahkan satu baris kode berikut di dalam tag `<head>` pada setiap halaman website Anda:"
    > ```html
    > <script src="https://nama-proyek-anda.vercel.app/license-caller.js" async defer></script>
    > ```

### Langkah 3: Kelola Lisensi
- Buka dashboard admin Anda.
- Di bagian "Tambah / Perbarui Lisensi", masukkan domain atau domain/folder klien (contoh: `klienkeren.com` atau `klienkeren.com/produk-saya`).
- Pilih tanggal kedaluwarsa, lalu klik "Simpan".
- Selesai! Website klien kini terproteksi dan akan aktif sampai tanggal yang Anda tentukan.

---

## 📜 Lisensi & Harga

Sistem ini adalah produk komersial. Penggunaan kode ini untuk tujuan produksi memerlukan lisensi yang valid.

**Harga: Rp 300.000,-** (Lisensi seumur hidup, satu kali bayar)

Dengan membeli lisensi, Anda mendapatkan:
- Hak penuh untuk menggunakan, memodifikasi, dan men-deploy sistem ini.
- Dukungan prioritas untuk instalasi dan troubleshooting.

Jika Anda tertarik untuk membeli atau memiliki pertanyaan lebih lanjut, silakan hubungi kami.

## 📞 Kontak Developer

- **Nama:** ZendsHost
- **Telegram:** [@zendshost](https://t.me/zendshost)
