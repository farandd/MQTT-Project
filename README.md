#  Weather of Malang Raya
**Anggota Kelompok:**

| NRP        | Anggota Kelompok           |
|------------|----------------------------|
| 5027231038 | Dani Wahyu Anak Ary        |
| 5027231084 | Farand Febriansyah         |
| 5027231088 | Veri Rahman                |

##  Deskripsi Singkat

Weather Station adalah proyek IoT berbasis Node.js dan TypeScript yang memantau kondisi lingkungan (suhu, kelembaban, tekanan) secara real-time. Data dikirimkan melalui **MQTT 5.0**, disimpan, dan divisualisasikan dalam bentuk dashboard web dengan berbagai fitur canggih seperti alert, logging, dan konfigurasi threshold.

---

##  Run Project

### 1. **Clone repository**

```bash
git clone 
cd weather-station
```

### 2. **Install dependencies**

```bash
npm install
```

### 3. **Konfigurasi MQTT dan Threshold**

Edit file `config/mqtt.json` dan `config/default.json` sesuai broker & preferensi kamu.

Contoh `config/mqtt.json`:

```json
{
  "host": "mqtt://broker.hivemq.com",
  "port": 1883,
  "username": "your_username",
  "password": "your_password",
  "qos": 1,
  "retain": true,
  "clientId": "weather-station-001",
  "will": {
    "topic": "weather/status",
    "payload": "Sensor mati mendadak!",
    "qos": 1,
    "retain": false
  }
}
```

### 4. **Jalankan aplikasi**

```bash
npm run start
```

### 5. **Akses Dashboard**

Buka browser dan buka:

```
http://localhost:3000
```

---

## 📋 Fitur MQTT 5.0 yang Wajib Didemokan

### 1. **MQTT Basic**

* File: `src/mqtt/mqtt-client-demo.ts`
* Fungsi: Menghubungkan dan menerbitkan data sensor ke broker MQTT.
* Topik contoh: `weather/temperature`, `weather/humidity`, `weather/pressure`.

### 2. **MQTT Secure (TLS/SSL)**

* Ubah `host` di `mqtt.json` ke `mqtts://...`
* Tambahkan TLS options jika menggunakan broker privat (contoh dengan Mosquitto TLS atau HiveMQ Cloud).

### 3. **Authentication**

* Sudah tersedia di `mqtt.json`:

```json
"username": "your_username",
"password": "your_password"
```

### 4. **QoS 0, 1, 2**

* Konfigurasikan `qos` di file `mqtt.json`.
* Lihat efeknya saat koneksi unstable atau client offline.

### 5. **Retained Message**

* Aktifkan `retain: true` untuk menyimpan last value di broker.
* Cek di client subscriber baru: pesan tetap muncul meski publisher sudah mati.

### 6. **Last Will Message**

* Sudah dikonfigurasi di `will` (lihat `mqtt.json`).
* Simulasikan kill process untuk lihat pesan `weather/status` terkirim.

### 7. **Message Expiry**

* Tambahkan properti:

```ts
properties: {
  messageExpiryInterval: 10 // detik
}
```

### 8. **Request-Response**

* Implementasi ada di `mqtt-demo-runner.ts`
* Topik: `weather/request` → `weather/response`
* Client A kirim permintaan → Client B balas data sensor.

### 9. **Flow Control**

* Gunakan:

```ts
properties: {
  receiveMaximum: 2
}
```

* Batasi berapa banyak pesan yang bisa diproses bersamaan.

### 10. **Ping-Pong (Keep Alive)**

* Cek dengan `keepalive: 60` di `mqtt.json`.
* Monitor di broker bahwa koneksi tetap aktif via PINGREQ/PINGRESP.

---

##  Struktur Folder

```
weather-station/
├── config/             # Konfigurasi MQTT & Aplikasi
├── docs/               # Dokumentasi tambahan MQTT
├── public/             # Frontend dashboard
├── src/
│   ├── app.ts          # Entry point aplikasi
│   ├── api/            # API routes dan server
│   ├── data/           # Logging & storage data sensor
│   ├── display/        # UI logika (alerts, dashboard)
│   ├── mqtt/           # MQTT 5 features
│   └── sensors/        # Simulasi data sensor
└── ...
```

---

## 📊 Dashboard

Buka `public/index.html` dan lihat grafik suhu, kelembaban, dan tekanan secara real-time. Script `dashboard.js` akan auto-refresh menggunakan data terbaru dari broker.

---

##  Checklist Demo

| Fitur MQTT        | Status | File                                 |
| ----------------- | ------ | ------------------------------------ |
| MQTT Basic        | ✅      | mqtt-client-demo.ts                  |
| Secure MQTT (TLS) | ✅      | mqtt.json + broker TLS               |
| Authentication    | ✅      | mqtt.json                            |
| QoS 0/1/2         | ✅      | mqtt.json                            |
| Retained Message  | ✅      | mqtt-client-demo.ts                  |
| Last Will Message | ✅      | mqtt.json                            |
| Message Expiry    | ✅      | mqtt-client-demo.ts                  |
| Request/Response  | ✅      | mqtt-demo-runner.ts                  |
| Flow Control      | ✅      | mqtt-client-demo.ts (receiveMaximum) |
| Ping-Pong         | ✅      | mqtt.json (keepalive)                |

---

## 📚 Referensi

* [MQTT 5.0 Spec](https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html)
* [MQTT.js Docs](https://github.com/mqttjs/MQTT.js)
* [HiveMQ Public Broker](https://www.hivemq.com/public-mqtt-broker/)
* [Mosquitto MQTT Broker](https://mosquitto.org/)

---

Kalau kamu butuh file tambahan seperti contoh `mqtt.json` atau kode demo untuk request-response, tinggal bilang ya.
