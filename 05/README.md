## SISTEM TERDISTRIBUSI DAN TERDESENTRALISASI

**Modul 5: Fault Tolerance**

Nama:Avatar Dava Wahyuputra
NIM:255410008


### **BAB I: PENDAHULUAN**

* 1.1 Latar Belakang / Tujuan Praktikum
  Materi ini merupakan materi Praktikum SIstem Terdistribusi dan Terdesentralisasi
untuk pembahasan tentang Fault Tolerance. Pembahasan tentang load balancing diperlukan
agar high-availability dari suatu aplikasi bisa tercapai dengan melakukan proses scaling
aplikasi menjadi lebih dari satu dan mengkonfigurasi proxy load balancer.

### **BAB II: LOAD BALANCING APLIKASI (BAGIAN 4.1)**

* **2.1 Persiapan Environment**
* Instalasi Docker dan Docker Compose
<img width="454" height="472" alt="image" src="https://github.com/user-attachments/assets/b0ae9ff5-d7c4-428f-9d38-b4dffa1b5094" />
penjelasan:
gambar tersebut menujukan cara menginstal docker di laptop kita untuk persiapan melakukan praktik

* Pembuatan workspace direktori dan pengaturan *environment Python* (menggunakan *package manager* seperti `uv`)
<img width="466" height="327" alt="image" src="https://github.com/user-attachments/assets/ef4e2d2b-9ac8-4b7d-aaf8-69477c5f7bde" />
penjelasan:
Gambar tersebut menunjukkan proses di terminal PowerShell menggunakan alat *uv* untuk mengecek daftar versi Python yang tersedia, membuat *virtual environment* baru berbasis Python 3.12, mengaktifkannya, serta memverifikasi versi Python yang sedang aktif di dalam lingkungan virtual tersebut.

* Instalasi framework BlackSheep dan `blacksheep-cli`
<img width="324" height="424" alt="image" src="https://github.com/user-attachments/assets/c13f786f-5ad5-4e25-a037-3867588335b4" />

penjelasan:
Gambar tersebut menunjukkan proses di terminal PowerShell saat pengguna menginstal framework web BlackSheep beserta pustaka pendukungnya dan blacksheep-cli menggunakan alat manajemen paket uv pip, yang ditandai dengan berhasil terinstalnya berbagai dependensi seperti blacksheep==2.6.4, rodi, click, dan komponen CLI lainnya ke dalam virtual environment yang akt


* **2.2 Pembuatan dan Pengujian Aplikasi Web**
* Pembuatan proyek baru menggunakan perintah `blacksheep create` (template MVC, OpenAPI, TOML)
<img width="233" height="139" alt="image" src="https://github.com/user-attachments/assets/542ca829-891b-48fd-8bc8-d1566f51159e" />
penjelasan:
Gambar tersebut menunjukkan proses pembuatan proyek baru menggunakan utilitas CLI BlackSheep di terminal, di mana pengguna memasukkan konfigurasi interaktif seperti nama proyek blacksheep_lb, templat mvc, pemilihan dokumentasi OpenAPI, serta format pengaturan TOML, sehingga proyek berhasil dibuat dan siap untuk dilanjutkan dengan instalasi dependensi serta menjalankan server pengembangan.

* Instalasi *dependencies* dari `requirements.txt`
<img width="345" height="158" alt="image" src="https://github.com/user-attachments/assets/1d2f2f7e-9746-494b-891e-6971b0dd9462" />
penjelasan:
Gambar tersebut menunjukkan proses di terminal PowerShell saat pengguna menginstal seluruh paket pustaka dan dependencies yang diperlukan untuk proyek melalui perintah uv pip install -r requirements.txt di dalam direktori proyek blacksheep_lb, menggunakan virtual environment Python 3.12.14.

* Pengujian aplikasi lokal menggunakan skrip `dev.py` pada port `44777`*
  <img width="395" height="58" alt="image" src="https://github.com/user-attachments/assets/ba807034-00c3-4c42-ba62-5a8c1134db8e" />

<img width="467" height="430" alt="image" src="https://github.com/user-attachments/assets/d46a46ee-910b-4bbe-a5af-b6bbb61b8452" />
penjelasan:
Gambar tersebut menampilkan tangkapan layar dari halaman utama aplikasi web BlackSheep yang berhasil dijalankan di peramban web pada port lokal, menampilkan maskot domba khas BlackSheep serta teks deskripsi mengenai kerangka kerja web asinkron berbasis Python tersebut.



* **2.3 Konfigurasi Docker & Load Balancer**
* Penyusunan struktur direktori proyek (*blacksheep_lb*, *nginx*, *docker-compose.ymal
  <img width="213" height="353" alt="image" src="https://github.com/user-attachments/assets/7a05547b-67a0-459f-935f-4d2070aef455" />
penjelasan:
Gambar tersebut menampilkan struktur hierarki direktori proyek di dalam editor Visual Studio Code, yang memperlihatkan folder utama workspace-05 yang berisi direktori aplikasi blacksheep_lb (dengan subfolder app, domain, tests, serta berkas-berkas seperti Dockerfile, dev.py, dan requirements.txt) serta folder penunjang nginx yang mencakup berkas konfigurasi nginx.conf dan Dockerfile untuk kebutuhan load balancer.   


* Penyesuaian Dockerfile aplikasi (`blacksheep_lb/Dockerfile`) untuk versi Python `3.14.4-slim` dan port `80`
<img width="452" height="356" alt="image" src="https://github.com/user-attachments/assets/b8ebd792-e7b7-444f-b5ef-360bc15c188e" />
Gambar tersebut menampilkan isi berkas Dockerfile untuk aplikasi BlackSheep, yang menggunakan metode multi-stage build dengan basis python:3.13.1-slim untuk menginstal dependensi dan membangun server pada tahap pertama, lalu menyalin hasilnya ke image produksi yang lebih ringkas pada tahap kedua, serta mengatur perintah otomatis untuk menjalankan aplikasi menggunakan uvicorn pada port 80.   
  


* Konfigurasi NGINX *Reverse Proxy* (`nginx/nginx.conf`) dengan mendefinisikan grup `upstream backend`
<img width="337" height="230" alt="image" src="https://github.com/user-attachments/assets/fcb4d7a4-2136-4216-b1c8-7009c6e1b782" />
penjelasan:
Kode di atas adalah berkas konfigurasi NGINX (nginx.conf) yang berfungsi sebagai reverse proxy dan load balancer untuk mendistribusikan lalu lintas jaringan masuk dari port 80 ke grup server backend (yaitu container aplikasi bs_app:80) dengan kapasitas dukungan koneksi pekerja hingga 4096 melalui blok pengaturan events dan http.   



* Pengaturan *orchestration* menggunakan `docker-compose.yml`

<img width="300" height="185" alt="image" src="https://github.com/user-attachments/assets/e491022e-db36-4957-923d-cd9d26b4548b" />
penjelasan:
Gambar tersebut menampilkan isi dari berkas konfigurasi docker-compose.yaml yang ditampilkan melalui terminal PowerShell, di mana file tersebut mendefinisikan tiga layanan utama yaitu bs_app, bs_app_2 (sebagai instance aplikasi BlackSheep yang akan di-load balancing), dan layanan nginx yang mengekspos port 8080:80 serta bergantung pada kedua layanan aplikasi tersebut


* **2.4 Menjalankan dan Verifikasi Docker Compose**
* Menjalankan *service daemon* dan perintah *scaling* container (`bs_app=2`)


* Verifikasi container yang berjalan menggunakan perintah `docker ps` (*dps*)


* Pengujian akses melalui *browser* pada `http://localhost` (Port 80)


* Analisis log container aplikasi (`docker logs`) untuk membuktikan distribusi *request* (*round-robin*) dari NGINX


* Perintah memberhentikan container (`docker-compose down` / *dcd*)





---

### **BAB III: FAILURE DETECTION (BAGIAN 4.2)**

* **3.1 Konsep Dasar Failure Detection**
* Penjelasan mengenai pentingnya deteksi kegagalan pada sistem terdistribusi.




* **3.2 Protokol Heartbeat (`check-server.py`)**
* Penjelasan cara kerja protokol *heartbeat*.


* Skenario pengujian saat server/port aktif (`Server up`) dan tidak aktif (`Server down`).




* **3.3 Pola Retry (`check-retry.py`)**
* Implementasi *retry* menggunakan library `tenacity` untuk menangani *transient failure*.


* Analisis perbandingan log saat aplikasi aktif dan non-aktif setelah beberapa kali percobaan ulang (*retries*).




* **3.4 Pola Circuit Breaker (`check-circuit-breaker.py`)**
* Penjelasan *state machine* pada *circuit breaker* (*Closed*, *Open*, *Half-Open*).


* Analisis pengujian skrip pada kondisi aplikasi aktif dan non-aktif serta mekanisme pencegahan *cascading failure*.





---

### **BAB IV: KESIMPULAN**

* 4.1 Kesimpulan dari praktikum *Load Balancing*


* 4.2 Kesimpulan dari praktikum *Failure Detection*



---
