# 🏫 Occupancy-Aware Smart Classroom (Project 09)

Hệ thống phòng học thông minh tự động điều khiển thiết bị (Đèn & Quạt) dựa trên cảm biến hiện diện kép, tối ưu hóa năng lượng tiêu thụ và duy trì tiện nghi môi trường.

---

## 🏗️ Kiến Trúc Hệ Thống (End-to-End Architecture)

1. **Firmware (ESP32 DevKit V1):**
   - **Hiện diện kép (Dual-Tech):** PIR HC-SR501 (GPIO 12) + Radar RCWL-0516 (GPIO 13).
   - **Môi trường:** DHT22 (GPIO 14), BH1750 (I2C SDA 21, SCL 22, Address `0x23`).
   - **Đo điện năng:** INA219 (I2C SDA 21, SCL 22, Address `0x40`).
   - **Chấp hành:** Relay 2 Kênh 5V (GPIO 25 điều khiển Đèn, GPIO 26 điều khiển Quạt) có bảo vệ bởi Diode Schottky 1N5819.
2. **Network Broker:** HiveMQ Public MQTT Broker (`broker.hivemq.com:1883` / `wss://broker.hivemq.com:8884/mqtt`).
3. **Backend Service:** Node.js Express REST API & MQTT Subscriber (Deploy trên Render.com).
4. **Database:** Neon Serverless PostgreSQL Database.
5. **Frontend Web Dashboard:** Web Realtime Chart.js & Control Panel (Deploy trên Vercel.com).

---

## 📂 Cấu Trúc Thư Mục GitHub

```text
smart_classroom_github_repo/
├── .gitignore
├── .env.example
├── .env
├── README.md
├── firmware_esp32/             # Mã nguồn C++ PlatformIO cho ESP32
│   ├── platformio.ini
│   └── src/
│       └── main.cpp
├── backend_render/             # Server Node.js Express API & MQTT Consumer
│   ├── package.json
│   ├── server.js
│   ├── .env
│   └── .env.example
├── frontend_vercel/            # Web Dashboard Realtime HTML/JS
│   ├── index.html
│   └── vercel.json
└── database_neon/              # Script khởi tạo Database PostgreSQL
    └── schema.sql
```

---

## 🚀 Hướng Dẫn Triển Khai (Deployment Guide)

### 1. Database (Neon.tech)
1. Đăng nhập vào [Neon.tech](https://neon.tech) và tạo dự án mới.
2. Mở **SQL Editor** và chạy toàn bộ lệnh trong file `database_neon/schema.sql`.
3. Sao chép chuỗi `DATABASE_URL`.

### 2. Backend (Render.com)
1. Tạo Web Service mới trên [Render.com](https://render.com) trỏ đến thư mục `backend_render`.
2. Khai báo biến môi trường (Environment Variables):
   - `DATABASE_URL`: Chuỗi kết nối Neon DB.
   - `MQTT_BROKER_URL`: `mqtt://broker.hivemq.com:1883`.

### 3. Frontend Web (Vercel.com)
1. Tạo Project mới trên [Vercel.com](https://vercel.com) trỏ đến thư mục `frontend_vercel`.
2. Trỏ **Root Directory** vào `frontend_vercel` (hoặc upload duy nhất file `index.html` + `vercel.json`).

### 4. Firmware (ESP32 PlatformIO)
1. Mở thư mục `firmware_esp32` bằng VS Code (đã cài extension PlatformIO).
2. Chỉnh sửa tên Wi-Fi và Mật khẩu trong file `src/main.cpp`.
3. Bấm **Upload** để nạp chương trình vào bo mạch ESP32.
