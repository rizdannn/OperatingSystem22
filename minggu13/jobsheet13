## Laporan Praktikum Sistem Operasi Jobsheet

<h4>Nama : Rafif Rizdan Prastana<h4>
<h4>NIM  : 254107020052<h4>
<h4>Kelas : TI 1H<h4>


## 1. Persiapan Sistem Dasar

### Siapkan file ISO Ubuntu versi LTS (Long Term Support) terbaru.
### Gunakan tool remastering pilihan Anda (sangat disarankan menggunakan Cubic).

#### Perintah 1
```
sudo apt-add-repository universe
```
![namafile](images/1satu.png)
![namafile](images/1satuu.png)
![namafile](images/1dua.png)
![namafile](images/1empat.png)

#### Perintah 2 
```
sudo apt-add-repository ppa:cubic-wizard/release
```

#### Peirntah 3
```
sudo apt update
```

#### Perintah 4 
```
sudo apt install cubic
```



## 2. Kustomisasi dan Instalasi Aplikasi Pasang beberapa aplikasi berikut ke dalam sistem yang sedang di-remaster:

### VLC Media Player (Pemutar multimedia)

#### Perintah 1 — VLC
```
apt install -y vlc
```

### GIMP (Editor gambar)

#### Perintah 2 — GIMP
```
apt install -y gimp
```

![namafile](images/3dua.png)

### Apache2 + PHP (Web server lokal)

#### Perintah 3 — Apache2 + PHP
```
apt install -y apache2 php libapache2-mod-php
```

![namafile](images/3tiga.png)

### Visual Studio Code (Code editor)

#### Perintah 4 — VS Code 
```
wget -O /tmp/microsoft.asc https://packages.microsoft.com/keys/microsoft.asc
```

#### Langkah 2
```
gpg --dearmor < /tmp/microsoft.asc > /tmp/microsoft.gpg
```
#### Langkah 3
```
install -o root -g root -m 644 /tmp/microsoft.gpg /etc/apt/trusted.gpg.d/
```
#### Langkah 4
```
echo "deb [arch=arm64] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list
```
#### Langkah 5
```
apt update
apt install -y code
```

![namafile](images/3empat.png)

## Aplikasi Kustom Buatan Sendiri: Buat sebuah program sederhana berbasis Bash Script yang berfungsi untuk menampilkan informasi dasar perangkat keras komputer, meliputi:
```
Informasi Prosesor (CPU)
Kapasitas dan Penggunaan Memori (RAM)
Kapasitas Ruang Penyimpanan (Storage)
```

#### Perintah 1 — Membuat File Script
```
nano /usr/local/bin/sysinfo
```
#### Perintah 2 — Baris Pertama Script (Shebang)
```
#!/bin/bash

echo "=================================="
echo "   INFORMASI PERANGKAT KERAS"
echo "=================================="

echo ""
echo ">>> PROSESOR (CPU)"
echo "----------------------------------"
grep "Model name" /proc/cpuinfo | head -1 | cut -d: -f2 | xargs
echo "Core: $(nproc) core(s)"
echo "Arsitektur: $(uname -m)"

echo ""
echo ">>> MEMORI (RAM)"
echo "----------------------------------"
free -h | awk '/^Mem:/ {print "Total  : " $2 "\nDigunakan: " $3 "\nTersedia : " $7}'

echo ""
echo ">>> PENYIMPANAN (STORAGE)"
echo "----------------------------------"
df -h --output=source,size,used,avail,pcent | grep -v tmpfs | grep -v udev | grep -v loop

echo ""
echo "=================================="
```

#### Perintah 3 — Memberi Izin Eksekusi
```
chmod +x /usr/local/bin/sysinfo
```

#### Perintah 10 — Menjalankan Script
```
sysinfo
```


#### Hasilnya

![namafile](images/3lima.png)
![namafile](images/3limaa.png)
![namafile](images/3limaaa.png)

![namafile](images/3limaaaa.png)

## 3. Kustomisasi Tampilan (Antarmuka)

### Ubah visual atau estetika pada Desktop Environment bawaan, yang mencakup perubahan pada:

### Wallpaper (Latar belakang desktop)

#### Langkah 1 — Download Wallpaper
```
wget -O /usr/share/backgrounds/custom-wallpaper.jpg "https://images.unsplash.com/..."
```

#### Langkah 2 — Buat Folder Konfigurasi
```
mkdir -p /etc/dconf/db/local.d
```

#### Langkah 3 — Set Wallpaper Default
```
cat > /etc/dconf/db/local.d/01-wallpaper << 'EOF'
[org/gnome/desktop/background]
picture-uri='file:///usr/share/backgrounds/custom-wallpaper.jpg'
picture-uri-dark='file:///usr/share/backgrounds/custom-wallpaper.jpg'
picture-options='zoom'
EOF
```

#### BAGIAN 4
```
dconf update
```

![namafile](images/4satu.png)

### Tema sistem (Theme)

#### Langkah 1 — Install Tema Yaru Dark
```
apt install -y yaru-theme-gtk yaru-theme-icon yaru-theme-sound
```

#### Langkah 2 — Set Tema Default
```
cat > /etc/dconf/db/local.d/02-theme << 'EOF'
[org/gnome/desktop/interface]
gtk-theme='Yaru-dark'
color-scheme='prefer-dark'
EOF
```

#### BAGIAN 3
```
dconf update
```

![namafile](images/4dua.png)

### Paket ikon (Icons pack)

#### Langkah 1 — Install Papirus Icon
```
apt install -y papirus-icon-theme
```

#### Langkah 2 — Set Icon Default
```
cat > /etc/dconf/db/local.d/03-icons << 'EOF'
[org/gnome/desktop/interface]
icon-theme='Papirus-Dark'
EOF
```

#### BAGIAN 4
```
dconf update
```

![namafile](images/4tiga.png)


## 4. Pembuatan File ISO Baru

## Lakukan proses build atau generate untuk menghasilkan file ISO baru hasil modifikasi Anda, lalu beri nama dengan format: Ubuntu-Custom-[NIM].iso (Ganti [NIM] dengan Nomor Induk Mahasiswa Anda).
![namafile](images/5satu.png)


## Pengujian dan Dokumentasi


![namafile](images/6satu.jpg)
![namafile](images/6dua.jpg)
![namafile](images/6tiga.jpg)
