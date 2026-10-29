### ** LAPORAN PRAKTIKUM SISTEM TERDISTRIBUSI DAN TERDESENTRALISASI**

**MODUL 5: FAULT TOLERANCE**

#### **I. JUDUL / TOPIK PRAKTIKUM**

* Modul 5: Fault Tolerance (Load Balancing dan Failure Detection)



#### **II. TUJUAN PRAKTIKUM**

1. Memahami konsep dan implementasi *Load Balancing* untuk mencapai *high-availability* pada aplikasi terdistribusi.


2. Melakukan *scaling* instance aplikasi web menggunakan Docker dan Docker Compose.


3. Mengonfigurasi proxy *load balancer* menggunakan Nginx.


4. Memahami dan menguji mekanisme *Failure Detection* (Heartbeat, Retry, dan Circuit Breaker).



#### **III. DASAR TEORI**

* **Fault Tolerance:** Kemampuan sistem untuk tetap berjalan dengan baik meskipun terdapat kegagalan pada salah satu komponennya.


* **Load Balancing:** Proses mendistribusikan lalu lintas jaringan (*traffic*) ke beberapa server atau instance aplikasi agar beban kerja terbagi merata.


* **Failure Detection & Circuit Breaker:** Mekanisme untuk mendeteksi kegagalan layanan (seperti protokol *heartbeat* dan pola *circuit breaker*) guna mencegah kegagalan berantai (*cascading failure*).



#### **IV. LANGKAH-LANGKAH PRAKTIKUM & HASIL (DOKUMENTASI FOTO)**

1. **Persiapan Lingkungan dan Instalasi BlackSheep**
* Instalasi Docker, Docker Compose, serta pengaturan *environment* Python (`uv` / Micromamba).


* Pembuatan proyek web menggunakan framework BlackSheep (`blacksheep create`) dan instalasi dependensi via `requirements.txt`.


* Pengujian awal server lokal di port `44777` menggunakan perintah `python dev.py`.


* * **Dokumentasi Foto yang Disertakan:**
* Tampilan browser saat mengakses halaman utama BlackSheep di `http://localhost:44777`.


* *Screenshot* terminal saat menjalankan server pengembangan lokal.




2. **Konfigurasi Dockerfile dan Docker Compose untuk Load Balancing**
* Penyiapan struktur direktori proyek, `Dockerfile` aplikasi, serta konfigurasi Nginx (`nginx.conf` dan `nginx/Dockerfile`).


* Penyusunan file `docker-compose.yml` untuk mendefinisikan layanan aplikasi (`bs_app`) dan proxy `nginx`.


* * **Dokumentasi Foto yang Disertakan:**
* Struktur direktori proyek praktikum (pohon direktori).


* Potongan kode konfigurasi `nginx.conf` dan `docker-compose.yml`.






3. **Menjalankan Docker Compose dan Pengujian Load Balancing**
* Menjalankan daemon Docker (`sudo systemctl start dockerd` / `dinitctl`).


* Menjalankan perintah Docker Compose untuk melakukan *scaling* instance aplikasi, misalnya `docker compose up --build -d --scale bs_app=2`.


* * **Dokumentasi Foto yang Disertakan:**
* Tampilan terminal saat proses *build* dan *startup* container Docker Compose (proses *Running*).


* Tampilan browser saat mengakses `http://localhost` (port 80) yang dilayani melalui *load balancer* Nginx.


* Hasil pengecekan container yang sedang berjalan menggunakan perintah `dps` (`docker ps`), menampilkan dua instance `bs_app` dan satu instance `nginx`.


* Tampilan log container (`docker logs`) yang menunjukkan pembagian beban permintaan (*request*) dari Nginx ke masing-masing instance aplikasi.






4. **Pengujian Failure Detection, Retry, dan Circuit Breaker**
* Pengujian *Heartbeat* / pengecekan status server (`check-server.py`) pada kondisi port aktif dan non-aktif.


* Pengujian mekanisme percobaan ulang (*retry*) menggunakan pustaka `tenacity` (`check-retry.py`).


* Pengujian pola *Circuit Breaker* (`check-circuit-breaker.py`) untuk menangani kegagalan koneksi layanan.


* * **Dokumentasi Foto yang Disertakan:**
* *Screenshot* terminal saat menjalankan skrip `check-server.py` dengan status "Server up" dan "Server down".


* *Screenshot* terminal saat eksekusi skrip `check-retry.py` (berhasil terhubung vs gagal setelah beberapa kali *retry*).


* *Screenshot* terminal saat pengujian skrip `check-circuit-breaker.py` pada kondisi aplikasi aktif dan non-aktif.







#### **V. KESIMPULAN**

* Berisi ringkasan hasil praktikum, di mana arsitektur *load balancing* menggunakan Nginx dan Docker Compose berhasil mendistribusikan trafik ke beberapa instance aplikasi BlackSheep, serta mekanisme *failure detection* terbukti mampu mendeteksi serta menangani kegagalan layanan secara handal.

---

Apakah ada bagian spesifik dari modul atau kode program tertentu yang ingin saya bantu jelaskan lebih mendalam untuk laporan Anda?
