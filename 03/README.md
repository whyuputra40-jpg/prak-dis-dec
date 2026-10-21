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
penjelasan:
Kode Python tersebut mendemonstrasikan kondisi deadlock pada pemrograman multithreading, di mana t1 mengunci lock_a dan menunggu lock_b, sementara secara bersamaan t2 mengunci lock_b dan menunggu lock_a. Karena kedua thread saling menunggu kunci yang dipegang oleh yang lain tanpa saling melepasnya, program akan berhenti dan tertahan selamanya (hang).
## 2.3 Deadlock02

<img width="284" height="442" alt="Screenshot 2026-10-21 221013" src="https://github.com/user-attachments/assets/b2fa74a1-d667-4f9c-abc5-e04b1d1c3197" />
penjelasan;
Kode Python tersebut mendemonstrasikan cara mencegah kondisi deadlock pada multithreading dengan menggunakan parameter timeout pada metode acquire() serta melepaskan (release) kunci pertama sebelum mencoba mengambil kunci berikutnya. Karena thread1_task mengambil lock_a, melepaskannya, lalu mengambil lock_b (begitu pula sebaliknya untuk thread2_task), program terhindar dari hang permanen dan dapat menyelesaikan seluruh eksekusinya dengan sukses.

## 2.4 multithreads
<img width="232" height="320" alt="Screenshot 2026-10-21 221311" src="https://github.com/user-attachments/assets/8a9ff0a7-e31f-434d-882b-a3b5f2cfffc1" />
penjelasan:
Kode Python tersebut mendemonstrasikan implementasi multithreading dasar menggunakan modul threading untuk menjalankan lima tugas secara bersamaan (concurrent), di mana setiap thread menjalankan fungsi task yang mensimulasikan operasi I/O-bound dengan jeda waktu selama 5 detik. Melalui pemanggilan metode .start() dan .join(), program memastikan kelima thread (t1 hingga t5) dieksekusi secara paralel dan menunggu semuanya benar-benar selesai sebelum akhirnya mencetak teks "All tasks completed.".

## 2.5 Race condition
<img width="326" height="199" alt="Screenshot 2026-10-21 221316" src="https://github.com/user-attachments/assets/279b568d-b890-44bb-837f-4bfa28ce4f3f" />
penjelasan:
Kode Python tersebut mendemonstrasikan masalah race condition (kondisi balapan) dalam pemrograman multithreading, di mana dua thread (t1 dan t2) secara bersamaan mengakses dan memodifikasi variabel bersama balance tanpa mekanisme penguncian (lock). Karena adanya jeda time.sleep(0.01) setelah pengecekan kondisi saldo, kedua thread dapat membaca saldo awal sebesar 100 secara bersamaan yang dianggap mencukupi untuk menarik 80, sehingga berpotensi menyebabkan saldo akhir menjadi tidak konsisten atau bernilai negatif.

## 2.6 Race condition02
<img width="316" height="266" alt="Screenshot 2026-10-21 221322" src="https://github.com/user-attachments/assets/8dfb08e8-8d78-4e90-b274-3b4a5e1b84ec" />
penjelasan:
Kode Python tersebut mendemonstrasikan cara mengatasi masalah race condition pada multithreading dengan menerapkan mekanisme penguncian menggunakan threading.Lock() (counter_lock). Melalui penggunaan blok with counter_lock:, hanya satu thread yang dapat mengakses dan memodifikasi variabel bersama balance dalam satu waktu, sehingga proses pengecekan saldo serta penarikan dapat berjalan aman, konsisten, dan terhindar dari hasil yang tidak valid

## 2.7 Raft
<img width="281" height="479" alt="Screenshot 2026-10-21 221342" src="https://github.com/user-attachments/assets/1923c641-a2fa-428a-aeb6-a73399cccd41" />
<img width="325" height="285" alt="Screenshot 2026-10-21 221349" src="https://github.com/user-attachments/assets/6dc2ea0b-ebb1-481e-beef-5b1b4fd8ba55" />
penjelasan:
Kode Python tersebut mendemonstrasikan simulasi sederhana dari algoritma konsensus Raft untuk koordinasi sistem terdistribusi, di mana kelas RaftNode mengatur transisi status node antara FOLLOWER, CANDIDATE, dan LEADER. Melalui fungsi simulasi dan mekanisme tick, program memodelkan pemilihan pemimpin berbasis timeout acak, pengiriman heartbeat, serta penambahan entri log secara tersinkronisasi di antara beberapa node

## 2.8 vlock
<img width="219" height="427" alt="Screenshot 2026-10-21 221402" src="https://github.com/user-attachments/assets/c0246fb4-7bee-4f74-a3f9-8e255e92b093" />
penjelasan:
Kode Python tersebut mendemonstrasikan implementasi Vector Clock (Jam Vektor) untuk melacak hubungan kausalitas antar kejadian dalam sistem terdistribusi. Kelas VectorClock menyediakan metode untuk melakukan increment, pengiriman serta penerimaan pesan, penentuan relasi kausalitas (happens_before), dan pengecekan konkurensi, yang kemudian disimulasikan menggunakan tiga proses (vc0, vc1, vc2) untuk memantau serta mengevaluasi urutan waktu logis antar proses secara akurat

2.9 Kesimpulan
Berdasarkan praktikum Modul 3 tentang Sinkronisasi pada Sistem Terdistribusi dan Terdesentralisasi, dapat disimpulkan bahwa sinkronisasi memiliki peran penting dalam menjaga koordinasi dan konsistensi proses pada sistem terdistribusi. Sinkronisasi waktu menggunakan NTP membantu menyelaraskan waktu komputer dengan server waktu. Selain itu, Vector Clock digunakan untuk mengetahui urutan dan hubungan antarperistiwa pada beberapa proses yang berjalan secara terdistribusi.

Praktikum ini juga membahas permasalahan yang dapat terjadi ketika proses berjalan tanpa sinkronisasi yang tepat, yaitu race condition dan deadlock. Race condition dapat terjadi ketika beberapa thread mengakses data bersama tanpa pengaturan yang sesuai, sedangkan deadlock terjadi ketika beberapa proses saling menunggu sumber daya sehingga tidak dapat melanjutkan pekerjaannya. Kedua masalah tersebut dapat dicegah melalui pengelolaan akses dan sumber daya yang tepat.

Selanjutnya, algoritma Raft digunakan untuk memahami proses pemilihan leader dalam sistem terdistribusi melalui mekanisme pemungutan suara. Melalui praktikum ini, mahasiswa dapat memahami konsep sinkronisasi waktu, pengurutan kejadian, pencegahan masalah konkurensi, serta pemilihan pemimpin dalam sistem terdistribusi. Penerapan konsep-konsep tersebut diharapkan dapat membantu mahasiswa memahami cara kerja sistem terdistribusi dan mengimplementasikannya menggunakan bahasa pemrograman Python.
