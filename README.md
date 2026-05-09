# ⚡ WATT·IQ — Smart Energy Management Platform

> **Measure Smarter. Manage Better.**  
> วัดให้แม่น · รู้ให้ทัน · จัดการพลังงานอย่างชาญฉลาด

[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-Live-brightgreen?logo=github)](https://username.github.io/wattiq)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Stack](https://img.shields.io/badge/Stack-Selec%20%2B%20Messung%20%2B%20CODESYS%20%2B%20MQTT%20%2B%20Grafana-orange)](https://github.com/username/wattiq)

---

## 📖 เกี่ยวกับโครงการ

**WATT·IQ** คือแพลตฟอร์มการเรียนรู้แบบเปิด (Open Learning Platform) สำหรับระบบบริหารจัดการพลังงานอัจฉริยะ (Smart Energy Management System) โดยเชื่อมโยง Hardware อุตสาหกรรมจริงกับเทคโนโลยี IIoT และ Cloud Analytics

เนื้อหาทั้งหมดพัฒนาจาก **Prototype จริง** ที่ประกอบด้วย:

| Layer | Technology |
|-------|-----------|
| 🔌 Field | Selec EM2M-96 Energy Meter + CT Sensor 100A/5A |
| ⚙️ Edge/PLC | Messung NX-ERA PLC + CODESYS IEC 61131-3 |
| 🌐 Protocol | RS485 Modbus RTU → MQTT (Mosquitto) |
| 💾 Storage | InfluxDB 2 Time-Series Database |
| 📊 Visualization | Grafana Dashboard (embed-ready) |
| 🧠 AI | Python ML — Anomaly Detection + Demand Forecasting |

---

## 🎯 กลุ่มเป้าหมาย

- 🎓 **นักศึกษา** ปวส. – ปริญญาตรี สาขาไฟฟ้า/อัตโนมัติ
- 👨‍🏫 **อาจารย์** ที่ต้องการสื่อการสอนพร้อมใช้พร้อม Prototype จริง
- 🏭 **วิศวกร** ในอุตสาหกรรมที่ต้องการลดต้นทุนพลังงาน
- 🏢 **SME** ที่สนใจระบบ Smart Energy Management

---

## 📁 โครงสร้างไฟล์

```
wattiq/
│
├── index.html                  ← หน้าหลัก (B3_all_pages.html)
├── B1_hero_banner.html         ← Hero Banner embed-ready
├── B2_logo_assets.svg          ← Logo ทุก Variant + Color System
├── README.md                   ← ไฟล์นี้
│
├── infrastructure/
│   ├── C1_nodered_flow.json    ← Node-RED: Modbus → MQTT bridge
│   ├── C2_docker_compose.yml   ← Full stack: Mosquitto+InfluxDB+Grafana
│   └── C2_config_notes.txt     ← Config Mosquitto + Telegraf
│
├── codesys/
│   └── EnergyMonitor.project   ← CODESYS project (Messung NX-ERA)
│
├── ai/
│   ├── D1_anomaly_detection.py ← Isolation Forest บน InfluxDB data
│   └── D2_lstm_forecasting.py  ← LSTM Demand Forecasting
│
└── docs/
    ├── wiring_diagram.pdf      ← แผนผังการเดินสาย CT + RS485
    └── selec_register_map.pdf  ← Register Map Selec EM2M
```

---

## 🚀 เริ่มต้นใช้งาน

### ความต้องการระบบ

```bash
# Software ที่ต้องติดตั้ง
Docker Engine 24+          # สำหรับ Full Stack
Docker Compose v2          # รวมอยู่ใน Docker Desktop
Node-RED 3.1+              # สำหรับ Edge flow (หรือใช้ Docker)
CODESYS 3.5 SP19+          # PLC programming (Windows)
Python 3.10+               # สำหรับ AI module
```

### 1️⃣ Clone Repository

```bash
git clone https://github.com/username/wattiq.git
cd wattiq
```

### 2️⃣ เปิด Full Stack ด้วย Docker Compose

```bash
# รัน Mosquitto + InfluxDB + Telegraf + Grafana + Node-RED
docker compose -f infrastructure/C2_docker_compose.yml up -d

# ตรวจสอบ services
docker compose ps
```

| Service | URL | Login |
|---------|-----|-------|
| **Grafana** | http://localhost:3000 | admin / wattiq2024 |
| **InfluxDB** | http://localhost:8086 | admin / wattiq2024 |
| **Node-RED** | http://localhost:1880 | - |
| **MQTT Broker** | localhost:1883 | - |

> ⚠️ **สำคัญ:** เปลี่ยน password และ InfluxDB token ก่อนใช้งานจริงใน production

### 3️⃣ Import Node-RED Flow

```
1. เปิด http://localhost:1880
2. Menu (☰) → Import → เลือกไฟล์ infrastructure/C1_nodered_flow.json
3. แก้ IP address ของ Messung PLC ใน "Modbus Server" node
4. Deploy
```

### 4️⃣ ตั้งค่า Hardware

```
Selec EM2M:
  - Modbus Address : 1
  - Baud Rate      : 9600
  - Data bits      : 8
  - Stop bits      : 1
  - Parity         : None

RS485 Bus:
  - Termination 120Ω ที่ปลายทั้งสองด้าน
  - Topology: Daisy-chain เท่านั้น (ไม่ใช่ Star)
```

---

## 📚 เนื้อหาหลักสูตร (6 สัปดาห์)

```
Week 1-2 │ 🔌 HARDWARE  │ CT Sensor · Selec EM2M · RS485 · Modbus Poll
Week 3-4 │ ⚙️ PLC       │ CODESYS · Modbus Master FB · Alarm Logic
Week 5-6 │ ☁️ IT/CLOUD  │ Node-RED · MQTT · InfluxDB · Grafana Dashboard
Bonus    │ 🧠 AI        │ Anomaly Detection · LSTM Forecasting
```

---

## 🔌 Selec EM2M Register Map (Key Registers)

| Register | Parameter | Unit | Scale |
|----------|-----------|------|-------|
| 4000 | Voltage L1-N | V | ÷10 |
| 4003 | Current L1 | A | ÷10 |
| 4019 | Total Active Power | kW | ÷10 |
| 4027 | Power Factor (signed) | — | ÷1000 |
| 4029 | Frequency | Hz | ÷10 |
| 4500–4501 | Import Energy (DWORD) | kWh | ÷10 |

---

## 🐍 AI Module — Quick Start

```bash
# ติดตั้ง dependencies
pip install influxdb-client pandas scikit-learn tensorflow matplotlib

# รัน Anomaly Detection
python ai/D1_anomaly_detection.py

# รัน LSTM Forecasting (ต้องมีข้อมูล 2+ สัปดาห์)
python ai/D2_lstm_forecasting.py
```

---

## 💰 Bill of Materials

| รายการ | รุ่น | ราคาประมาณ |
|--------|------|-----------|
| Energy Meter | Selec EM2M-96 | ~3,500 ฿ |
| CT Sensor | Split-core 100A/5A | ~800 ฿ |
| PLC | Messung NX-ERA | ~15,000 ฿ |
| Edge Computer | Raspberry Pi 4 (4GB) | ~2,500 ฿ |
| RS485 Converter | USB-RS485 dongle | ~350 ฿ |
| **Software** | CODESYS + Node-RED + Docker Stack | **฿ 0** |
| **รวม** | | **~22,000–25,000 ฿** |

---

## 🗺️ Roadmap

- [x] Brand Identity (Logo, Colors, Tagline)
- [x] Google Sites Structure (6 pages)
- [x] Hero Banner HTML
- [x] CODESYS Sample Code
- [x] Node-RED Modbus→MQTT Flow
- [x] Docker Compose Full Stack
- [ ] Grafana Dashboard JSON (import-ready)
- [ ] CODESYS .project file
- [ ] Python AI Scripts (D1, D2)
- [ ] Wiring Diagram PDF
- [ ] Video tutorials (YouTube)

---

## 🤝 Contributing & Partners

สนใจร่วมพัฒนาหลักสูตร นำ Prototype ไปใช้ในสถาบัน หรือเป็น Hardware Partner  
ติดต่อผ่าน GitHub Issues หรือ Google Sites Contact form

---

## 📄 License

MIT License — ใช้งาน แก้ไข และเผยแพร่ได้อย่างเสรี โดยอ้างอิงแหล่งที่มา

---

<div align="center">
<strong>WATT·IQ</strong> — Smart Energy Management Learning Platform<br>
<em>"We believe the best way to learn energy systems is to build one."</em>
</div>
