# LAPORAN PRAKTIKUM MODUL 3
# SINKRONISASI PADA SISTEM TERDISTRIBUSI DAN TERDESENTRALISASI

**Disusun untuk memenuhi tugas Praktikum Sistem Terdistribusi dan Terdesentralisasi**

**Program Studi Informatika**  
**Universitas Teknologi Digital Indonesia**  


Disusun oleh:

Nama: Avatar Dava Wahyuputra 
NIM: 255410008

Tahun Akademik 2026/2027

---

# BAB I PENDAHULUAN

## 1.1 Latar Belakang

Sistem terdistribusi merupakan sistem yang terdiri dari beberapa komputer atau proses yang saling berkomunikasi untuk menyelesaikan suatu pekerjaan. Dalam sistem tersebut, setiap komputer dapat menjalankan proses secara bersamaan sehingga diperlukan mekanisme sinkronisasi agar proses dan pertukaran data dapat berjalan dengan baik.

Sinkronisasi merupakan salah satu hal penting dalam sistem terdistribusi. Tanpa sinkronisasi yang tepat, sistem dapat mengalami perbedaan waktu, kesalahan pengaksesan data, *race condition*, dan *deadlock*. Selain itu, sistem terdistribusi juga membutuhkan mekanisme untuk menentukan pemimpin atau *leader* agar koordinasi antarproses dapat berjalan dengan baik.

Pada praktikum Modul 3 ini, dilakukan pembelajaran mengenai sinkronisasi waktu menggunakan NTP, pengurutan kejadian menggunakan Vector Clock, masalah yang terjadi tanpa sinkronisasi, serta algoritma Raft untuk memilih *leader*. Praktikum dilakukan menggunakan perangkat lunak yang sesuai dengan sistem operasi dan program yang tersedia.

## 1.2 Tujuan Praktikum

Tujuan praktikum ini adalah:

1. Memahami proses sinkronisasi waktu menggunakan NTP.
2. Memahami cara kerja dan penerapan algoritma Vector Clock.
3. Mengetahui penyebab terjadinya *race condition* dan cara mencegahnya.
4. Memahami proses terjadinya *deadlock* dan cara mengatasinya.
5. Memahami algoritma Raft dalam pemilihan *leader* pada sistem terdistribusi.
6. Mampu membuat dan menggunakan modul Python untuk Vector Clock dan simulasi algoritma Raft.

## 1.3 Alat dan Bahan

Alat dan bahan yang digunakan dalam praktikum ini meliputi:

1. Komputer atau laptop.
2. Sistem operasi Windows.
3. Perangkat lunak NetTime untuk sinkronisasi waktu.
4. Python sebagai bahasa pemrograman.
5. Visual Studio Code sebagai editor kode program.
6. Source code praktikum Modul 3.
7. Koneksi internet untuk proses sinkronisasi waktu.

# BAB II DASAR TEORI

## 2.1 Sinkronisasi Waktu

Sinkronisasi waktu merupakan proses menyamakan waktu pada beberapa perangkat agar memiliki acuan waktu yang konsisten. Dalam sistem terdistribusi, sinkronisasi waktu dibutuhkan karena setiap komputer memiliki jam internal yang dapat menunjukkan waktu berbeda.

Salah satu protokol yang digunakan adalah NTP (*Network Time Protocol*). NTP memungkinkan perangkat menyelaraskan waktu dengan server waktu melalui jaringan. Selain NTP, terdapat PTP (*Precision Time Protocol*) yang digunakan untuk kebutuhan sinkronisasi dengan ketelitian tinggi pada lingkungan yang mendukungnya.


## 2.2 Deadlock
<img width="272" height="302" alt="image" src="https://github.com/user-attachments/assets/de373dd4-8031-49cd-a8d1-2c1af0f68129" />

## 2.3 Deadlock02

## 2.4 multithreads

## 2.5 Race condition

## 2.6 Race condition02

## 2.7 Raft

## 2.8 vlock
