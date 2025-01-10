## **Soal Nomor 1: Konfigurasi IP Address, DHCP Server MikroTik dan Verifikasi di AntiX Linux**

### **Langkah 1: Konfigurasi IP Address pada MikroTik**
1. **Tambahkan IP Address pada Interface yang akan digunakan untuk jaringan lokal (klien):**
   - Gunakan **ether1** untuk jaringan lokal.
   - Tambahkan IP Address **192.168.0.45/24** pada interface **ether1**:
     ```bash
     /ip address add address=192.168.0.45/24 interface=ether1
     ```

2. **Verifikasi IP Address yang telah ditambahkan:**
   ```bash
   /ip address print
   ```

---

### **Langkah 2: Konfigurasi DHCP Server MikroTik**
1. **Buat Pool IP Address**  
   Tentukan rentang IP yang akan didistribusikan oleh DHCP Server. Misalnya, gunakan rentang **192.168.0.46–192.168.0.55**:
   ```bash
   /ip pool add name=dhcp_pool ranges=192.168.0.46-192.168.0.55
   ```

2. **Tambahkan DHCP Server pada Interface (ether1):**  
   Hubungkan DHCP Server dengan pool yang telah dibuat:
   ```bash
   /ip dhcp-server add name=dhcp1 interface=ether1 address-pool=dhcp_pool
   ```

3. **Konfigurasi Network untuk DHCP Server:**  
   Tentukan gateway dan DNS untuk klien:
   ```bash
   /ip dhcp-server network add address=192.168.0.0/24 gateway=192.168.0.45 dns-server=8.8.8.8
   ```

4. **Verifikasi DHCP Server:**
   ```bash
   /ip dhcp-server print
   ```

---

### **Langkah 3: Verifikasi Konfigurasi MikroTik dengan AntiX Linux**
#### **Konfigurasi Jaringan di AntiX**
1. **Aktifkan DHCP Client pada AntiX Linux:**
   Jalankan perintah berikut di terminal:
   ```bash
   sudo dhclient -r eth0 && sudo dhclient eth0
   ```

2. **Cek apakah IP Address telah diterima:**
   Gunakan perintah berikut untuk melihat IP Address:
   ```bash
   ifconfig
   ```
   Atau:
   ```bash
   ip a
   ```

3. **Pastikan perangkat mendapatkan IP dalam rentang yang ditentukan (192.168.0.46–192.168.0.55).**

---

### **Langkah 4: Uji Koneksi**
#### **Tes Koneksi ke Gateway**
Ping ke gateway MikroTik (**192.168.0.45**):
```bash
ping 192.168.0.45
```

#### **Tes Koneksi Internet**
Ping ke Google DNS (**8.8.8.8**):
```bash
ping 8.8.8.8
```

#### **Tes Resolusi DNS**
Ping ke nama domain (misalnya **google.com**) untuk memastikan fungsi DNS:
```bash
ping google.com
```

---

### **Langkah 5: Memeriksa Status DHCP Lease**
1. **Cek Status DHCP Lease di MikroTik**  
   Periksa status dari perangkat yang terhubung di DHCP Server:
   ```bash
   /ip dhcp-server lease print
   ```

2. **Penjelasan Status pada DHCP Lease**:
   - **Offered**: DHCP Server telah menawarkan alamat IP kepada perangkat, tetapi perangkat belum menerima atau mengonfirmasi IP tersebut.
   - **Bound**: DHCP Server telah menawarkan dan perangkat telah berhasil menerima IP. Ini menandakan bahwa DHCP server berfungsi dengan baik.
   - **Expired**: Lease IP telah kadaluarsa dan perangkat belum memperbarui atau memperpanjangnya.
   - **Released**: Perangkat telah melepaskan IP-nya dan tidak lagi menggunakannya.
   - **Rejected**: DHCP Server menolak permintaan klien untuk mendapatkan IP.

   Jika status perangkat adalah **"Bound"**, berarti DHCP Server berfungsi dengan baik dan perangkat telah berhasil mendapatkan IP.

---
