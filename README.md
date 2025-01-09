# Soal Nomor 1: IP Address dan DHCP Server

## Langkah 1: Konfigurasi IP Address pada Interface
### Menambahkan IP Address pada Interface
1. Tentukan interface yang akan digunakan:
   - **ether1** untuk koneksi internet.
   - **ether2** untuk jaringan lokal (klien).
2. Ketik perintah berikut untuk menambahkan IP address pada **ether2**:

```bash
/ip address add address=192.168.88.1/24 interface=ether2
```

## Langkah 2: Buat DHCP Server
### Buat Pool IP Address
Tentukan rentang IP yang akan didistribusikan ke klien, misalnya 10 IP (192.168.88.2–192.168.88.11):

```bash
/ip pool add name=dhcp_pool ranges=192.168.88.2-192.168.88.11
```

### Tambahkan DHCP Server pada Interface
Tambahkan DHCP Server untuk mendistribusikan IP dari pool yang sudah dibuat:

```bash
/ip dhcp-server add name=dhcp1 interface=ether2 address-pool=dhcp_pool
```

### Tambahkan Jaringan untuk DHCP Server
Tentukan gateway dan DNS untuk klien:

```bash
/ip dhcp-server network add address=192.168.88.0/24 gateway=192.168.88.1 dns-server=8.8.8.8
```

## Langkah 3: Verifikasi Konfigurasi
### Cek IP Address yang Ditambahkan
Periksa IP yang sudah Anda tambahkan:

```bash
/ip address print
```

### Cek DHCP Server
Pastikan DHCP Server sudah aktif:

```bash
/ip dhcp-server print
```

### Cek Daftar Klien DHCP
Untuk memeriksa perangkat yang terhubung dan mendapatkan IP:

```bash
/ip dhcp-server lease print
```

## Cara Testing
### Langkah 1: Hubungkan Perangkat Klien ke Interface ether2
1. Hubungkan perangkat klien (misalnya komputer atau perangkat lain) ke interface **ether2** pada MikroTik.
2. Pastikan perangkat klien diset untuk DHCP agar menerima IP otomatis.

### Langkah 2: Cek IP yang Diberikan oleh DHCP
#### Pada Komputer Klien (Windows)
Buka Command Prompt dan ketik perintah berikut:

```bash
ipconfig
```
Pastikan IP yang diberikan berada dalam rentang yang sudah Anda tentukan di pool (misalnya 192.168.88.2–192.168.88.11).

#### Pada Komputer Klien (Linux/Mac)
Buka Terminal dan ketik:

```bash
ifconfig
```
Pastikan IP yang diterima berada dalam rentang yang telah Anda tentukan.

### Langkah 3: Tes Koneksi Lokal
Tes ping ke gateway MikroTik (**192.168.88.1**):

```bash
ping 192.168.88.1
```
Jika ping berhasil, berarti perangkat klien terhubung ke MikroTik.

### Langkah 4: Tes Koneksi Internet
Tes ping ke internet dengan mencoba alamat seperti **8.8.8.8** (Google DNS):

```bash
ping 8.8.8.8
```
Jika ping ke **8.8.8.8** berhasil, berarti perangkat klien dapat mengakses internet melalui MikroTik.

---

Dengan langkah-langkah di atas, konfigurasi IP Address dan DHCP Server Anda seharusnya sudah berjalan dengan baik. Jika Anda menemui kendala, silakan periksa konfigurasi dan log di MikroTik Anda.
