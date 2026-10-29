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
<img width="197" height="124" alt="Screenshot 2026-10-29 121816" src="https://github.com/user-attachments/assets/4684513c-10d3-4512-8982-592d4f1b6547" />


penjelasan:

3.uji manipulasi data 
<img width="536" height="161" alt="Screenshot 2026-10-29 121825" src="https://github.com/user-attachments/assets/3c321bfa-73d1-4b11-aee0-2e49d490908b" />

penjelasan:

##2.5 Simulasi High-Availability
1.matikan server primary
<img width="301" height="86" alt="Screenshot 2026-10-29 121956" src="https://github.com/user-attachments/assets/7440c1bf-dedd-45b9-84b8-bbc46cb5be0b" />

Penjelasan:

2.Promosi standby menjadi primary baru
<img width="326" height="287" alt="Screenshot 2026-10-29 122002" src="https://github.com/user-attachments/assets/5bd4efca-227b-4434-b7f2-e3bd1b8a6789" />

penjelasan:

3.Bagian II replikasi master master menggunakan apache ignite 3.1.0

3.1 konfigurasi lingkungan cluster
<img width="335" height="131" alt="Screenshot 2026-10-29 122112" src="https://github.com/user-attachments/assets/069467a9-7c62-48c6-94b3-3a4474f6d8bc" />

penjelasan:
Gambar di atas menampilkan eksekusi serangkaian perintah SQL pada database PostgreSQL untuk melakukan proses failover atau promosi server replika. Melalui kueri SELECT pg_is_in_recovery();, awalnya diketahui bahwa database berstatus sebagai replika (t / true), lalu perintah SELECT pg_promote(); dijalankan untuk mengubah statusnya menjadi server utama, yang kemudian dikonfirmasi kembali dengan kueri pg_is_in_recovery() yang menghasilkan nilai f (false), menandakan database kini telah resmi aktif sebagai primary yang mandiri.

3.2 inisialisasi cluster dan inspeksi jaringan 
<img width="383" height="64" alt="Screenshot 2026-10-29 122206" src="https://github.com/user-attachments/assets/e6d64a68-09e2-4f60-8aac-90ef55dfac78" />

penjelasan:
Gambar di atas menampilkan eksekusi perintah alias dcu (singkatan umum untuk docker compose up -d) di terminal. Perintah tersebut berhasil membuat jaringan Docker kustom bernama ignite3_default beserta tiga container klaster Apache Ignite 3, yaitu ignite3-node1-1, ignite3-node2-1, dan ignite3-node3-1, yang kini aktif berjalan di latar belakang

1.pemetaan alamat ip node internal via docker inspect
<img width="602" height="151" alt="Screenshot 2026-10-29 122213" src="https://github.com/user-attachments/assets/5e3f7dd3-5446-4ed7-b623-638f91fc714d" />

penjelasan:
Gambar di atas menampilkan perintah terminal Linux yang menggunakan sudo docker inspect untuk mengambil alamat IP dari tiga container Docker yang berbeda (masing-masing bernama ignite3-node...), dengan hasil keluaran berupa IP 172.18.0.2, 172.18.0.3, dan 172.18.0.4

2.inisialisasi kluster melalui apache ignite cli
<img width="283" height="157" alt="image" src="https://github.com/user-attachments/assets/aad43096-0cca-48c6-9a92-161f45a4d196" />

penjelasan:
Gambar di atas menampilkan eksekusi serangkaian perintah baris perintah pada node klaster Apache Ignite ([node1]) untuk menjalankan file skrip SQL eksternal. Perintah pertama menjalankan schema.sql untuk menyiapkan struktur tabel (dengan hasil Updated 0 rows), diikuti oleh eksekusi file tracks.sql yang memperbarui data sebanyak 1000 dan 503 baris, serta file invoices.sql yang memasukkan atau memperbarui data faktur sebanyak 1000 dan 715 baris.

##kesimpulan##
Praktikum berhasil mengkonfigurasi PostgreSQL 18 Streaming Replication, membuktikan aliran sinkronisasi data instan dari node Primary ke Standby/Replica.

Skenario High-Availability pada PostgreSQL teruji dengan sukses lewat pengalihan fungsi replica menjadi primary menggunakan pg_promote().

Praktikum berhasil mengkonfigurasi kluster Apache Ignite 3.1.0 Multi-Primary, membuktikan bahwa eksekusi transaksi pada satu node secara otomatis terdistribusi dan terkueri dari node lainnya.
