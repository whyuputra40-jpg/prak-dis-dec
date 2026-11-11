* **Judul Laporan:** Laporan Praktikum Modul 6 - Distributed File System (HDFS)


* **Mata Kuliah:** Praktikum Sistem Terdistribusi dan Terdesentralisasi


* **Disusun Oleh:** [Avatar Dava Wahyuputra] / [255410008]
* **Program Studi / Institusi:** Informatika, Universitas Teknologi Digital Indonesia



---

### **DAFTAR ISI**

---

### **BAB I: PENDAHULUAN**

* **1.1 Latar Belakang**
Perkembangan era Big Data menuntut penggunaan sistem penyimpanan terdistribusi yang andal seperti Apache Hadoop Distributed File System (HDFS) untuk mengatasi keterbatasan komputer tunggal dalam mengolah data berskala besar. Praktikum ini bertujuan untuk membekali mahasiswa dengan pemahaman praktis mengenai instalasi, konfigurasi lingkungan kerja, inisialisasi NameNode, pengoperasian daemon HDFS, hingga manipulasi pengelolaan file terdistribusi secara langsung


* **1.2 Tujuan Praktikum**
 Memahami konsep dasar *Hadoop Distributed File System* (HDFS).


 Melakukan penyiapan lingkungan kerja, instalasi Apache Hadoop, serta konfigurasi pendukung seperti JDK, `pdsh`, dan *passwordless* SSH.


 Melakukan format NameNode dan menjalankan *daemon* HDFS.


 Memahami dan mempraktikkan perintah-perintah dasar manajemen direktori serta file pada HDFS.


* Melakukan penyiapan lingkungan kerja, instalasi Apache Hadoop, dan konfigurasi pendukung (`pdsh`, SSH *passwordless*, `JAVA_HOME`).
<img width="464" height="444" alt="image" src="https://github.com/user-attachments/assets/cfda9df6-85c7-407e-a3aa-fd85b7247ac6" />

<img width="197" height="38" alt="image" src="https://github.com/user-attachments/assets/1a4e970a-7930-4cff-8233-463c42d0393d" />
penjelasan:
Gambar tersebut menampilkan halaman unduhan (download page) resmi dari Apache Hadoop yang menyediakan informasi mengenai berbagai versi perangkat lunak yang dirilis, termasuk tanggal rilis, tautan unduhan untuk kode sumber (source download) dan biner (binary download), serta catatan rilis (release notes). Berdasarkan informasi yang tertera, Hadoop dirilis sebagai tarball kode sumber beserta tarball biner pendukung yang didistribusikan melalui situs cermin (mirror sites), di mana pengguna diimbau untuk memeriksa keamanannya terhadap potensi perusakan menggunakan GPG atau SHA-512. Tabel tersebut memperlihatkan beberapa versi yang tersedia seperti versi 3.5.0, 3.4.3, 3.4.2, dan 3.4.1 lengkap dengan tanggal rilis masing-masing.





### **BAB II: DASHBOARD & LINGKUNGAN KERJA**

* **2.1 Persyaratan Perangkat Lunak (*Prerequisites*)**
  <img width="462" height="146" alt="image" src="https://github.com/user-attachments/assets/6061d2e1-eea0-422d-817e-2098bdd1dd18" />
penjelasan:
Gambar tersebut menampilkan jendela terminal PowerShell di sistem Windows (PS C:\Users\X1 Yoga) yang sedang menjalankan perintah pemeriksaan versi Java serta variabel lingkungan. Berdasarkan output di layar, sistem telah berhasil menginstal Java versi 25 (LTS) beserta kompiler javac versi 25. Akan tetapi, saat perintah echo $env:JAVA_HOME dijalankan, variabel lingkungan JAVA_HOME tidak menampilkan hasil apa pun, yang menandakan bahwa variabel tersebut belum dikonfigurasi pada sistem.

* Apache Hadoop (Versi 3.5.0).
<img width="467" height="174" alt="image" src="https://github.com/user-attachments/assets/dccad55f-1cb9-4e69-84e3-1a9817019079" />
penjelasan:
Unduhan file hadoop-3.5.0.tar.gz telah berhasil diselesaikan melalui terminal Linux dengan ukuran sekitar 554 MB. Langkah berikutnya yang perlu Anda lakukan adalah mengekstrak file arsip tersebut dan mengatur konfigurasi lingkungan sistem agar Hadoop siap dijalankan.

* Utilities (`pdsh`, SSH, `sshd`).
<img width="426" height="135" alt="image" src="https://github.com/user-attachments/assets/0885bad4-6a10-40f6-924c-ac893bc939ac" />
penjelasan:
Status layanan SSH pada terminal menunjukkan bahwa ssh.service saat ini berada dalam kondisi active (running) dan berjalan dengan normal. Namun, terdapat catatan bahwa status layanan tersebut tertera disabled, yang berarti layanan SSH tidak akan otomatis aktif kembali secara otomatis ketika sistem (komputer/server) direstart


* **2.2 Spesifikasi Lingkungan Sistem**
* Sistem Operasi yang digunakan (misal: Artix Linux, Ubuntu, dll.).
<img width="381" height="278" alt="image" src="https://github.com/user-attachments/assets/107c9281-cb3b-4e94-bd7d-9a39e740dcd9" />
<img width="462" height="155" alt="image" src="https://github.com/user-attachments/assets/1b041bf9-e6e8-438e-bafd-84f6f3103de5" />
penjelasan:
Gambar terminal di atas menunjukkan proses instalasi WSL (Windows Subsystem for Linux) dengan distribusi Ubuntu-24.04 yang berhasil diselesaikan.

* Pengaturan Variabel Lingkungan (`PATH` dan `JAVA_HOME`).
<img width="385" height="64" alt="image" src="https://github.com/user-attachments/assets/7b8a3138-fe4f-454b-8408-9e44b7634524" />
penjelasan:
Perintah-perintah di terminal Linux tersebut menunjukkan bahwa Anda sedang mengatur variabel lingkungan untuk Hadoop dengan mengekspor JAVA_HOME, HADOOP_HOME, serta memperbarui PATH. Anda juga telah memuat ulang konfigurasi tersebut melalui file env.sh dan menambahkannya secara permanen ke dalam file .bashrc




---

### **BAB III: LANGKAH PRATIKUM DAN HASIL IMPLEMENTASI**

* **3.1 Pengunduhan & Ekstraksi Apache Hadoop**
 Langkah penyiapan *directory*, ekstraksi file `hadoop-3.5.0.tar.gz`, dan pembuatan *symbolic link* (`$HADOOP_HOME`).
<img width="455" height="292" alt="image" src="https://github.com/user-attachments/assets/93419638-6ee9-4688-acea-d88c12084e6d" />
penjelasan:
Gambar terminal di atas menunjukkan tahapan lengkap proses penyiapan Hadoop di direktori ~/software/storage

* Pengaturan file environment `hadoop.sh` serta pengujian perintah `hadoop`.
<img width="464" height="76" alt="image" src="https://github.com/user-attachments/assets/317c3e7b-ede1-46a6-a0b6-951380850b7c" />
penjelasan:
Gambar terminal di atas menunjukkan bahwa perintah hadoop version telah berhasil dijalankan dengan sukses.




* **3.2 Instalasi & Konfigurasi `pdsh` dan SSH**


* Langkah kompilasi/instalasi paket `pdsh`.
<img width="367" height="166" alt="image" src="https://github.com/user-attachments/assets/1d65e711-6a63-4456-a7de-0eef8b8999db" />
penjelasan:
Gambar terminal di atas menunjukkan bahwa Anda menjalankan perintah untuk memulai layanan SSH (sudo service ssh start) dan memeriksa statusnya (sudo service ssh status). Berdasarkan informasi yang ditampilkan, layanan OpenBSD Secure Shell server (ssh.service) berada dalam kondisi active (running) sejak hari Kamis, 5 November 2026 pukul 10:47:46 WIB.   Selain itu, log sistem di bagian bawah mengo

* Pengaturan `PDSH_RCMD_TYPE=ssh`.
<img width="431" height="32" alt="image" src="https://github.com/user-attachments/assets/857abc1a-68be-44f2-83ed-c41a52c3978a" />
penjelasan:
Perintah di terminal menunjukkan bahwa variabel lingkungan PDSH_RCMD_TYPE=ssh telah berhasil ditambahkan ke dalam file env.sh dan diverifikasi dengan sukses, menghasilkan output ssh. Pengaturan ini sangat penting saat menjalankan Hadoop agar utilitas parallel remote shell (pdsh) menggunakan protokol SSH alih-alih RSH

* Pembuatan SSH Key Pair (`ssh-keygen`) dan pengaturan *passwordless SSH* ke `localhost`.
<img width="463" height="352" alt="image" src="https://github.com/user-attachments/assets/a9aa5f0b-bb70-46ba-a20c-dde6e089b118" />
penjelasan:
Gambar terminal di atas menunjukkan bahwa Anda telah berhasil membuat kunci SSH baru (ssh-keygen), menambahkannya ke file authorized_keys, dan mengatur izin file dengan benar menggunakan chmod 600




* **3.3 Konfigurasi File Hadoop XML**


* Penjelasan edit/konfigurasi file:
1. `hdfs-site.xml` (Replikasi data).
  <img width="317" height="221" alt="image" src="https://github.com/user-attachments/assets/c28a4e8b-678e-45a7-8f14-a83ac223b558" />


2. `core-site.xml` (Pengaturan Default FS NameNode & Proxy user).
  <img width="327" height="275" alt="image" src="https://github.com/user-attachments/assets/bd002181-057d-4ac5-b97e-d8ce25286c9f" />

  
3. `mapred-site.xml` (Pengaturan MapReduce Framework YARN).
  <img width="284" height="196" alt="image" src="https://github.com/user-attachments/assets/379b01c9-a3f5-4812-8fe0-43cac9aec3e1" />


4. `yarn-site.xml` (Pengaturan NodeManager & Alokasi Memori).
  <img width="322" height="206" alt="image" src="https://github.com/user-attachments/assets/aad4bba5-5d80-47a0-8a31-ecf2ffcf6f4c" />


5. `hadoop-env.sh` (Pengaturan `JAVA_HOME`).
<img width="362" height="383" alt="image" src="https://github.com/user-attachments/assets/082085f5-2d25-49b6-a61b-f67b8b8ab9ca" />






* **3.4 Inisialisasi (Format NameNode) & Pengoperasian Daemon**


* Pelaksanaan format NameNode (`hdfs namenode -format`).
<img width="473" height="408" alt="image" src="https://github.com/user-attachments/assets/6d8ffd93-a983-4d4a-8ea3-00ade59abe1c" />
<img width="466" height="264" alt="image" src="https://github.com/user-attachments/assets/ad4e1c17-4f86-4353-961f-00ad6e99559a" />



* Menjalankan daemon HDFS (`start-dfs.sh`).
<img width="102" height="27" alt="image" src="https://github.com/user-attachments/assets/4409cec7-6e4d-4c0a-9703-ea2966e9f6d0" />


* Pemantauan Cluster via Web UI HDFS (`http://localhost:9870/dfshealth.html`).
<img width="958" height="509" alt="image" src="https://github.com/user-attachments/assets/8c9ffb05-8395-4487-a3c0-14eba10c28ae" />


* Penghentian daemon HDFS (`stop-dfs.sh`).
  <img width="385" height="67" alt="Screenshot 2026-11-05 133137" src="https://github.com/user-attachments/assets/473c0e98-d8da-4ef5-b42e-277f0eb31535" />

<img width="183" height="47" alt="image" src="https://github.com/user-attachments/assets/69f9bc58-09c0-4128-b08d-b059d01e03f1" />




---

### **BAB IV: PENYELESAIAN TUGAS PRAKTIKUM**

* **4.1 Pembuatan Direktori Dataset di HDFS**


* *Perintah:* Perintah CLI untuk membuat direktori `/user/<nama-user>/datasets` (misal: `hdfs dfs -mkdir -p /user/bpdp/datasets`).
<img width="385" height="67" alt="image" src="https://github.com/user-attachments/assets/803c9959-7a20-4777-9c4d-a18c2749a8a2" />





* **4.2 Mengunggah File CSV dari Local ke HDFS**

<img width="477" height="399" alt="image" src="https://github.com/user-attachments/assets/3041d3a9-244f-4592-ab6e-bd828821fa60" />


<img width="431" height="47" alt="image" src="https://github.com/user-attachments/assets/4dc42811-e1b5-4de4-8512-52157f600875" />

penjelasan:

### **BAB V: KESIMPULAN DAN SARAN**

* **5.1 Kesimpulan**
  Berdasarkan praktikum Modul 6 tentang Distributed File System (HDFS), dapat disimpulkan bahwa Apache Hadoop HDFS merupakan sistem penyimpanan terdistribusi yang digunakan untuk menyimpan dan mengelola data dalam jumlah besar. Melalui praktikum ini, mahasiswa mempelajari proses instalasi dan konfigurasi Hadoop, pengaturan Java dan SSH, konfigurasi NameNode dan DataNode, serta menjalankan layanan HDFS. Selain itu, mahasiswa juga memahami cara memantau kondisi cluster melalui Web UI HDFS dan mengelola layanan Hadoop menggunakan perintah terminal.

  Praktikum ini memberikan pemahaman mengenai penggunaan perintah dasar HDFS untuk membuat direktori, melihat struktur penyimpanan, serta mengunggah file ke dalam sistem penyimpanan terdistribusi. Hal tersebut dibuktikan dengan keberhasilan membuat direktori datasets dan mengunggah tiga file CSV, yaitu iris.csv, tips.csv, dan penguins.csv. Dengan demikian, melalui Modul 6 ini mahasiswa dapat memahami dasar penggunaan HDFS serta mengetahui bagaimana Hadoop membantu proses penyimpanan dan pengelolaan data secara terdistribusi.
