GitHub Pages (peerawata.github.io)
│
├── index.html          ← เว็บ WATT·IQ หลัก (รันอัตโนมัติ)
├── B1_hero_banner.html ← เรียกผ่าน URL หรือ embed ใน Google Sites
├── B2_logo_assets.svg  ← ใส่ <img src="..."> ใน index.html
│
└── infrastructure/     ← Download ไปใช้บน Server เท่านั้น
    ├── C1_nodered_flow.json    → import ใน Node-RED
    ├── C2_docker_compose.yml   → รันบน Raspberry Pi
    └── C2_config_notes.txt     → อ่านเป็น Reference
