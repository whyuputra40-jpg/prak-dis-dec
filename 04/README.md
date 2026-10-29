# Laporan Praktikum

**Mata Kuliah:** Praktikum Sistem Terdistribusi dan Terdesentralisasi

**Modul:** Modul 4 - Konsistensi dan Replikasi pada Sistem Terdistribusi

**Nama / NIM:** [Avatar DAva Wahyuputra / 255410008]

**Program Studi / Instansi:** Informatika, Universitas Teknologi Digital Indonesia

---

## 1. Pendahuluan

* **1.1 Latar Belakang**
Penjelasan singkat mengenai pentingnya **replikasi dan konsistensi data** dalam sistem terdistribusi untuk menjamin ketersediaan data (*high availability*), toleransi kesalahan (*fault tolerance*), serta pembagian beban kerja (*load balancing*).
* **1.2 Tujuan Praktikum**
1. Memahami dan mengkonfigurasi *Streaming Replication* (*Single-Primary*) pada PostgreSQL 18 menggunakan Docker Compose.


2. Memahami mekanisme *High Availability* (HA) dan failover menggunakan `pg_promote()`.


3. Memahami dan mengkonfigurasi replikasi *Master-Master* / *Multi-Primary Cluster* menggunakan Apache Ignite 3.1.0.





---

## 2. Bagian I: Streaming Replication Menggunakan PostgreSQL 18



### 2.1 Prasyarat dan Lingkungan Kerja

<img width="340" height="107" alt="Screenshot 2026-10-29 110935" src="https://github.com/user-attachments/assets/a4097194-c887-4c72-89a9-2983537a94d8" />

Sebelum menjalankan service, dipastikan daemon Docker (dockerd) telah aktif di sistem operasi Linux.

1. Berkas 00_init.sql
   <img width="310" height="46" alt="Screenshot 2026-10-29 110308" src="https://github.com/user-attachments/assets/ed247d5c-efe4-4f25-9306-833cf02eb332" />
penjelasan:
berisi perintah SQL yang dijalankan saat inisialisasi database PostgreSQL. File ini biasanya digunakan untuk membuat tabel, menentukan struktur kolom, dan menyiapkan data awal yang diperlukan dalam praktikum. Pada konfigurasi Docker Compose kamu, file tersebut dipasang ke direktori /docker-entrypoint-initdb.d/00_init.sql agar PostgreSQL dapat menjalankannya saat database pertama kali dibuat.

2. Berkas docker-compose.yaml
   
<img width="319" height="450" alt="Screenshot 2026-10-29 111942" src="https://github.com/user-attachments/assets/19743d0f-2fd4-448d-a968-14456118aa33" />
penjelasan:
File docker-compose.yaml digunakan untuk mengatur dua container PostgreSQL, yaitu postgres_primary sebagai server utama dan postgres_replica sebagai server replika. File ini menentukan image PostgreSQL, username dan password database, port koneksi, serta konfigurasi replikasi melalui pengaturan WAL dan pg_basebackup. Selain itu, terdapat healthcheck untuk memeriksa kesiapan database dan depends_on agar replica bergantung pada primary. Dengan konfigurasi tersebut, Docker Compose membantu menjalankan kedua server PostgreSQL dalam satu jaringan sehingga dapat digunakan untuk praktikum replikasi database.

3.Berkas env.sh

<img width="241" height="82" alt="Screenshot 2026-10-29 111947" src="https://github.com/user-attachments/assets/9866314c-f1e7-4a66-9d1b-0f9827e16a4e" />
penjelasan:
File env.sh digunakan untuk menyimpan konfigurasi lingkungan dan mempermudah pengguna menjalankan perintah praktikum melalui terminal Linux. File ini dapat berisi variabel lingkungan, alias, atau fungsi seperti dcu yang digunakan untuk menjalankan Docker Compose dengan lebih praktis. Perintah source env.sh berfungsi memuat konfigurasi tersebut ke terminal yang sedang digunakan, sehingga perintah yang didefinisikan di dalamnya dapat langsung dipakai. Dengan adanya file env.sh, proses menjalankan container menjadi lebih sederhana tanpa harus mengetik perintah Docker Compose yang panjang berulang kali.

## 2.3 Menjalasankan docker compose da analisis log
<img width="381" height="127" alt="Screenshot 2026-10-29 112238" src="https://github.com/user-attachments/assets/3c64dc54-bd89-45cc-8f9e-81a2eab77221" />
penjelasan:
Proses pada gambar menunjukkan inisialisasi dan pengaktifan arsitektur streaming replication PostgreSQL menggunakan Docker Compose pada proyek src. Perintah docker compose -p src up -d berhasil membuat jaringan Docker kustom beserta dua container, yaitu src-postgres_primary-1 dan src-postgres_replica-1, yang kemudian dikonfirmasi melalui perintah docker compose -p src ps bahwa keduanya telah aktif berjalan di latar belakang dalam kondisi sehat (healthy).

##2.4 Pengujian Replikasi Data dan singkronisasi
1.pemeriksaan status diagnostik pada server primary
<img width="287" height="247" alt="image" src="https://github.com/user-attachments/assets/317759ea-3dbb-4501-8c5a-5512c2beb85c" />
penjelasan:
SELECT * FROM pg_stat_replication; pada database PostgreSQL dengan mode tampilan diperluas (\x), yang menunjukkan bahwa server utama sedang aktif melakukan streaming replication ke server replika dengan status streaming serta nilai LSN (Log Sequence Number) yang sudah sinkron sempurna

2.Pemeriksaan status pada server replika

penjelasan:

3.uji manipulasi data 

##2.5 Simulasi High-Availability
1.matikan server primary

Penjelasan:

2.Promosi standby menjadi primary baru

penjelasan;

3.Bagian II replikasi master master menggunakan apache ignite 3.1.0

3.1 konfigurasi lingkungan cluster

penjelasan:

3.2 inisialisasi cluster dan inspeksi jaringan 

1.pemetaan alamat ip node internal via docker inspect

penjelasan:

2.inisialisasi kluster melalui apache ignite cli

penjelasan:
