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
<img width="312" height="458" alt="image" src="https://github.com/user-attachments/assets/0bd134a9-048f-4c47-b16c-dadc3d70e874" />
<img width="466" height="158" alt="image" src="https://github.com/user-attachments/assets/d30477f2-9f85-4a58-9a8c-988bd3ae3f68" />
penjelasan:
Kedua gambar di atas menunjukkan proses manajemen kontainer dan verifikasi sistem menggunakan Docker Compose, di mana gambar pertama memperlihatkan daftar kontainer yang sedang berjalan beserta status dan pemetaan portnya, sedangkan gambar kedua menampilkan proses pemutusan atau penghentian seluruh layanan kontainer dan pembersihan jaringan virtual secara otomatis melalui perintah docker-compose down (dcd).


* Pengujian akses melalui *browser* pada `http://localhost` (Port 80)
<img width="461" height="476" alt="image" src="https://github.com/user-attachments/assets/1a8ee4dd-51e5-44df-928b-e801d84bd1a7" />
penjelasan:
Gambar tersebut menampilkan tangkapan layar dari peramban web yang mengakses halaman utama aplikasi BlackSheep melalui localhost pada port 80, yang membuktikan bahwa trafik berhasil dirutekan melalui NGINX load balancer tanpa perlu menyebutkan port khusus seperti sebelumnya

* Analisis log container aplikasi (`docker logs`) untuk membuktikan distribusi *request* (*round-robin*) dari NGINX
<img width="344" height="233" alt="image" src="https://github.com/user-attachments/assets/6d105e0c-50bb-4926-aa5f-15568dd1be4a" />
penjelasan:
Gambar tersebut menunjukkan proses pengujian load balancing di terminal PowerShell dengan mengirimkan beberapa permintaan HTTP menggunakan perintah perulangan curl ke http://localhost, diikuti dengan pengecekan log dari kedua instance kontainer aplikasi (workspace-05-bs_app-1 dan workspace-05-bs_app-2), yang membuktikan bahwa NGINX berhasil mendistribusikan trafik permintaan secara bergantian ke kedua instance kontainer tersebut.

* Perintah memberhentikan container (`docker-compose down` / *dcd*)
<img width="471" height="131" alt="image" src="https://github.com/user-attachments/assets/6462e063-7870-433b-9c69-7a488d98537c" />
penjelasan:
Gambar tersebut menunjukkan proses di terminal PowerShell saat pengguna menjalankan perintah docker compose down untuk menghentikan dan menghapus seluruh kontainer terkait (nginx, workspace-05-bs_app-2, dan workspace-05-bs_app-1) beserta jaringan virtualnya, yang kemudian dilanjutkan dengan menjalankan perintah docker ps untuk menampilkan daftar kontainer Docker aktif lainnya yang sedang berjalan di sistem.


### **BAB III: FAILURE DETECTION (BAGIAN 4.2)**

* **3.1 Konsep Dasar Failure Detection**
* Penjelasan mengenai pentingnya deteksi kegagalan pada sistem terdistribusi.
Failure Detection adalah proses untuk mengetahui apakah suatu komponen dalam sistem terdistribusi mengalami kegagalan atau tidak.

Dalam sistem terdistribusi, aplikasi biasanya bergantung pada beberapa komponen yang saling berkomunikasi. Misalnya, aplikasi web membutuhkan server lain untuk memproses permintaan. Jika salah satu komponen berhenti bekerja, komunikasi dapat terganggu.

Tujuan Failure Detection adalah:
1. Mengetahui ketika suatu server atau layanan tidak merespons.
2. Membantu sistem mengenali komponen yang mengalami gangguan.
3. Mendukung penanganan kegagalan agar tidak mengganggu seluruh sistem.
4. Menjaga layanan tetap dapat digunakan ketika terjadi gangguan.
Contoh sederhana: aplikasi BlackSheep berjalan pada port 44777. Jika aplikasi aktif, sistem dapat terhubung. Jika aplikasi berhenti, sistem akan mendeteksi bahwa server tidak merespons.



* **3.2 Protokol Heartbeat (`check-server.py`)**
<img width="409" height="59" alt="image" src="https://github.com/user-attachments/assets/910adbf5-907f-4786-bb2f-a98f4de6f396" />
penjelasan:

Gambar tersebut menunjukkan proses di terminal PowerShell saat pengguna menjalankan skrip pengembangan lokal dev.py menggunakan interpreter Python dari virtual environment (C:\Users\X1 Yoga\.venv\Scripts\python.exe), yang berhasil memulai server ASGI Uvicorn pada port lokal 44777 dan siap diakses untuk pengujian aplikasi web BlackSheep.

* Skenario pengujian saat server/port aktif (`Server up`) dan tidak aktif (`Server down`).
<img width="463" height="113" alt="image" src="https://github.com/user-attachments/assets/6ecdc615-603e-4ecd-beaf-430acccc9d0a" />
penejelasan:
Gambar tersebut menunjukkan proses pengujian protokol heartbeat di terminal PowerShell, di mana pengguna berpindah ke direktori heartbeat dan menjalankan skrip pemantauan check-server.py untuk memeriksa status server lokal pada port 44777, yang kemudian menghasilkan keluaran Server up sebagai tanda bahwa server aplikasi sedang aktif dan merespons dengan baik



* **3.3 Pola Retry (`check-retry.py`)**
* Implementasi *retry* menggunakan library `tenacity` untuk menangani *transient failure*.
<img width="478" height="539" alt="image" src="https://github.com/user-attachments/assets/8dd2f9f2-a894-41e0-822a-4777a715ee2b" />
penjelasan;
Gambar di atas menunjukkan proses navigasi direktori kerja dan eksekusi skrip Python menggunakan Windows PowerShell. Pada jendela bagian atas, pengguna berpindah ke folder proyek failure-detection, menjalankan skrip check-retry.py menggunakan interpreter Python virtual environment (.venv), dan berhasil mengakses alamat http://localhost:44777. Sementara itu, jendela bagian bawah menampilkan proses navigasi ke folder proyek lain (blacksheep_lb), eksekusi skrip dev.py yang menjalankan server lokal menggunakan Uvicorn pada port 44777, serta log akses HTTP dengan status 200 OK yang menandakan bahwa aplikasi berhasil berjalan dengan baik.
<img width="471" height="78" alt="image" src="https://github.com/user-attachments/assets/4630fd88-d2b8-42e0-8e29-c59a8dc65fbf" />
penjelasan:
Gambar di atas menunjukkan pesan galat (error) di Windows PowerShell ketika mencoba menjalankan skrip check-retry.py di dalam direktori C:\Users\X1 Yoga\kerjaan\utdi\workspace-05\blacksheep_lb. Pesan tersebut menampilkan [Errno 2] No such file or directory, yang berarti file check-retry.py tidak ditemukan di dalam folder tersebut karena skrip tersebut sebelumnya berada di direktori failure-detection
perbandingan:
Gambar pertama menunjukkan proses yang berhasil ketika kita berpindah direktori proyek dan menjalankan skrip Python yang sesuai, seperti check-retry.py di folder failure-detection serta dev.py di folder blacksheep_lb dengan server Uvicorn yang berjalan normal. Sebaliknya, gambar kedua memperlihatkan terjadinya galat (error) [Errno 2] No such file or directory karena Anda mencoba menjalankan skrip check-retry.py di dalam direktori blacksheep_lb, padahal file skrip tersebut sebenarnya berada di folder failure-detection.   




* **3.4 Pola Circuit Breaker (`check-circuit-breaker.py`)**
<img width="473" height="380" alt="image" src="https://github.com/user-attachments/assets/606cf12c-80e9-4ac1-a7ac-79e879914887" />
penjelasan:
Circuit Breaker digunakan untuk mencegah aplikasi terus melakukan permintaan kepada server yang sedang mengalami gangguan. Ketika server aktif, permintaan dapat diproses dengan normal. Namun, ketika server tidak aktif dan terjadi tiga kegagalan berturut-turut, Circuit Breaker akan masuk ke kondisi OPEN sehingga permintaan berikutnya ditolak sementara.




---

### **BAB IV: KESIMPULAN**

* 4.1 Kesimpulan dari praktikum *Load Balancing*
Berdasarkan praktikum Load Balancing yang telah dilakukan, dapat disimpulkan bahwa Load Balancing merupakan teknik untuk membagi beban permintaan dari pengguna ke beberapa server agar tidak hanya bergantung pada satu server saja. Pada praktikum ini, Docker digunakan untuk menjalankan beberapa container aplikasi BlackSheep, sedangkan Nginx digunakan sebagai pengatur dan pembagi permintaan ke server yang tersedia. Dengan penerapan Load Balancing, beban kerja dapat dibagi sehingga pelayanan aplikasi menjadi lebih terdistribusi dan penggunaan sumber daya server menjadi lebih optimal.

* 4.2 Kesimpulan dari praktikum *Failure Detection*
  Berdasarkan praktikum Failure Detection yang telah dilakukan, dapat disimpulkan bahwa Failure Detection merupakan mekanisme untuk mengetahui apakah suatu komponen dalam sistem terdistribusi masih berjalan atau mengalami kegagalan. Pada praktikum ini, terdapat tiga metode yang dipelajari, yaitu Heartbeat, Retry, dan Circuit Breaker. Heartbeat digunakan untuk memeriksa apakah server masih memberikan respons, Retry digunakan untuk mencoba kembali ketika terjadi kegagalan koneksi, sedangkan Circuit Breaker digunakan untuk menghentikan sementara permintaan ketika server mengalami gangguan. Melalui praktikum ini, dapat dipahami bahwa ketiga metode tersebut membantu sistem mengetahui dan menangani gangguan agar aplikasi tetap dapat berjalan dengan lebih baik.



---
