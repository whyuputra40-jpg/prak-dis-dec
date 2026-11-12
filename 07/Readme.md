Berikut adalah kerangka laporan praktikum yang disesuaikan dengan struktur tugas dan materi pada **Modul 7: Cloud Computing**:

---

### **KERANGKA LAPORAN PRAKTIKUM**

**Mata Kuliah:** Praktikum Sistem Terdistribusi dan Terdesentralisasi

**Modul 7:** Cloud Computing (XaaS & Containerized App)

#### **BAB I: PENDAHULUAN**

* **1.1 Latar Belakang**
* Pengantar konsep *Cloud Computing* dengan pendekatan XaaS (*Everything as a Service*).


* Peran *Containerized App* (CA) dalam mendukung layanan komputasi awan modern.




* **1.2 Tujuan Praktikum**
* Memahami kategori layanan XaaS (SaaS, PaaS, IaaS) beserta arsitekturnya.


* Mampu membangun aplikasi web berbasis Python (Flask) menjadi *Containerized App* menggunakan Docker dan Podman.





#### **BAB II: DASAR TEORI & TUGAS PENDAHULUAN (TUGAS 0)**

* **2.1 Konsep Dasar Cloud Computing (XaaS)**


*Berikut adalah masing-masing satu contoh vendor dari tiga kategori utama layanan Cloud Computing (XaaS):

SaaS (Software as a Service): Google (melalui layanan Google Workspace).

PaaS (Platform as a Service): Salesforce (melalui layanan Heroku).

IaaS (Infrastructure as a Service): Amazon (melalui layanan Amazon Web Services / AWS EC2).



* **2.2 Studi Kasus Vendor/Komunitas XaaS (Jawaban Tugas 0)**

* **Uraian Layanan (*Service*)** dari masing-masing vendor tersebut berdasarkan kategorinya.


Google Workspace (Kategori SaaS):

  Bentuk Layanan: Menyediakan perangkat lunak produktivitas dan kolaborasi berbasis cloud yang siap pakai, seperti Gmail (email), Google Drive (penyimpanan awan), Google Docs, Sheets, dan Google Meet.

  Karakteristik: Pengguna akhir (end-user) dapat langsung mengakses dan menggunakan aplikasi melalui web browser atau aplikasi mobile tanpa perlu memikirkan instalasi perangkat lunak, pemeliharaan server, pembaruan sistem, maupun pengelolaan infrastruktur di belakangnya. Semua dikelola sepenuhnya oleh pihak Google.

Heroku (Kategori PaaS):

  Bentuk Layanan: Menyediakan platform berbasis cloud bagi pengembang (developer) untuk membangun, menjalankan, dan mengelola aplikasi (mendukung berbagai bahasa pemrograman seperti Python, Node.js, Java, PHP, dll.) secara langsung.

  Karakteristik: Developer hanya perlu fokus pada penulisan source code aplikasi dan pengelolaan data. Heroku secara otomatis menangani penyediaan server virtual, sistem operasi, runtime environment, load balancing, hingga scaling aplikasi tanpa mengharuskan pengguna mengonfigurasi infrastruktur dasar secara manual.

Amazon Web Services / AWS EC2 (Kategori IaaS):

  Bentuk Layanan: Menyediakan infrastruktur komputasi dasar secara virtual melalui internet, khususnya Amazon Elastic Compute Cloud (EC2) yang berupa mesin virtual (Virtual Machine / instance), ruang penyimpanan (storage), dan jaringan (networking).

  Karakteristik: Pengguna memiliki kendali penuh atas infrastruktur virtual yang disewa, mulai dari memilih spesifikasi CPU/RAM, menginstal sistem operasi (Linux/Windows), mengatur keamanan jaringan (firewall), hingga menginstal runtime dan aplikasi sendiri. Penyedia layanan (AWS) hanya bertanggung jawab menjaga ketersediaan fisik data center, perangkat keras server, jaringan fisik, dan lapisan virtualisasi.




* **Arsitektur Umum XaaS** (disertai visualisasi gambar/diagram perbandingan lapisan yang dikelola pengguna vs *provider*).

<img width="360" height="188" alt="image" src="https://github.com/user-attachments/assets/826dcecd-e8b0-45f9-8836-df8c2c895565" />

penjelasan:
Pada IaaS: Provider mengelola lapisan fisik hingga virtualisasi (Networking, Storage, Servers, Virtualization), sementara konsumen mengelola sistem operasi hingga aplikasi di atasnya.

Pada PaaS: Provider mengambil alih pengelolaan sistem operasi, middleware, dan runtime, sehingga konsumen hanya mengelola kode aplikasi dan datanya.

Pada SaaS: Seluruh lapisan dari infrastruktur bawah hingga aplikasi dan penyimpanan data dikelola sepenuhnya oleh provider, dan konsumen tinggal menggunakannya.



* **2.3 Konsep Containerization (Docker & Podman)**
* Pengertian *Containerized App* dan perbedaan arsitektur dasar antara Docker (berbasis *daemon* `dockerd`) dan Podman (*daemonless*).





#### **BAB III: PELAKSANAAN PRAKTIKUM & PEMBAHASAN**

* **3.1 Persiapan Lingkungan Kerja & Aplikasi Flask**



* *Cloning repository* Flask dan menyalin direktori `examples/tutorial` ke `flask-app`.
<img width="452" height="139" alt="image" src="https://github.com/user-attachments/assets/7cbe40be-1454-46c7-bda1-8d4354120dcc" />

* Pembuatan *virtual environment* Python 3.14.4 menggunakan `uv`.
<img width="240" height="63" alt="image" src="https://github.com/user-attachments/assets/2b97e3b3-aed6-4b23-8464-ad910ace0476" />


* Instalasi paket dependensi (`uv pip install -e .`), inisialisasi database (`init-db`), dan pengujian lokal melalui browser (`http://localhost:5000`).
<img width="365" height="127" alt="image" src="https://github.com/user-attachments/assets/58e32508-1abe-470f-8451-40ea157f92aa" />

<img width="467" height="196" alt="image" src="https://github.com/user-attachments/assets/f52a5f9c-adb2-4a73-921e-85a2cbdef609" />




* **3.2 Pembuatan Containerized App dengan Docker**


* Pembuatan dan penjelasan isi `Dockerfile` (`FROM`, `WORKDIR`, `ADD`, `RUN`, `EXPOSE`, `CMD`).
<img width="266" height="70" alt="image" src="https://github.com/user-attachments/assets/bffd1308-0bce-493d-b944-7cabe0bd5360" />


* Aktivasi *service* Docker (`dockerd`) dan proses *build image* (`docker build -t flaskr:1.0.0 .`).
<img width="466" height="227" alt="image" src="https://github.com/user-attachments/assets/c13f39d3-00ba-450e-89c5-e4b20b334d80" />


* Pengecekan *image* (`docker image ls`) dan manajemen *image* (`docker rmi`).
<img width="249" height="57" alt="image" src="https://github.com/user-attachments/assets/b79f648d-76af-48d8-b373-a7f41c5c3f39" />


* Menjalankan *container* dengan *port mapping* `5001:5000` dan pengujian di `http://localhost:5001`.
<img width="468" height="101" alt="image" src="https://github.com/user-attachments/assets/00476fc8-5fdf-4748-8b0f-de8910ed6efa" />

<img width="470" height="204" alt="image" src="https://github.com/user-attachments/assets/465a3f57-ecec-417a-8e7d-694a5d47451a" />


* **3.3 Pembuatan Containerized App dengan Podman**


* Menghentikan *service* `dockerd` dan memanfaatkan `Dockerfile` yang sama untuk Podman.


* Proses *build image* menggunakan `podman build -f Dockerfile -t flaskr:1.0.0 .`.


* Pengecekan daftar *image* (`podman image ls`).


* Menjalankan *container* dengan `podman run -p 5001:5000 flaskr:1.0.0` dan verifikasi akses di `[http://127.0.0.1:5001](http://127.0.0.1:5001)`.


* *Sertakan screenshot output build Podman, daftar image, dan hasil pengujian browser.*


* **3.4 Analisis Perbandingan**
* Analisis perbandingan proses *build*, ukuran *image*, dan cara eksekusi *container* antara Docker dan Podman berdasarkan hasil praktikum.





#### **BAB IV: PENUTUP**

* **4.1 Kesimpulan**
* Rangkuman hasil pemahaman konsep XaaS dan keberhasilan implementasi *Containerized App* pada dua *container engine* berbeda.





#### **DAFTAR PUSTAKA**

* Referensi modul praktikum, dokumentasi Flask, dokumentasi `uv`, serta referensi sumber gambar arsitektur XaaS.



---

Apakah ada bagian tertentu (misalnya isi **Tugas 0** atau penjelasan perintah **Dockerfile**) yang perlu dijabarkan lebih detail untuk membantu penyusunan isinya?
