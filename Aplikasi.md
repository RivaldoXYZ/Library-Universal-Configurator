# Penjelasan Universal Configurator BLE

Universal Configurator BLE adalah aplikasi yang dirancang untuk memudahkan pengguna dalam melakukan pengaturan, pemantauan, dan kustomisasi perangkat-perangkat berbasis Bluetooth Low Energy (BLE). Aplikasi ini mengintegrasikan library BLE yang andal untuk menangani proses komunikasi nirkabel antara smartphone dan perangkat BLE, sehingga semua proses scanning, koneksi, dan konfigurasi dapat berlangsung secara seamless dan stabil.

Aplikasi ini sangat bergantung pada library BLE yang digunakan, di mana library tersebut berperan sebagai jembatan antara aplikasi dan perangkat keras BLE. Melalui integrasi ini, aplikasi dapat melakukan berbagai operasi seperti scanning perangkat di sekitar, melakukan koneksi, otentikasi PIN, serta mengirim dan menerima data konfigurasi ke/dari perangkat BLE.

## Fitur dan Fungsionalitas

### 1. Pemindaian Perangkat BLE (Scan Devices)
- Aplikasi secara otomatis atau melalui satu ketukan tombol (ikon kaca pembesar) akan mencari perangkat BLE yang aktif di sekitar Anda.
- Setiap perangkat yang terdeteksi akan tampil pada daftar **Available Devices**, lengkap dengan:
  - Nama perangkat (jika tersedia)
  - Alamat MAC unik
  - Indikator kekuatan sinyal (RSSI), misal: -59, -87
- Semua data pemindaian ini diperoleh dan diproses melalui integrasi dengan library BLE.

### 2. Koneksi ke Perangkat (Connect to Device)
- Pengguna dapat memilih salah satu perangkat dari daftar **Available Devices** untuk mencoba menghubungkannya.
- Bila koneksi berhasil, perangkat tersebut akan diberi tanda khusus dan biasanya dipindahkan ke bagian **Remembered Devices** (Perangkat yang Diingat).
- Proses koneksi dan pemeliharaan status koneksi sepenuhnya di-handle oleh library BLE sehingga koneksi tetap stabil dan responsif.

### 3. Manajemen Perangkat yang Diingat (Remembered Devices)
- Perangkat-perangkat yang sering digunakan dapat disimpan ke daftar **Remembered Devices** untuk akses cepat di masa mendatang.
- Untuk setiap perangkat yang diingat, tersedia beberapa opsi:
  - **Konfigurasi (Settings/Gear Icon):** Akses halaman pengaturan khusus perangkat.
  - **Putuskan Koneksi (Disconnect/Red Icon):** Mengakhiri sesi koneksi tanpa menghapus perangkat dari daftar. Akan muncul dialog konfirmasi sebelum memutuskan koneksi.
  - **Lupakan Perangkat (Forget Device/Trash Icon):** Menghapus perangkat dari daftar "Remembered Devices" dengan konfirmasi terlebih dahulu.
- Semua aksi ini dikendalikan secara real-time berkat integrasi dengan library BLE.

### 4. Autentikasi PIN (PIN Authentication)
- Sebelum masuk ke halaman konfigurasi perangkat, pengguna diwajibkan memasukkan PIN (biasanya 4 digit).
- PIN ini digunakan untuk memastikan hanya pengguna yang berwenang yang dapat mengubah pengaturan perangkat.

### 5. Konfigurasi Parameter Perangkat (Device Configuration)
- Setelah autentikasi PIN berhasil ("PIN accepted. Access granted."), pengguna diarahkan ke halaman **Device Configuration**.
- Di halaman ini, pengguna dapat melihat dan mengubah berbagai parameter perangkat, antara lain:
  - **PIN untuk autentikasi:** Mengatur ulang PIN perangkat.
  - **IP address MQTT Server:** Menentukan alamat IP server MQTT untuk komunikasi perangkat.
  - **Username & Password MQTT:** Mengatur kredensial untuk koneksi MQTT.
  - **Threshold sistem pengairan:** Mengatur nilai ambang sistem (dapat berupa integer, float, string, atau IPv4).
- Setiap parameter dapat diedit dengan mengetuk ikon pensil, lalu menginput nilai baru dan menekan "OK".
- Setelah semua perubahan dilakukan, pengguna harus menekan tombol **Save** untuk mengirim dan menyimpan konfigurasi baru ke perangkat melalui koneksi BLE.

## Tujuan Penggunaan Aplikasi

Universal Configurator BLE bertujuan menjadi antarmuka yang mudah digunakan dan efisien bagi siapa saja yang ingin melakukan pengaturan atau kustomisasi perangkat keras berbasis BLE. Aplikasi ini sangat berguna untuk:
- Pengembang (developer) yang ingin mempercepat proses konfigurasi perangkat.
- Teknisi yang melakukan instalasi dan pemeliharaan perangkat-perangkat BLE.
- Pengguna akhir yang ingin menyesuaikan fungsi perangkatnya tanpa harus menggunakan alat konfigurasi khusus yang rumit.

Dengan adanya integrasi langsung ke library BLE, aplikasi ini menjamin komunikasi yang handal, pengelolaan koneksi yang mudah, serta pengalaman pengguna yang intuitif dalam mengelola perangkat BLE.

---

**Catatan:**  
Integrasi dengan library BLE menjadi inti dari aplikasi ini, memastikan bahwa seluruh fungsi (scanning, koneksi, pengiriman/penyimpanan konfigurasi) berjalan mulus dan aman.
