## Laporan Praktikum Sistem Operasi Jobsheet

<h4>Nama : Rafif Rizdan Prastana<h4>
<h4>NIM  : 254107020052<h4>
<h4>Kelas : TI 1H<h4>

### Dasar Input/Output (I/O)

### 1.1 Filosofi I/O Unix: “Everything is a File”
#### Informasi CPU dapat dibaca seperti file biasa
![namafile](images/1satu.png)

#### Informasi memory
![namafile](images/1dua.png)

### 1.2 File Descriptor di Linux

#### Melihat file descriptor dari proses bash saat ini
![namafile](images/2satu.png)

### 1.3 Pengalihan I/O Standar

#### Membaca input dari file.txt alih-alih keyboard
![namafile](images/3satu.png)

### Menghitung jumlah baris dari file
![namafile](images/3dua.png)

### 1.4 Pipa dan Operator |

#### Menampilkan 10 file terbesar
![namafile](images/4satu.png)

#### Menghitung jumlah user di sistem
![namafile](images/4dua.png)

#### Mencari proses tertentu
![namafile](images/4tiga.png)

### 1.6 Penggunaan tee untuk Output Ganda

#### Tampilkan di layar DAN simpan ke file
![namafile](images/6.png)

#### Append ke file dengan -a
![namafile](images/6.png)

#### Menulis ke beberapa file sekaligus
![namafile](images/6.png)

### 1.7 Teknik Lanjutan I/O

#### Membandingkan output dua perintah
![namafile](images/7satu.png)

#### Menggunakan output sebagai input
![namafile](images/7dua.png)

#### Menggabungkan hasil beberapa perintah
![namafile](images/7tiga.png)

### 1.8 Kapan Menggunakan Teknik I/O Tertentu?

#### Menggunakan pipe (lebih efisien)
![namafile](images/8.png)

### 1.9 Best Practices I/O Redirection

### 1.11 Latihan

#### Latihan 3.1

#### 1. Menampilkan daftar 10 file terbesar di direktori 
![namafile](images/11satu.png)

#### 2. Menyimpan hasilnya ke file large-logs.txt
![namafile](images/11dua.png)

#### 3. Menampilkan output juga di terminal menggunakan tee
![namafile](images/11tiga.png)

#### 4. Menangani error dengan redirect ke error.log
![namafile](images/11empat.png)

### Latihan 3.2

#### 1. Membaca /etc/passwd
![namafile](images/12satu.png)

#### 2. Mengekstrak username (kolom pertama)
![namafile](images/12dua.png)

#### 3. Mengurutkan alfabetis
![namafile](images/12tiga.png)

#### 4. Menyimpan ke file sorted-users.txt
![namafile](images/12empat.png)

### Latihan 3.3

#### 1. Mencatat penggunaan CPU dan memory setiap 5 detik
![namafile](images/13satu.png)

#### 2. Menyimpan log dengan timestamp
![namafile](images/13dua.png)

#### 3. Berjalan selama 1 menit (12 iterasi)
![namafile](images/13tiga.png)

#### 4. Output ditampilkan di terminal DAN disimpan ke file
![namafile](images/13empat.png)

### Latihan 3.4

#### 1. Mencari semua file .conf di sistem
![namafile](images/15satu.png)

#### 2. Membuang pesan "Permission denied"
![namafile](images/15dua.png)

#### 3. Menghitung jumlah file yang ditemukan
![namafile](images/15tiga.png)

#### 4. Menyimpan daftar path lengkap ke file
![namafile](images/15empat.png)

### Latihan 3.5

#### 1. Menggunakan tar untuk backup direktori
#### 2. Menampilkan progress dengan tee
#### 3. Mencatat stdout ke backup-success.log
#### 4. Mencatat stderr ke backup-error.log
#### 5. Menambahkan timestamp di setiap log entry


![namafile](images/16satu.png)
![namafile](images/16dua.png)
![namafile](images/16tiga.png)






