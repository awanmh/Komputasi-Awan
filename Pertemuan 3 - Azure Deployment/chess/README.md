# Proyek Catur (PHP Server-Side)

Sebuah aplikasi catur server-side yang dibuat murni dengan PHP 8.x. Proyek ini mengimplementasikan aturan catur lengkap di mana semua logika permainan dan manajemen status (state) dikelola di sisi server menggunakan PHP Sessions.

## Fitur Utama

Logika permainan inti tetap sama dengan versi JavaScript, mencakup:

Aturan Lengkap: Mendukung Castling (Rokade), En Passant, dan Pawn Promotion.

Deteksi Status: Secara otomatis mendeteksi Skak (Check), Skakmat (Checkmate), dan Stalemate.

Validasi Langkah: Hanya mengizinkan langkah yang legal, termasuk logika untuk memblokir skak.

Manajemen Status Sisi Server: Seluruh status papan, giliran, dan riwayat permainan dikelola dengan aman di server menggunakan `$_SESSION`.

### Fitur UI:

- Undo: Membatalkan 1 langkah terakhir (menggunakan riwayat di session).

- Reset: Mengulang permainan dari awal.

- Flip Board: Membalik orientasi papan.

- Captured Pieces: Menampilkan semua bidak yang telah dimakan.

---

## Teknologi (Tech Stack)

- Proyek ini telah dialihkan dari aplikasi statis menjadi aplikasi dinamis yang server-side.

- PHP 8.x: Digunakan untuk semua logika permainan, validasi langkah, rendering HTML, dan manajemen status.

- PHP Sessions (`$_SESSION`): Digunakan untuk menyimpan dan mengelola status permainan (papan, giliran, dll.) untuk setiap pemain di antara setiap klik (HTTP request).

- HTML & CSS: Digunakan untuk UI, yang sekarang dirender secara dinamis oleh PHP.

---

## Menjalankan Secara Lokal

### Cara Menjalankan dengan XAMPP

**Langkah 1: Pindahkan File Anda**
Pindahkan file `index.php` Anda ke dalam folder `htdocs` di dalam direktori instalasi XAMPP Anda.

* **Lokasi Default:** `C:\xampp\htdocs\`
* **Rekomendasi:** Buat folder baru di dalamnya agar rapi, misalnya: `C:\xampp\htdocs\catur\`
* **Jadi, path file Anda adalah:** `C:\xampp\htdocs\catur\index.php`

**Langkah 2: Jalankan Server Apache**
Ini adalah bagian penting yang Anda tanyakan:

1.  Buka **XAMPP Control Panel** (cari di Start Menu Anda).
2.  Anda akan melihat daftar "Module". Untuk proyek ini, kita **hanya butuh Apache**.
3.  Klik tombol **"Start"** di sebelah modul **Apache**.
4.  Tunggu beberapa detik. Jika berhasil, "Apache" akan menyala dengan latar belakang hijau.
    *(Anda tidak perlu menyalakan MySQL, karena kode kita tidak menggunakan database).*

**Langkah 3: Buka di Browser**
Sekarang server Anda sudah berjalan di `localhost`.

* Buka browser (Chrome, Firefox, dll.).
* Ketikkan alamat `localhost` diikuti dengan nama folder Anda.
* **Alamat URL:** `http://localhost/catur/` atau `http://localhost/catur/index.php`

Maka permainan catur Anda akan muncul dan siap dimainkan!

---

### Cara Menjalankan dengan Laragon

Laragon sering dianggap lebih modern dan otomatis. Caranya sedikit berbeda:

**Langkah 1: Pindahkan File Anda**
Laragon menggunakan folder `www` sebagai *root* dokumennya.

* **Lokasi Default:** `C:\laragon\www\`
* **Rekomendasi:** Sama seperti XAMPP, buat folder baru, misalnya: `C:\laragon\www\catur\`
* **Jadi, path file Anda adalah:** `C:\laragon\www\catur\index.php`

**Langkah 2: Jalankan Server**

1.  Buka aplikasi **Laragon**.
2.  Anda akan melihat tombol besar di tengah. Klik **"Start All"**.
    
3.  Ini akan secara otomatis menyalakan **Apache** (atau Nginx, tergantung pengaturan Anda) dan **MySQL**.

**Langkah 3: Buka di Browser (Ini Kelebihan Laragon)**
Laragon secara otomatis membuat "pretty URL" untuk Anda.

* **Cara 1 (Otomatis):** Jika Anda menamai folder Anda `catur` (di `C:\laragon\www\catur`), Laragon otomatis membuat URL `http://catur.test`. Coba buka URL tersebut di browser Anda.
* **Cara 2 (Manual):** Jika cara otomatis tidak berfungsi, Anda tetap bisa menggunakan `http://localhost/catur/`.

---

## Deployment ke Azure (Untuk Praktikum)

Aplikasi ini adalah aplikasi dinamis (PHP), bukan lagi statis. Anda tidak bisa menggunakan Azure Blob Storage atau Static Web Apps. Anda harus menggunakan layanan yang dapat menjalankan kode PHP.

Pilihan terbaik untuk praktikum adalah **Azure App Service**.

1. Buka Portal Azure dan buat **"App Service"** baru.

2. Saat membuat, konfigurasikan **"Runtime stack"**. Pilih **PHP 8.x** (sesuai versi yang Anda gunakan, misal 8.2 atau 8.3).

3. Pilih **"Operating System"**, misalnya Linux.

4. Selesaikan pembuatan App Service.

5. Setelah App Service Anda dibuat, buka menu **"Deployment Center"**.

6. Metode Paling Mudah (GitHub):

   - Tempatkan file `.php` Anda di repositori GitHub(pastikan namanya `index.php` agar dimuat otomatis).

   - Hubungkan Deployment Center ke repositori GitHub Anda.

   - Azure akan secara otomatis mengambil (`pull`) dan mendeploy file Anda setiap kali Anda melakukan `push` ke repositori.

7. Metode Alternatif (FTP):

   - Di App Service Anda, cari credentials (nama pengguna, kata sandi, dan host) untuk FTPS.

   - Gunakan klien FTP seperti FileZilla untuk terhubung ke server Anda.

   - Upload file `.php` ke folder `wwwroot` di server Azure Anda.

8. Setelah di-deploy, kunjungi URL publik yang disediakan oleh Azure App Service Anda untuk memainkan game catur.


---

