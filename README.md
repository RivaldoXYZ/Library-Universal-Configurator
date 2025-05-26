# Universal Configurator Library (UCFG) untuk ESP32

**Universal Configurator** adalah library Arduino yang memudahkan Anda mengatur parameter konfigurasi ESP32 melalui Bluetooth (BLE). Cocok untuk proyek IoT, smart home, dan aplikasi lain yang membutuhkan pengaturan parameter tanpa harus memprogram ulang kode.

---

## Fitur Utama

- **Pengaturan parameter lewat aplikasi BLE (Bluetooth Low Energy)**
- **Penyimpanan konfigurasi di memori internal ESP32 (Preferences)**
- **Mudah digunakan, tanpa repot memahami detail BLE**
- **Mendukung berbagai tipe data (int, float, string, ip, dll.)**
- **Aman, data tetap tersimpan meski board dimatikan**

---

## Contoh Penggunaan Singkat

### 1. Inisialisasi Library

```c++
#include "UCFG.h"
UniversalConfigurator ucfg;
```

### 2. Inisialisasi Bluetooth & Konfigurasi Awal

```c++
void setup() {
    Serial.begin(115200);
    ucfg.initBLE("NamaPerangkatBLE"); // Nama BLE sesuai keinginan

    // Deklarasi parameter yang bisa diubah lewat aplikasi
    ucfg.initConfig("PIN", String(1234), "PIN autentikasi", "int");
    ucfg.initConfig("ipmqtt", "192.168.1.1", "IP MQTT Server", "ip");
    ucfg.initConfig("usernamemqtt", "admin", "Username MQTT", "string");
    ucfg.initConfig("passwordmqtt", "admin", "Password MQTT", "string");
    ucfg.initConfig("Threshold", String(88), "Ambang sistem", "float");
}
```

### 3. Mengambil Nilai Konfigurasi

Selalu refresh data jika ingin nilai terbaru:
```c++
void loop() {
    if (ucfg.isDeviceConnected()) {
        ucfg.sendConfig();    // Kirim konfigurasi ke aplikasi BLE
        ucfg.parseConfig();   // Refresh config dari memori
        // Ambil data:
        int pin = ucfg.getConfigValue("PIN").toInt();
        String ip = ucfg.getConfigValue("ipmqtt");
        // dst...
    }
    delay(3000);
}
```

---

## Penjelasan Fungsi Utama

| Fungsi                              | Keterangan                                                                 |
|--------------------------------------|----------------------------------------------------------------------------|
| `initBLE(namaPerangkat)`             | Mengaktifkan BLE dengan nama tertentu                                      |
| `initConfig(nama, nilai, desc, tipe)`| Menambah/ubah parameter konfigurasi                                        |
| `sendConfig()`                       | Mengirim data konfigurasi ke aplikasi BLE                                  |
| `parseConfig()`                      | Membaca konfigurasi dari memori dan memperbarui map internal               |
| `getConfigValue(nama)`               | Mengambil nilai parameter tertentu                                         |
| `saveToPreferences(key, data)`       | Menyimpan data ke memori internal (advanced)                               |
| `readFromPreferences(key)`           | Membaca data dari memori internal (advanced)                               |
| `clearPreferences()`                 | Menghapus seluruh konfigurasi di memori (reset config)                     |
| `isDeviceConnected()`                | Mengecek status koneksi BLE                                                |

---

## Alur Penggunaan

1. **Inisialisasi BLE dan parameter di setup()**
2. **Koneksi aplikasi BLE ke ESP32**
3. **Ubah parameter lewat aplikasi**
4. **Panggil `ucfg.parseConfig()` setiap ingin mengambil data terbaru**
5. **Ambil nilai parameter dengan `ucfg.getConfigValue()`**

---

## Tips Penggunaan Aman

- Jangan tampilkan password di Serial Monitor.
- Hindari memanggil `clearPreferences()` di setup kecuali ingin reset total data.
- Selalu cek apakah parameter ada sebelum dikonversi ke int/float.
- Bisa menambah parameter baru tanpa perlu mengubah aplikasi BLE.

---

## Contoh Kode Lengkap

```c++
#include "UCFG.h"
UniversalConfigurator ucfg;

void setup() {
    Serial.begin(115200);
    ucfg.initBLE("MyDevice");
    ucfg.initConfig("PIN", String(1234), "PIN autentikasi", "int");
    ucfg.initConfig("ipmqtt", "192.168.1.1", "IP MQTT Server", "ip");
    ucfg.initConfig("usernamemqtt", "admin", "Username MQTT", "string");
    ucfg.initConfig("passwordmqtt", "admin", "Password MQTT", "string");
    ucfg.initConfig("Threshold", String(88), "Ambang sistem", "float");
}

void loop() {
    if (ucfg.isDeviceConnected()) {
        ucfg.sendConfig();
        ucfg.parseConfig();

        int pin = ucfg.getConfigValue("PIN").toInt();
        String ip = ucfg.getConfigValue("ipmqtt");
        String user = ucfg.getConfigValue("usernamemqtt");
        String pass = ucfg.getConfigValue("passwordmqtt");
        float th = ucfg.getConfigValue("Threshold").toFloat();

        Serial.print("PIN: "); Serial.println(pin);
        Serial.print("IP MQTT: "); Serial.println(ip);
        Serial.print("Username: "); Serial.println(user);
        Serial.print("Password: "); Serial.println(pass.length() > 0 ? "****" : "(not set)");
        Serial.print("Threshold: "); Serial.println(th);
    }
    delay(3000);
}
```

---

## FAQ

**Q: Apakah library ini hanya untuk ESP32?**  
A: Ya, library ini dirancang khusus untuk ESP32 dengan BLE.

**Q: Apakah harus menggunakan aplikasi khusus di HP?**  
A: Bisa menggunakan aplikasi BLE scanner umum, tetapi lebih optimal dengan aplikasi Universal Configurator.

**Q: Apakah data hilang jika board dimatikan?**  
A: Tidak. Data disimpan di memori internal (Preferences).

---

## Kontribusi & Dukungan

Silakan buat issue di GitHub jika menemukan bug atau ingin request fitur!
