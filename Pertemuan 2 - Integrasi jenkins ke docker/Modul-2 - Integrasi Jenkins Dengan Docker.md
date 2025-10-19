**Modul: Integrasi Jenkins dengan Docker** **Ringkasan singkat **

Modul ini mengajarkan langkah praktis integrasi Jenkins dengan Docker: menyiapkan Jenkins \(dijalankan di container atau host\), membuat aplikasi contoh, membuat Dockerfile, menulis Jenkinsfile yang membangun image Docker, memberi tag, dan mendorong \(push\) ke Docker Hub. 

Modul berisi petunjuk langkah demi langkah, perintah yang siap-pakai, latihan singkat, kuis, dan bagian troubleshooting yang sering muncul di lab. 



**Tujuan Pembelajaran **

Setelah mengikuti modul ini, peserta **mampu**: 1. Menjalankan Jenkins yang terhubung ke Docker Engine. 

2. Membuat dan menyimpan credential GitHub & Docker Hub di Jenkins dengan aman. 

3. Menulis Jenkinsfile yang membangun image Docker dan mendorong ke registry. 

4. Mengidentifikasi & menyelesaikan masalah umum pada integrasi Jenkins–Docker. 



**Prasyarat **

• Mesin host \(Linux/Windows\) dengan Docker Engine terpasang. 

• Akses administrator ke mesin Jenkins \(atau kemampuan menjalankan container Jenkins\). 

• Akun GitHub dan Docker Hub. 

• Pengetahuan dasar: Git, Docker \(image/container\), dan CLI dasar. 



**Perlengkapan Lab **

• Docker \(Linux host atau Docker Desktop di Windows\) 

• Jenkins \(direkomendasikan: run in container untuk lab\) 

• Browser untuk mengakses UI Jenkins 

• Repository contoh \(GitHub\) yang berisi aplikasi sederhana \(contoh: Flask/Node\) 





**1. Menjalankan Jenkins \(Quick-start\)** Opsi A — Jenkins di Docker \(Linux host, direkomendasikan\) 

*\# buat volume untuk data Jenkins* 

*\# buat volume untuk data Jenkins* 

docker volume create jenkins\_home 

docker volume create jenkins\_home 

*\# jalankan Jenkins dan mount socket Docker* 

*\# jalankan Jenkins dan mount socket Docker* docker run -d --name jenkins-docker \\ 

docker run -d --name jenkins-docker \\ 

-p 8080:8080 -p 50000:50000 \\ 

-p 8080:8080 -p 50000:50000 \\ 

-v jenkins\_home:/var/jenkins\_home \\ 

-v jenkins\_home:/var/jenkins\_home \\ 

-v /var/run/docker.sock:/var/run/docker.sock \\ 

-v /var/run/docker.sock:/var/run/docker.sock \\ jenkins/jenkins:lts 

jenkins/jenkins:lts 

**Kelebihan:** Jenkins berjalan di lingkungan Linux, mudah akses Docker socket \(/var/run/docker.sock\) sehingga pipeline dapat membangun image. 

**Opsi B — Jenkins di Windows \(Docker Desktop\) **

• Pastikan Docker Desktop berjalan. 

• Jika Jenkins dijalankan sebagai Windows Service, ubah *Log On* service Jenkins ke akun user yang menjalankan Docker Desktop agar punya akses ke named pipe Docker \(npipe:////./pipe/docker\_engine\). 



**2. Contoh Proyek \(Struktur repo\)** 

simple-app/ 

simple-app/ 

├─ app.py \# contoh Flask app

├─ app.py \# contoh Flask 

app 

├─ requirements.txt

├─ requirements.t 

xt 

├─ Dockerfile 

├─ Dockerfile 

└─ Jenkinsfile

└─ Jenkinsfi le 

**C** **ontoh app.py \(Flask minimal\):** **from** flask **import** Flask 

**from** flask **import** Flask 

app = Flask\(\_\_name\_\_\) 

app = Flask\(\_\_name\_\_\) 

@app.route\('/'\) 

@app.route\('/'\) 

**def** hello\(\): 

**def** hello\(\): 

**return** "Halo dari Flask \+ Docker \+ Jenkins\!" 

**return** "Halo dari Flask \+ Docker \+ 

Jenkins\!" 

**if** \_\_name\_\_ == '\_\_main\_\_': 



app.run\(host='0.0.0.0', port=5000\) 

**if** \_\_name\_\_ == '\_\_main\_\_': 

app.run\(host='0.0.0.0', port=5000 

**requirements.txt: **

Flask==2.2.5 



**Dockerfile:** 

**FROM** python:3.10-slim 

**FROM** python:3.10-slim 

**WORKDIR** /app 

**WORKDIR** /app 

**COPY** requirements.txt ./ 

**COPY** requirements.txt ./ 

**RUN** pip install --no-cache-dir -r requirements.txt **RUN** pip install --no-cache-dir -r **COPY** . . 

requirements.txt 

**EXPOSE** 5000 

**COPY** . . 

**CMD** \["python", "app.py"\]

**EXPOSE** 5000 

**CMD** \["python", "app.py"\] 



**3. Jenkinsfile — Contoh \(Linux agent\)** pipeline \{ 

pipeline \{ 

agent any 

agent any 

environment \{ 

environment \{ 

IMAGE\_NAME = 'youruser/simple-app' 

IMAGE\_NAME = 'youruser/simple-app' 

REGISTRY = 'https://index.docker.io/v1/' 

REGISTRY = 'https://index.docker.io/v1/' 

REGISTRY\_CREDENTIALS = 'dockerhub-credentials' 

REGISTRY\_CREDENTIALS = 'dockerhub-credentials' 

\} 

\} 

stages \{ 

stages \{ 

stage\('Checkout'\) \{ steps \{ checkout scm \} \} 

stage\('Checkout'\) \{ steps \{ checkout scm \} \} 

stage\('Build'\) \{ 

stage\('Build'\) \{ 

steps \{ sh 'echo "Mulai build aplikasi"' \} 

steps \{ sh 'echo "Mulai build aplikasi"' \} 

\} 

\} 

stage\('Build Docker Image'\) \{ 

stage\('Build Docker Image'\) \{ 

steps \{ script \{ docker.build\("$\{IMAGE\_NAME\}:$\{env.BUILD\_NUMBER\}"\) \} \} 

steps \{ script \{ docker.build\("$\{IMAGE\_NAME\}:$\{env.BUILD\_NUMBER\}"\) \} \} 

\} 

\} 

stage\('Push Docker Image'\) \{ 

stage\('Push Docker Image'\) \{ 

steps \{ 

steps \{ 

script \{ 

script \{ 

docker.withRegistry\(REGISTRY, REGISTRY\_CREDENTIALS\) \{ 

docker.withRegistry\(REGISTRY, REGISTRY\_CREDENTIALS\) \{ 

def tag = "$\{IMAGE\_NAME\}:$\{env.BUILD\_NUMBER\}" 

def tag = "$\{IMAGE\_NAME\}:$\{env.BUILD\_NUMBER\}" 

docker.image\(tag\).push\(\) 

docker.image\(tag\).push\(\) 

docker.image\(tag\).push\('latest'\) 

docker.image\(tag\).push\('latest'\) 

\} 

\} 

\} 

\} 

\} 

\} 

\} 

\} 

\} 

\} 

post \{ always \{ echo 'Selesai build' \} \} 

post \{ always \{ echo 'Selesai build' \} \} 

\} \} 

**Catatan:** pada agent Windows, gunakan bat dan docker login manual \(withCredentials\). Contoh Windows ada di lampiran. 





**Jenkinsfile — Contoh \(Windows agent\)** pipeline \{

pipeline \{ 

agent any

agent



any 

environment \{

environment \{ 

IMAGE\_NAME = 'youruser/simple-app' 

IMAGE\_NAME = 'youruser/simple-



app' 

REGISTRY\_CREDENTIALS = 'dockerhub-credentials' 

REGISTRY\_CREDENTIALS = 'dockerhub-credential s' 

\}

\} 

stages \{

stages \{ 

stage\('Checkout'\) \{ steps \{ checkout scm \} \}

stage\('Checkout'\) \{ steps \{ checkout scm \} \} 

stage\('Build'\) \{ steps \{ bat 'echo "Build di Windows"' \} \}

stage\('Build'\) \{ steps \{ bat 'echo "Build di Windows"' \} \} 

stage\('Build Docker Image'\) \{ steps \{ bat "docker build -t %IMAGE\_NAME%:%BUILD\_NUMBER% 

stage\('Build Docker Image'\) \{ steps \{ bat "docker build -t 

." \} \}

%I 

MAGE\_NAME%:%BUILD\_NUMBER% ." \} \} 

stage\('Push Docker Image'\) \{

stage\('Push Docker Image'\) \{ 

steps \{

steps \{ 

withCredentials\(\[usernamePassword\(credentialsId: REGISTRY\_CREDENTIALS, withCredentials\(\[usernamePassword\(credentialsId: REGISTRY\_CREDENTIALS, usernameVariable: 'USER', passwordVariable: 'PASS'\)\]\) \{

usernameVariable: 'USER', passwordVariable: 'PASS'\)\]\) \{ 

bat 'docker login -u %USER% -p %PASS%' 

bat 'docker login -u %USER% -p %PASS 

%' 

bat "docker push %IMAGE\_NAME%:%BUILD\_NUMBER%" 

bat "docker push %IMAGE\_NAME%:%BUILD\_NUMBER% " 

bat "docker tag %IMAGE\_NAME%:%BUILD\_NUMBER% %IMAGE\_NAME%:latest" 

bat "docker tag %IMAGE\_NAME%:%BUILD\_NUMBER% %IMAGE\_NAME%:latest " 

bat "docker push %IMAGE\_NAME%:latest" 

bat "docker push %IMAGE\_NAME%:latest " 

\}

\} 

\}

\} 

\}

\} 

\}

\} 

\} \} 





**4. ** **Membuat Personal Access Token \(PAT\)** **GitHub PAT** \(jika repo private\): - Settings → Developer settings → Personal access tokens → 

Fine-grained / Classic - Scope minimal: repo \(atau public\_repo untuk repo public\) **Docker Hub PAT**: - Docker Hub → Account settings → Security → New Access Token - Beri permission: Read, Write, Delete \(untuk push\) 

****

**5. Menyimpan Credential di Jenkins **

1. Buka *Manage Jenkins* → *Manage Credentials* → \(Global\) → *Add Credentials* 2. Tambah credential: **Kind:** Username with password o Username: Docker Hub username 

o Password: Docker PAT 

o ID: dockerhub-credentials \(catat; akan dipakai di Jenkinsfile\) 3. Untuk GitHub \(jika private\): bisa simpan sebagai **Secret text** \(token\) atau **Username** **with password**. 

**6. Menyimpan Credential di Jenkins** 4. New Item → Pipeline → beri nama → OK 

5. Di bagian *Pipeline*, pilih *Pipeline script from SCM* jika repo di Git o SCM: Git 

o Repository URL: https://github.com/xxx/simple-app.git o Credentials \(jika private\) 

o Script Path: Jenkinsfile 

6. Simpan, lalu *Build Now*. 



**7. Checklist Verifikasi \(setelah pipeline berjalan\) **

☐ Jenkins berhasil checkout dari repo \(cek console log\) 

☐ docker build terlihat berhasil di log 

☐ docker login & docker push menunjukkan Login Succeeded dan pushed 

☐ Tag muncul di Docker Hub UI 



**8. Troubleshooting Cepat \(kasus umum\) **

• **sh: command not found** → Agent Windows menjalankan sh. Gunakan bat di Windows pipeline atau jalankan agent Linux. 

• **docker daemon is not running** → Jalankan Docker Desktop / service Docker. 

• **named pipe error \(Windows\)** → Pastikan Jenkins service dan Docker Desktop dijalankan oleh user yang sama. 

• **credential tidak ditemukan** → Pastikan credentialsId sama dan disimpan di scope Global. 

• **token scope insufficient** → Buat token baru dengan permission Read/Write/Delete. 

• **push lambat / rate limiting** → Cek koneksi, gunakan registry lokal \(Harbor\) di lab. 





**9. Latihan & Tugas \(Praktek\)** 1. **Latihan cepat \(15 menit\):** Clone repo contoh, tambahkan credential Docker Hub di Jenkins, jalankan pipeline, amati proses build & push. 

2. **Tugas mandiri \(30 menit\):** Tambahkan stage Unit Test sebelum Build Docker Image. 

Gunakan pytest \(atau framework test sesuai bahasa\). Pipeline hanya boleh push jika test lulus. 



**11. Referensi & Bahan Bacaan **

• Official Jenkins docs \(Pipeline, Credentials\) 

• Docker docs \(images, login, registries\) 

• Artikel best-practices: jangan mount /var/run/docker.sock di server publik tanpa proteksi



