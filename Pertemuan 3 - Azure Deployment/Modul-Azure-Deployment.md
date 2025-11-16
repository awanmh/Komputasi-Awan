# Azure Deployment

**Komputasi Awan**

Semester Ganjil 2025 – 2026

**M. Nizar P. Ma’ady, S.Kom., M.IM., M.Kom.**

**Team Asprak**
- Ricky Adam Saputra
- Wahyu Ade Cahaya
- Cisa Valentino Cahya Ramadhabi
- Setiawan Muhammad

**Pre-Requirement:**
-	Landingpage bebas, yang ada UI nya, pakai PHP native (pastikan Repo Private Access)
-	Akun **Azure Student** Sudah Login
-	Extention **Azure Tool** di VSCode Sudah Login

## Setting Repo projek GitHub ke private :

1.	Masuk ke repo github, kemudian pilih setting

<img src="image/image32.png" />
 
2.	Di menu general, scroll kebawah sampai bertemu danger zone.

<img src="image/image33.png" />

3.	Pilih visibility dan ikuti instruksinya.

<img src="image/image34.png" />

## 1. Konfigurasi App Service Plan

- Cari di pencarian Azure di bagian atas tengah(menu pencarian), `App Service Plans`.

<img src="image/image1.png" />

- Gambar di bawah merupakan tampilan dari **App Service Plans**.

<img src="image/image2.png"/>

- Klik Create di kiri atas.

<img src="image/image3.png"/>

- Untuk name pakai praktikum-cloud-(name) contoh: **praktikum-cloud-bahlil**

<img src="image/image4.png" width="500"/>

**Penjelasan**:
- Untuk Resource Group Create new, dan isi sesuai dengan nama kalian, contoh disini : `PraktikumCloud`
- Sesuaikan Name dari App Service Plan
- Pilih Operating  System Linux
- Untuk Region Pilih Indonesia Central (Pilih Paling Dekat)
- Pricing Plan Pilih Free F1 (Free 60Menit/1 Hari)
Kalau sudah silahkan lakukan `View + Create` → `Create`
Hasil Jika berhasil:

<img src="image/image5.png" width="500"/>

## 2. Konfigurasi App Services

Kita akan buat service/server untuk menampung program kita

Cari `App Services` di pencarian,

<img src="image/image6.png"/>

- Pada kiri atas tekan `Create` → pilih `Web App`

<img src="image/image7.png"/>
<img src="image/image8.png"/>

- Sesuaikan konfigurasinya, Harus tercantum nama temen-temen: `web-praktikum-(name)`

<img src="image/image9.png" width="500"/>

Temen-temen langsung saja klik tombol `Review + create` → `Create`.

Kalau di next akan ada menu deployment, untuk menu Deployment kita akan sambungin di GitHub nya setelah membuat Service/Server ini

Tunggu beberapa saat hasilnya akan seperti ini:

<img src="image/image10.png" width="500"/>


## 3. Menyambungkan Azure Ke GitHub
Kembali ke **App Services** dan pilih Services yang di buat sebelumnya

<img src="image/image11.png"/>

- Pilih titik 3 agar full screen

<img src="image/image12.png" width="500"/>

- Cari **Deployment** → **Deployment Center**, sesuaikan berdasarkan detail berikut:

<img src="image/image13.png"/>

**Penjelasan:**
- Pilih Source : **GitHub** (bisa disesuaikan)
- **SigIn** Terlebih dahulu ke akun **GitHub** nya temen-temen
- Organization : Pilih Akun Yang Ada
- Repository : Repository Project temen-temen
- Branch : Sesuaikan branch project temen” bisa saja di `main`/`master`
- Untuk Authentication biarin saja

Kemudian silahkan temen-temen save di bagian atas

<img src="image/image14.png" width="500"/>

- Jika berhasil akan muncul seperti berikut:

*Tunggu sampai Status Done, masih proses mengambil project temen-temen "In Progress"*

<img src="image/image15.png"/>

Hasilnya akan seperti berikut, status *Succeeded*:

<img src="image/image16.png"/>

Akses URL temen temen pada bagian Overview, Bagian **Default domain**

<img src="image/image17.png"/>

Hasilnya akan seperti berikut ini, sesuai dengan web temen”:

<img src="image/image25.png"/>

jika ada kendala error pastikan role teman-teman sebagai `owner`/`contributor` agar dia memiliki acces full dalam memanage webnya, untuk ceknya :

web-kalian → Acces control (IAM) → Check acces → My access → tekan `View my access`

<img src="image/image31.png"/>
<img src="image/image30.png"/>

---

## Azure Deployment With VSCode

<img src="image/image18.png"/>

Disini kita full VsCode jadi gaperlu github, hal tersebut kurang best practice untuk di perusahaan jadi Opsional, kalau project individu + mau satset untuk pengembangan bisa aja

**1. Buat 1 App Service Lagi**

- Hasil service berhasil di buat:

<img src="image/image19.png" width="500"/>
<img src="image/image21.png" width="500"/>

**2. Login Extension Azure di VsCode**

<img src="image/image20.png"/>

- Jika Service belum terdeteksi silahkan **refresh** terlebih dahulu

<img src="image/image22.png"/>

- Maka service yang di buat sebelumnya akan tampil seperti berikut ini:

<img src="image/image23.png"/>

**3. Deploy VsCode**
- Klik kanan Service yang kita buat tadi dan klik **Deploy to Web App**.

<img src="image/image24.png"/>

- Kemudian pilih yang saat ini atau jika project belum di buka bisa pakai browser

<img src="image/image26.png"/>

- Maka akan muncul seperti berikut, selanjutnya klik `Deploy`

<img src="image/image27.png"/>

**Output:**
- Tunggu beberapa saat, selanjutnya temen-temen bisa melakukan **Browse Website** untuk membuka website nya atau bisa pakai URL di service azure nya

<img src="image/image28.png"/>

**Hasil Web nya:**

<img src="image/image29.png"/>

---

## Tugas Mandiri
1. Silahkan buat Web Apps + Database, (project bebas harus ada CRUD nya)
2. Silahkan jelakan alur konfigurasinya step by step
3. Sertakan Link Apps nya, url web akan di cek oleh Asprak

