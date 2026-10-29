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
   
2. Berkas docker-compose.yaml

3.Berkas env.sh


