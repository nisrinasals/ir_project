
# IR Sensor Monitoring System

Proyek ini merupakan sistem pemantauan berbasis Sensor Inframerah (IR) menggunakan mikrokontroler ESP32. Sistem ini dirancang secara komprehensif, mencakup *backend* API untuk pengelolaan data dan *frontend dashboard* untuk visualisasi data sensor secara *real-time*.

## 🚀 Fitur Utama

- **Autentikasi Pengguna**: Sistem *login* yang aman dengan kontrol akses berbasis peran (*Role-Based Access Control* / RBAC).
- **Dasbor Pemantauan**: Antarmuka visual untuk memantau data sensor secara langsung.
- **Panel Administrator**: Halaman khusus untuk manajemen data dan pengguna.
- **Integrasi Perangkat Keras**: Komunikasi data yang mulus dari perangkat ESP32 ke server.
- **Penyimpanan Terpusat**: Pencatatan riwayat data sensor ke dalam basis data untuk keperluan analisis.

## 🏗️ Arsitektur Proyek

Struktur repositori ini dibagi menjadi tiga komponen utama:

```text
IR_Project
├── backend/       # REST API (Node.js + Express)
├── frontend/      # Web dashboard (HTML, CSS, JS)
└── esp32/         # Source code mikrokontroler (Arduino IDE)


## ⚙️ Teknologi yang Digunakan

### Backend
- **Lingkungan**: Node.js
- **Framework**: Express.js
- **Basis Data**: Menyesuaikan konfigurasi (Mendukung MySQL / PostgreSQL / MongoDB)
- **Keamanan**: JSON Web Token (JWT) untuk autentikasi

### Frontend
- **Bahasa**: HTML5, CSS3, JavaScript (ES6)
- **Styling**: Kustom (*Native*, tanpa framework tambahan)

### Perangkat Keras (Hardware)
- Mikrokontroler **ESP32**
- Modul **Sensor IR** (Infrared)

## 📦 Panduan Instalasi dan Pengaturan

### 1. Kloning Repositori
Langkah pertama adalah mengunduh repositori ini ke sistem lokal Anda:
```bash
git clone <url-repositori-anda>
cd ir_project
```

### 2. Pengaturan Backend
Masuk ke direktori *backend* dan instal semua dependensi yang dibutuhkan:
```bash
cd backend
npm install
```
**Konfigurasi Basis Data:**
Buka dan sunting file konfigurasi basis data pada `backend/config/database.js`. Sesuaikan kredensial dengan server basis data yang Anda gunakan.

**Menjalankan Server:**
```bash
npm start
```
*Catatan: Secara bawaan, server akan berjalan pada `http://localhost:3000`.*

### 3. Pengaturan Frontend
Antarmuka pengguna tidak memerlukan proses *build*. Anda dapat langsung membuka file berikut di peramban (browser):
```text
frontend/pages/login.html
```
*Rekomendasi: Gunakan ekstensi seperti Live Server pada Visual Studio Code untuk pengalaman pengembangan yang lebih baik.*

### 4. Pengaturan ESP32
Masuk ke direktori perangkat keras:
```text
esp32/ir_sensor/ir_sensor.ino
```
Buka file tersebut menggunakan **Arduino IDE**. Sebelum mengunggah kode ke ESP32, pastikan Anda mengubah parameter berikut di dalam kode:
- `SSID` dan `Password` jaringan WiFi lokal Anda.
- `URL Endpoint API` dari *backend* yang telah berjalan.

## 📡 Dokumentasi API (Endpoint Bawaan)

Berikut adalah daftar antarmuka komunikasi (API) yang tersedia pada sistem:

| Metode | Endpoint      | Deskripsi |
| :--- | :--- | :--- |
| `POST` | `/auth/login` | Autentikasi dan *login* pengguna |
| `GET`  | `/sensor`     | Mengambil riwayat dan data sensor terbaru |
| `POST` | `/sensor`     | Menerima transmisi data dari perangkat ESP32 |
| `GET`  | `/admin`      | Mengambil data khusus administrator |

## 👤 Hak Akses Pengguna (Role)

- **Administrator**: Memiliki akses penuh terhadap dasbor, termasuk manajemen data sensor dan pengelolaan akun pengguna.
- **User (Pengguna Standar)**: Memiliki akses terbatas, difokuskan pada pemantauan dan melihat visualisasi data sensor.

## 📊 Alur Kerja Sistem

1. Perangkat **ESP32** membaca input fisik dari Sensor IR.
2. ESP32 mentransmisikan data tersebut ke **Backend** melalui protokol HTTP (*API Endpoint*).
3. **Backend** memproses dan menyimpan log data ke dalam **Basis Data**.
4. **Frontend** melakukan permintaan (*request*) data terbaru dari Backend.
5. Data divisualisasikan dan disajikan kepada pengguna melalui **Dasbor Pemantauan**.

## 🛠️ Struktur File Utama

### Backend
- `server.js` : *Entry point* dari aplikasi server.
- `models/` : Definisi skema dan struktur tabel basis data.
- `routes/` : Deklarasi rute dan *endpoint* API.
- `middleware/`: Logika validasi token JWT dan pengecekan otorisasi peran (*role*).

### Frontend
- `pages/dashboard.html`: Halaman utama untuk visualisasi data sensor.
- `pages/admin.html`: Panel kontrol administrator.
- `pages/login.html`: Halaman autentikasi gerbang masuk.

### ESP32
- `ir_sensor.ino` : Skrip utama operasional mikrokontroler.

## Dokumentasi

| Status | Tampilan / Tangkapan Layar |
| :---: | :--- |
| **Detected** | <img width="1280" height="960" alt="detected" src="https://github.com/user-attachments/assets/987f6f95-bc9f-41eb-ad26-b40b6953797b" /> |
| **Idle** | <img width="1280" height="960" alt="idle" src="https://github.com/user-attachments/assets/9ebd5801-b0d3-4b62-87a6-bd227ba1abc9" /> |
| **Admin Panel** | <img width="1920" height="1080" alt="Screenshot 2026-01-16 165211" src="https://github.com/user-attachments/assets/a0df3253-a42f-4e18-9844-9523c6248b84" /> |
| **Object Detected** | <img width="1920" height="1080" alt="Screenshot 2026-01-16 165107" src="https://github.com/user-attachments/assets/6cd60ff7-34b5-4417-a29e-1cd4a9f08ea7" /> |
| **No Object** | <img width="1920" height="1080" alt="Screenshot 2026-01-16 165117" src="https://github.com/user-attachments/assets/cb39304b-36ad-476a-98d5-2ced15c816c1" /> |





## ⚠️ Catatan Penting

- **Prioritas Server**: Pastikan server *backend* sudah berjalan secara normal sebelum ESP32 diaktifkan agar tidak terjadi kegagalan transmisi data (*timeout*).
- **Konektivitas**: Pastikan jaringan WiFi memiliki akses internet/intranet yang stabil untuk ESP32.
- **Pengujian**: Disarankan untuk menggunakan aplikasi seperti **Postman** atau **Insomnia** untuk menguji fungsionalitas API sebelum menghubungkannya ke *frontend* atau ESP32.
