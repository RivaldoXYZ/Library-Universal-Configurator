# Dokumentasi Fungsi Library Universal Configurator (UCFG)

Library Universal Configurator (UCFG) adalah pustaka untuk ESP32 yang memudahkan pengaturan parameter perangkat secara dinamis melalui Bluetooth Low Energy (BLE) dan penyimpanan di Preferences. Berikut dokumentasi lengkap fungsi-fungsi utama dalam library ini beserta contoh penggunaannya.

---

## Daftar Fungsi dan Penjelasan

### 1. `generateUUID()`
**Tipe:** `String UniversalConfigurator::generateUUID()`  
**Deskripsi:**  
Membuat string UUID acak sesuai standar UUID, digunakan untuk BLE service/characteristic agar selalu unik pada setiap perangkat.

---

### 2. `getOrCreateUUID(const String& key)`
**Tipe:** `String UniversalConfigurator::getOrCreateUUID(const String& key)`  
**Deskripsi:**  
Mengambil UUID dari Preferences dengan nama key tertentu. Jika belum ada, akan dibuat UUID baru dan disimpan ke Preferences.
- **key**: Nama key UUID yang ingin diambil/dibuat (contoh: `"serviceUUID"`, `"characteristicUUID"`).

---

### 3. `saveUUID(const String& key, const String& uuid)`
**Tipe:** `void UniversalConfigurator::saveUUID(const String& key, const String& uuid)`  
**Deskripsi:**  
Menyimpan UUID ke Preferences pada key tertentu.

---

### 4. `initBLE(const String& deviceName)`
**Tipe:** `void UniversalConfigurator::initBLE(const String& deviceName)`  
**Deskripsi:**  
Menginisialisasi BLE dengan nama perangkat yang diinginkan.  
Membuat BLE Service dan Characteristic secara otomatis dengan UUID unik.  
**Wajib** dipanggil di `setup()` sebelum menggunakan fitur BLE.

---

### 5. `sendConfig()`
**Tipe:** `void UniversalConfigurator::sendConfig()`  
**Deskripsi:**  
Mengirimkan data konfigurasi (JSON) terbaru ke aplikasi BLE yang terhubung.  
Perlu dipanggil setiap kali ingin mengupdate aplikasi mobile/klien.

---

### 6. `saveToPreferences(const String& key, const String& jsonData)`
**Tipe:** `void UniversalConfigurator::saveToPreferences(const String& key, const String& jsonData)`  
**Deskripsi:**  
Menyimpan data string (umumnya JSON) ke Preferences pada key tertentu.  
Digunakan untuk menyimpan konfigurasi atau data lain secara persisten.

---

### 7. `readFromPreferences(const String& key)`
**Tipe:** `String UniversalConfigurator::readFromPreferences(const String& key)`  
**Deskripsi:**  
Membaca data string dari Preferences pada key tertentu.

---

### 8. `readOrInitPreferences()`
**Tipe:** `String UniversalConfigurator::readOrInitPreferences()`  
**Deskripsi:**  
Membaca konfigurasi utama (key `"config"`) dari Preferences.  
Jika belum ada, akan dibuat default (`{}`) secara otomatis.

---

### 9. `initConfig(const String& param, const String& value, const String& description, const String& type)`
**Tipe:** `void UniversalConfigurator::initConfig(const String& param, const String& value, const String& description, const String& type)`  
**Deskripsi:**  
Mendaftarkan atau memperbarui parameter konfigurasi yang bisa diubah lewat BLE/app.
- **param**: Nama parameter (misal: `"ipmqtt"`)
- **value**: Nilai awal parameter
- **description**: Deskripsi parameter (akan tampil di aplikasi)
- **type**: Tipe data parameter (`"int"`, `"string"`, `"ip"`, `"float"`, dll)

---

### 10. `clearPreferences()`
**Tipe:** `void UniversalConfigurator::clearPreferences()`  
**Deskripsi:**  
Menghapus seluruh data konfigurasi di Preferences.  
**Hati-hati:** Data yang sudah dihapus tidak bisa dikembalikan!  
Biasanya hanya digunakan saat develop/testing.

---

### 11. `getConfig()`
**Tipe:** `String UniversalConfigurator::getConfig()`  
**Deskripsi:**  
Mengambil seluruh konfigurasi dalam format JSON dari Preferences.  
Jika ada data baru dari BLE, data ini akan di-update dan disimpan.

---

### 12. `parseConfig()`
**Tipe:** `void UniversalConfigurator::parseConfig()`  
**Deskripsi:**  
Membaca data konfigurasi JSON dari Preferences dan menyimpannya ke dalam map internal (`configMap`).  
Wajib dipanggil sebelum mengambil value parameter dengan `getConfigValue`.

---

### 13. `getConfigValue(const String& param)`
**Tipe:** `String UniversalConfigurator::getConfigValue(const String& param)`  
**Deskripsi:**  
Mengambil nilai parameter tertentu dari `configMap` (hasil parsing config).  
- **param**: Nama parameter yang ingin diambil nilainya.

---

### 14. `isDeviceConnected()`
**Tipe:** `bool UniversalConfigurator::isDeviceConnected()`  
**Deskripsi:**  
Mengembalikan status koneksi BLE (true jika aplikasi BLE terhubung ke perangkat).

---

## Contoh Program Sederhana

```c++
#include "UCFG.h"

UniversalConfigurator ucfg;

// Struct untuk menampung seluruh parameter
struct Config {
    int port;
    String ipmqtt;
    String usernamemqtt;
    String passwordmqtt;
    float Threshold;
} config;

// Fungsi untuk refresh config dari Preferences ke variabel program
void refreshConfig(Config &cfg) {
    ucfg.parseConfig();
    cfg.port = ucfg.getConfigValue("port").toInt();
    cfg.ipmqtt = ucfg.getConfigValue("ipmqtt");
    cfg.usernamemqtt = ucfg.getConfigValue("usernamemqtt");
    cfg.passwordmqtt = ucfg.getConfigValue("passwordmqtt");
    cfg.Threshold = ucfg.getConfigValue("Threshold").toFloat();
}

// Fungsi untuk menampilkan config ke Serial Monitor
void printConfig(const Config &cfg) {
    Serial.print("port: "); Serial.println(cfg.port);
    Serial.print("ipmqtt: "); Serial.println(cfg.ipmqtt);
    Serial.print("usernamemqtt: "); Serial.println(cfg.usernamemqtt);
    Serial.print("passwordmqtt: "); Serial.println(cfg.passwordmqtt.length() > 0 ? "****" : "(not set)");
    Serial.print("Threshold: "); Serial.println(cfg.Threshold);
    Serial.println();
}

void setup() {
    Serial.begin(115200);
    ucfg.initBLE("Seminar TA 1");
    // ucfg.clearPreferences(); // Aktifkan hanya jika ingin reset semua config

    // Inisialisasi/config parameter
    ucfg.initConfig("port", String(1845), "Port Node Red", "int");
    ucfg.initConfig("ipmqtt", "192.168.1.1", "IP address MQTT Server", "ip");
    ucfg.initConfig("usernamemqtt", "admin", "Username MQTT", "string");
    ucfg.initConfig("passwordmqtt", "admin", "Password MQTT", "string");
    ucfg.initConfig("Threshold", String(88), "Threshold sistem pengairan", "float");

    refreshConfig(config);
    printConfig(config);
}

void loop() {
    if (ucfg.isDeviceConnected()) {
        Serial.println("Device is connected!");
        ucfg.sendConfig();
        refreshConfig(config);
        printConfig(config);
    } else {
        Serial.println("Device is not connected.");
    }
    delay(3000);
}
```

---

## Best Practice

- **Selalu panggil `parseConfig()` sebelum membaca nilai parameter.**
- **Gunakan `sendConfig()` untuk mengirimkan update ke aplikasi BLE.**
- **Jangan lupa inisialisasi BLE dan parameter di `setup()`.**
- **Simpan dan baca konfigurasi hanya melalui fungsi yang disediakan agar data konsisten.**
- **Jangan gunakan `clearPreferences()` di deployment kecuali benar-benar ingin reset.**

---

## FAQ

**Q: Apakah data akan hilang jika perangkat mati?**  
A: Tidak. Data konfigurasi disimpan di Preferences (memori non-volatile) ESP32.

**Q: Bagaimana jika ingin menambah parameter baru?**  
A: Cukup panggil `initConfig()` dengan nama dan nilai parameter baru di kode Anda.

**Q: Bagaimana cara reset semua konfigurasi?**  
A: Panggil `clearPreferences()` di `setup()` (jangan lupa dikomentari/dimatikan lagi setelah reset).

---

## Kontak dan Dukungan

Untuk pertanyaan, bug, atau kontribusi, silakan buat Issue di repository GitHub ini.
