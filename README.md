# Kerangka Laporan Praktikum

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

Penjelasan konfigurasi replikasi *Single-Primary* (Master-Slave/Standby).

### 2.1 Prasyarat dan Lingkungan Kerja

* **Persiapan Sistem:** Pengecekan instalasi Docker Engine, Docker Compose, dan pengaktifan *daemon* `dockerd` (misal menggunakan `sudo dinitctl start dockerd`).


* **Struktur Direktori & File:**


* `00_init.sql`: Perintah SQL awal untuk membuat user replikasi (`replicator`) dan *replication slot* (`replication_slot`).


* `docker-compose.yaml`: Menentukan konfigurasi dua container (`postgres_primary` dan `postgres_replica`).


* `env.sh`: Script definisi *alias* perintah Docker untuk mempermudah eksekusi terminal.





### 2.2 Menjalankan dan Menguji Container

* **Pengaktifan Service:** Menjalankan perintah `source env.sh` dilanjutkan `dcu` (`docker-compose up -d`).


* **Verifikasi Status Container:** Memeriksa kesehatan container dengan `dps` (`docker ps`) dan memastikan status bernilai `healthy`.


* **Analisis Log Server Standby:** Memeriksa log replica via `dlr` untuk melihat proses inisialisasi awal dengan `pg_basebackup` dan dimulainya *streaming WAL*.



### 2.3 Pengujian Replikasi Data

* **Pemeriksaan Status Replikasi pada Primary:**
* Eksekusi query `SELECT * FROM pg_stat_replication;` untuk memastikan status `streaming`.


* Query `SELECT * FROM pg_replication_slots;` untuk memastikan slot replikasi aktif.




* **Pemeriksaan Status Mode Standby pada Replica:**
* Exec ke container replica (`der`) lalu jalankan `SELECT pg_is_in_recovery();` (harus menghasilkan `t` / *true*).




* **Uji Sinkronisasi Data (Read/Write Test):**
* Pembuatan tabel `testrep` dan manipulasi data (`INSERT INTO`) pada server *Primary*.


* Verifikasi bahwa data yang diinput otomatis tereplikasi dan dapat dibaca di server *Replica*.





### 2.4 Pengujian High-Availability (Failover)

* **Simulasi Failure:** Mematikan *Primary server* menggunakan perintah `sudo docker-compose stop postgres_primary`.


* **Promosi Standby menjadi Primary:** Menjalankan fungsi `SELECT pg_promote();` pada server *Replica*.


* **Verifikasi Promosi:** Memeriksa `SELECT pg_is_in_recovery();` yang kini menghasilkan nilai `f` (*false*), menandakan server telah menjadi *Primary* baru yang siap menerima akses *read-write*.


* **Pembersihan Service:** Mematikan dan menghapus seluruh container dengan perintah `dcd` (`docker-compose down`).



---

## 3. Bagian II: Replikasi Master-Master Menggunakan Apache Ignite 3.1.0

Penjelasan konfigurasi kluster terdistribusi multi-node di mana semua node bertindak sebagai *Primary*.

### 3.1 Struktur Direktori dan Konfigurasi

* **Persiapan Lingkungan:** Penyiapan `docker-compose.yaml` untuk 3 node Ignite (`node1`, `node2`, `node3`) serta pengunduhan skrip-skrip SQL.


* **Penggunaan Environment Variables / Alias:** Konfigurasi `dcli` untuk CLI interaktif dan `dclm` untuk CLI dengan mounting folder `/opt/ignite/sql`.



### 3.2 Menjalankan Kluster dan Inisialisasi

* **Menjalankan Container:** Mengaktifkan kluster dengan `dcu`.


* **Inspeksi Network:** Mengetahui IP internal masing-masing node menggunakan `docker inspect` dan pengujian konektivitas via `ping`.


* **Inisialisasi Cluster (CLI):** Menjalankan perintah `cluster init --name ignite3 --metastorage-group=node1,node2,node3` melalui Ignite CLI di `http://localhost:10300`.


* **Pemeriksaan Status Kluster:** Memastikan status bernilai `active` pada `cluster status`.



### 3.3 Eksekusi SQL dan Verifikasi Replikasi Multi-Node

* **Load Schema & Data:**
* Menjalankan file SQL (`schema.sql`, `tracks.sql`, `invoices.sql`, dll.) dari `node1` (`http://localhost:10300`).




* **Verifikasi Replikasi Lintas Node:**
* Pindah koneksi ke `node2` (`http://localhost:10301`) dan `node3` (`http://localhost:10302`).


* Menjalankan query pendaftaran tabel (`SELECT * FROM system.tables WHERE schema = 'PUBLIC';`) dan query analisis data (*Monthly Sales Summary*) pada `node2` dan `node3`.


* Membuktikan bahwa seluruh skema dan data terduplikasi dan tersinkronisasi sempurna secara transparan.




* **Pembersihan Service:** Menghentikan seluruh kluster dengan `dcd`.



---

## 4. Analisis dan Pembahasan

* **Perbandingan Model Replikasi:**
* **PostgreSQL Streaming Replication (Single-Primary):** Menggunakan skema *Active-Passive* / *Primary-Standby*. Server utama menangani *write*, sedangkan standby melayani *read-only*. Membutuhkan mekanisme failover (`pg_promote`) saat primary down.


* **Apache Ignite (Master-Master / Multi-Primary):** Setiap node dalam cluster memiliki kapabilitas yang seimbang (dapat melayani *read* dan *write*). Sinkronisasi data dilakukan secara otomatis ke seluruh node metastorage/cluster.




* **Toleransi Kesalahan & Konsistensi Data:** Penjelasan singkat bagaimana Write-Ahead Logging (WAL) pada PostgreSQL dan protokol konsistensi pada Apache Ignite menjamin integritas data terdistribusi.



---

## 5. Kesimpulan

* Ringkasan dari hasil praktikum yang telah dilaksanakan.
* Penjelasan singkat mengenai situasi kapan harus memilih model *Single-Primary* (misal: RDBMS konvensional dengan pembacaan tinggi) vs *Master-Master* (misal: sistem *in-memory/distributed computing* berskala tinggi).



---

## 6. Referensi

* Purnomosidi D. P., Bambang. *Modul 4 Praktikum Sistem Terdistribusi dan Terdesentralisasi: Konsistensi dan Replikasi pada Sistem Terdistribusi*. Universitas Teknologi Digital Indonesia.


* Dokumentasi Resmi Docker Compose, PostgreSQL Streaming Replication, dan Apache Ignite 3 Manual.



---

### 💡 Tips Penyusunan Laporan di GitHub:

1. Simpan kerangka di atas dalam file **`README.md`** di repositori GitHub Anda.


2. Sertakan **tangkapan layar (screenshot)** pada bagian-bagian penting seperti:
* Output `docker ps` (*status healthy*).


* Query `pg_stat_replication` dan `pg_is_in_recovery()`.


* Hasil eksekusi `CREATE/INSERT` pada Primary dan `SELECT` pada Replica.


* Output `cluster status` dan hasil query analytical pada Apache Ignite.
