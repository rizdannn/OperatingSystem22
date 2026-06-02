## Laporan Praktikum Sistem Operasi Jobsheet 

<h4>Nama  : Rafif Rizdan Prastana<h4>
<h4>NIM   : 254107020052<h4>
<h4>Kelas : TI 1H<h4>

## Praktikum 10.1 — Amati Layanan Aktif Saat Boot 

### Langkah 1 — Lihat semua layanan yang sedang berjalan:
```
systemctl list-units --type=service --state=running
```
![namafile](images/1satu.png)

### Langkah 2 — Lihat semua unit service:
```
systemctl list-unit-files --type=service | head -30
```
![namafile](images/1dua.png)

### Langkah 3 — Analisis waktu boot:
```
systemd-analyze
systemd-analyze blame | head -15
```
![namafile](images/1tiga.png)

## Tantangan 10.1 — Tiga Layanan Terlama Saat Boot
```
systemd-analyze blame | sort -rh | head -3
```
![namafile](images/t1.png)

### Untuk setiap layanan yang muncul, cek fungsinya:
```
systemctl cat nama-layanan
```
![namafile](images/t11.png)


## Praktikum 10.2 — Kelola Layanan SSH

### Langkah 1 — Periksa status SSH:
```
systemctl status ssh
systemctl is-active ssh
systemctl is-enabled ssh
```
![namafile](images/2satu.png)

### Langkah 2 — Restart dan pantau:
```
sudo systemctl restart ssh
systemctl status ssh
```
![namafile](images/2dua.png)

### Langkah 3 — Lihat dependensi SSH:
```
systemctl status ssh
```
![namafile](images/2tiga.png)

### Langkah 4 — Cek unit yang gagal:
```
systemctl --failed
```
![namafile](images/2empat.png)

## Tantangan 10.2 — Script cek-layanan.sh

### Langkah 1 — Buat file daftar layanan:
```
nano ~/lab-os/chapter10-services/daftar-layanan.txt
```
![namafile](images/t2.png)

### Ketik isi berikut:
```
ssh
cron
rsyslog
```
![namafile](images/t22.png)

### Langkah 2 — Buat script:
```
nano ~/lab-os/chapter10-services/cek-layanan.sh
```
```
#!/bin/bash
# Script: cek-layanan.sh
# Fungsi: cek status layanan dari file daftar-layanan.txt

DAFTAR="$HOME/lab-os/chapter10-services/daftar-layanan.txt"
LAPORAN="$HOME/lab-os/chapter10-services/laporan-layanan.log"

# Validasi file daftar ada
if [ ! -f "$DAFTAR" ]; then
    echo "ERROR: File $DAFTAR tidak ditemukan."
    exit 1
fi

# Bersihkan laporan lama
> "$LAPORAN"

# Baca setiap layanan dan cek statusnya
while read -r LAYANAN; do
    # skip baris kosong
    [ -z "$LAYANAN" ] && continue

    STATUS=$(systemctl is-active "$LAYANAN" 2>/dev/null)

    if [ "$STATUS" = "active" ]; then
        LABEL="ACTIVE"
    else
        LABEL="INACTIVE"
    fi

    TANGGAL=$(date '+%Y-%m-%d %H:%M:%S')
    echo "[$TANGGAL] $LAYANAN: $LABEL" | tee -a "$LAPORAN"

done < "$DAFTAR"

echo ""
echo "Laporan disimpan ke: $LAPORAN"
```
![namafile](images/t2222.png)

### Langkah 3 — Jalankan:
```
chmod +x ~/lab-os/chapter10-services/cek-layanan.sh
bash ~/lab-os/chapter10-services/cek-layanan.sh
```
![namafile](images/t22222.png)

### Langkah 4 — Lihat hasil laporan:
```
cat ~/lab-os/chapter10-services/laporan-layanan.log
```
![namafile](images/t222222.png)


## Praktikum 10.3 — Buat Layanan Sederhana

### Langkah 1 — Siapkan konten:
```
cd ~/lab-os/chapter10-services
mkdir -p situs-demo
nano situs-demo/index.html
```
![namafile](images/3satu.png)

### Ketik isi berikut:
```
<!DOCTYPE html>
<html>
<body>
<h1>Halo dari layanan systemd kustom!</h1>
<p>Layanan ini dibuat pada praktek Bab 10.</p>
</body>
</html>
```
![namafile](images/3dua.png)

### Langkah 2 — Buat skrip wrapper:
```
nano ~/lab-os/chapter10-services/jalankan-server.sh
```
![namafile](images/3tiga.png)

### Ketik isi berikut:
```
#!/bin/bash
DIREKTORI="$HOME/lab-os/chapter10-services/situs-demo"
PORT=9090
echo "Memulai server di port $PORT..."
exec python3 -m http.server $PORT --directory "$DIREKTORI"
```
![namafile](images/3empat.png)

### Langkah 3 — Buat berkas unit:
```
nano ~/lab-os/chapter10-services/demo-web.service
```
![namafile](images/3lima.png)

### Ketik isi berikut (ganti nama-pengguna-kamu dengan username kamu):
```
[Unit]
Description=Demo Web Server Praktek Bab 10
After=network.target

[Service]
Type=simple
User=nama-pengguna-kamu
WorkingDirectory=/home/nama-pengguna-kamu/lab-os/chapter10-services/situs-demo
ExecStart=/usr/bin/python3 -m http.server 9090
Restart=on-failure
RestartSec=3s

[Install]
WantedBy=multi-user.target
```
![namafile](images/3enam.png)
```
sudo cp ~/lab-os/chapter10-services/demo-web.service /etc/systemd/system/demo-web.service
sudo systemctl daemon-reload
```
![namafile](images/3tujuh.png)

### Langkah 4 — Jalankan dan verifikasi:
```
sudo systemctl start demo-web
systemctl status demo-web
curl http://localhost:9090
```
![namafile](images/3delapan.png)

### Langkah 5 — Uji restart otomatis:
```
systemctl status demo-web | grep "Main PID"
sudo kill -9 $(systemctl show demo-web --property=MainPID --value)
sleep 5
systemctl status demo-web
```
![namafile](images/3sembilan.png)

### Langkah 6 — Bersihkan:
```
sudo systemctl disable --now demo-web
sudo rm /etc/systemd/system/demo-web.service
sudo systemctl daemon-reload
```
![namafile](images/3sepuluh.png)


## Tantangan 10.3 — Modifikasi demo-web.service

### Langkah 1 — Edit berkas unit:
```
nano ~/lab-os/chapter10-services/demo-web.service
```
![namafile](images/t3.png)
```
[Unit]
Description=Demo Web Server Praktek Bab 10
After=network.target

[Service]
Type=simple
User=nama-pengguna-kamu
WorkingDirectory=/home/nama-pengguna-kamu/lab-os/chapter10-services/situs-demo
Environment="PORT=9091"
ExecStart=/usr/bin/python3 -m http.server ${PORT}
Restart=on-failure
RestartSec=10s

[Install]
WantedBy=multi-user.target
```
![namafile](images/t33.png)

### Langkah 2 — Salin dan reload:
```
sudo cp ~/lab-os/chapter10-services/demo-web.service /etc/systemd/system/demo-web.service
sudo systemctl daemon-reload
```
![namafile](images/t333.png)

### Langkah 3 — Aktifkan dan jalankan:
```
sudo systemctl enable --now demo-web
systemctl status demo-web
```
![namafile](images/t3333.png)

### Langkah 4 — Verifikasi port baru:
```
curl http://localhost:9091
ss -tlnp | grep 9091
```
![namafile](images/t33333.png)

### Langkah 5 — Uji restart otomatis dengan RestartSec=10s:
```
# Catat PID sebelum kill
systemctl status demo-web | grep "Main PID"

# Kill paksa
sudo kill -9 $(systemctl show demo-web --property=MainPID --value)

# Tunggu 10 detik (sesuai RestartSec=10s)
sleep 12
systemctl status demo-web
# PID berubah = layanan berhasil restart otomatis
```
![namafile](images/t333333.png)
![namafile](images/t3333333.png)
### Langkah 6 — Bersihkan:
```
sudo systemctl disable --now demo-web
sudo rm /etc/systemd/system/demo-web.service
sudo systemctl daemon-reload
```
![namafile](images/t33333333.png)


## Praktikum 10.4 — Filter dan Analisis Log

### Langkah 1 — Log SSH satu jam terakhir:
```
journalctl -u ssh --since "1 hour ago" --no-pager
```
![namafile](images/4satu.png)

### Langkah 2 — Log prioritas error:
```
journalctl -b -p err --no-pager
```
![namafile](images/4dua.png)

### Langkah 3 — Ikuti log real-time:
```
journalctl -u ssh -f
```
![namafile](images/4tiga.png)

### Langkah 4 — Simpan log ke file:
```
cd ~/lab-os/chapter10-services
journalctl -u ssh --since today --no-pager > log-ssh-hari-ini.txt
wc -l log-ssh-hari-ini.txt
grep -i "error\|failed" log-ssh-hari-ini.txt | head -20
```
![namafile](images/4empat.png)

## Tantangan 10.4 — Analisis Error SSH 24 Jam
```
### Simpan error SSH 24 jam terakhir
journalctl -u ssh -p err --since "24 hours ago" --no-pager > error-ssh-24jam.txt

### Hitung total baris error
wc -l error-ssh-24jam.txt

### Tampilkan 10 pesan paling sering muncul
cat error-ssh-24jam.txt | sort | uniq -c | sort -rn | head -10
```
![namafile](images/t4.png)

## Praktikum 10.5 — Konfigurasi SSH Server

### Langkah 1 — Periksa konfigurasi awal:
```
sudo grep -n "^Port\|^#Port" /etc/ssh/sshd_config
ss -tlnp | grep ssh
```
![namafile](images/5satu.png)

### Langkah 2 — Backup dan ubah port:
```
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup.lab12
sudo sed -i 's/^#Port 22/Port 2222/' /etc/ssh/sshd_config
grep "^Port" /etc/ssh/sshd_config
```
![namafile](images/5dua.png)

### Langkah 3 — Validasi dan restart:
```
sudo sshd -t
echo "Kode keluar sshd -t: $?"
sudo systemctl restart ssh
systemctl status ssh
```
![namafile](images/5tiga.png)

### Langkah 4 — Verifikasi port baru:
```
ss -tlnp | grep ssh
ss -tlnp | grep ssh > ~/lab-os/chapter10-services/bukti-port-ssh.txt
cat ~/lab-os/chapter10-services/bukti-port-ssh.txt
```
![namafile](images/5empat.png)

### Langkah 5 — Kembalikan ke port 22:
```
sudo cp /etc/ssh/sshd_config.backup.lab12 /etc/ssh/sshd_config
sudo sshd -t
sudo systemctl restart ssh
ss -tlnp | grep ssh
```
![namafile](images/5lima.png)

## Tantangan 10.5 — Hardening SSH

### Langkah 1 — Backup:
```
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup.tantangan
```
![namafile](images/t5.png)

### Langkah 2 — Edit konfigurasi:
```
sudo nano /etc/ssh/sshd_config

### Tambahkan atau ubah baris berikut:
PermitRootLogin no
MaxAuthTries 3
```
![namafile](images/t55.png)

### Langkah 3 — Validasi sintaks:
```
sudo sshd -t
echo "Kode keluar: $?"
```
![namafile](images/t555.png)

### Langkah 4 — Reload:
```
sudo systemctl reload ssh
```
![namafile](images/t5555.png)

### Langkah 5 — Verifikasi:
```
grep -E "PermitRoot|MaxAuth" /etc/ssh/sshd_config
journalctl -u ssh -n 20
```
![namafile](images/t55555.png)

### Langkah 6 — Kembalikan konfigurasi:
```
sudo cp /etc/ssh/sshd_config.backup.tantangan /etc/ssh/sshd_config
sudo sshd -t
sudo systemctl restart ssh
```
![namafile](images/t555555.png)

## Latihan 10.1 — Audit Layanan dan Analisis Boot

#### 1. Lihat layanan aktif
```
systemctl list-units --type=service --state=running
```
![namafile](images/l1.png)

### Periksa 3 layanan yang dikenal
```
systemctl status ssh
systemctl status cron
systemctl status rsyslog
```
![namafile](images/l11.png)

### 2. Lima layanan terlama saat boot
```
systemd-analyze blame | head -5
```
![namafile](images/l111.png)

### 3. Cek layanan yang gagal
```
systemctl --failed
```
![namafile](images/l1111.png)

### Jika ada yang gagal, cek lognya
```
journalctl -u nama-layanan -n 30
```
![namafile](images/l11111.png)

## Latihan 10.2 — Layanan Kustom dengan Restart Otomatis

### Langkah 1 — Buat skrip monitor-disk.sh:
```
nano ~/lab-os/chapter10-services/monitor-disk.sh
```
![namafile](images/l2.png)

### Ketik isi berikut:
```
#!/bin/bash
LOG="$HOME/lab-os/chapter10-services/disk-usage.log"
while true; do
    echo "[$(date '+%Y-%m-%d %H:%M:%S')]" >> "$LOG"
    df -h >> "$LOG"
    echo "---" >> "$LOG"
    sleep 30
done
```
![namafile](images/l22.png)

### Langkah 2 — Buat berkas unit:
```
sudo nano /etc/systemd/system/monitor-disk.service
```
![namafile](images/l222.png)

### Ketik isi berikut 
```
[Unit]
Description=Monitor Penggunaan Disk
After=network.target

[Service]
Type=simple
User=rizdannn
ExecStart=/rizdannn/lab-os/chapter10-services/monitor-disk.sh
Restart=always
RestartSec=5s

[Install]
WantedBy=multi-user.target
```
![namafile](images/l2222.png)

### Langkah 3 — Aktifkan dan jalankan:
```
sudo systemctl daemon-reload
sudo systemctl enable --now monitor-disk
systemctl status monitor-disk
journalctl -u monitor-disk -f
```
![namafile](images/l22222.png)

### Langkah 4 — Simulasi crash:
```
sudo kill -9 $(systemctl show monitor-disk --property=MainPID --value)
sleep 10
systemctl status monitor-disk
```
![namafile](images/l222222.png)
![namafile](images/l2222222.png)
![namafile](images/l22222222.png)

### Langkah 5 — Bersihkan:
```
sudo systemctl disable --now monitor-disk
sudo rm /etc/systemd/system/monitor-disk.service
sudo systemctl daemon-reload
```
![namafile](images/l222222222.png)


