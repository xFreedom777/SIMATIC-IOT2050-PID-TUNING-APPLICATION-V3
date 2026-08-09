# SIMATIC IOT2050 PID Tuning Application — V3

> **[ภาษาไทย 🇹🇭 อยู่ด้านล่าง / Thai version below ⬇️]**

---

<div align="center">

```
███████╗██╗ █████╗ ███╗   ███╗ █████╗ ████████╗██╗ ██████╗
██╔════╝██║██╔══██╗████╗ ████║██╔══██╗╚══██╔══╝██║██╔════╝
███████╗██║███████║██╔████╔██║███████║   ██║   ██║██║
╚════██║██║██╔══██║██║╚██╔╝██║██╔══██║   ██║   ██║██║
███████║██║██║  ██║██║ ╚═╝ ██║██║  ██║   ██║   ██║╚██████╗
╚══════╝╚═╝╚═╝  ╚═╝╚═╝     ╚═╝╚═╝  ╚═╝   ╚═╝   ╚═╝ ╚═════╝
```

**Siemens SIMATIC IOT2050 × S7-1200 PLC × PIDCompact V2**

*Gate Valve Control & Monitoring System — Mitr Phol Pin Mill Plant*

![Node.js](https://img.shields.io/badge/Node.js-18%2B-green?logo=node.js)
![Siemens](https://img.shields.io/badge/Siemens-S7--1200-009999?logo=siemens)
![Platform](https://img.shields.io/badge/Platform-IOT2050%20Debian-blue)
![Protocol](https://img.shields.io/badge/Protocol-S7%20over%20TCP%2FIP-orange)
![License](https://img.shields.io/badge/License-Proprietary-red)

</div>

---

## 📑 Table of Contents

- [Overview](#-overview)
- [System Architecture](#-system-architecture)
- [Repository Structure](#-repository-structure)
- [Key Features](#-key-features)
- [IT Layer — Document & Report System](#-it-layer--document--report-system)
- [OT Layer — PLC Control System](#-ot-layer--plc-control-system)
- [Hardware Requirements](#-hardware-requirements)
- [Software Dependencies](#-software-dependencies)
- [Installation & Deployment](#-installation--deployment)
- [Configuration](#-configuration)
- [API Reference](#-api-reference)
- [Security](#-security)
- [Troubleshooting](#-troubleshooting)

---

## 🌐 Overview

**SIMATIC IOT2050 PID Tuning Application V3** is a full-stack industrial web application running directly on the **Siemens SIMATIC IOT2050** edge gateway. It bridges the OT (Operational Technology) world of Siemens S7-1200 PLCs with IT-side management capabilities including real-time HMI dashboards, remote PID parameter tuning, data logging, and automated document generation.

This application was developed specifically for the **Mitr Phol Pin Mill Plant** Gate Valve Control and Monitoring project, managing multiple PID control loops for gate valve positioning with full auto/manual/inactive mode switching.

### What Makes V3 Different

| Layer | Capability |
|---|---|
| **OT (Control)** | Live PID loop monitoring, S7 Protocol read/write, Simulation mode |
| **IT (Document)** | Automated Thai/English manual generation, PDF/HTML export, Stability reports |
| **Edge (Deploy)** | PowerShell patch deployment, systemd service management, Kiosk mode |

---

## 🏗 System Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                    IT NETWORK (Office / Engineering)                 │
│  ┌──────────────┐   HTTP/WS    ┌──────────────────────────────────┐ │
│  │  Engineer PC  │ ◄──────────► │   SIMATIC IOT2050 (Edge Gateway) │ │
│  │  (Browser)   │             │                                  │ │
│  └──────────────┘             │  ┌────────────┐  ┌────────────┐  │ │
│                               │  │ Node.js    │  │ Chromium   │  │ │
│                               │  │ Express    │  │ Kiosk Mode │  │ │
│                               │  │ WebSocket  │  │ (Display)  │  │ │
│                               │  └─────┬──────┘  └────────────┘  │ │
│                               │        │ S7 Protocol              │ │
└───────────────────────────────┼────────┼──────────────────────────┘ │
                                │        │                             │
┌───────────────────────────────┼────────┼─────────────────────────────┐
│                    OT NETWORK │ (Field │/ Plant)                      │
│                               │        ▼                             │
│                        ┌──────┴────────────────┐                    │
│                        │  Siemens S7-1200 PLC   │                    │
│                        │  PIDCompact V2 Blocks  │                    │
│                        │  (Multiple DB Numbers) │                    │
│                        └───────────┬───────────┘                    │
│                                    │ I/O Signals                    │
│                         ┌──────────▼──────────┐                    │
│                         │  Gate Valve Actuators│                    │
│                         │  (4-20mA / Digital)  │                    │
│                         └─────────────────────┘                    │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 📁 Repository Structure

```
SIMATIC-IOT2050-PID-TUNING-APPLICATION-V3/
│
├── 📂 src/                          # OT Core — Backend Logic
│   ├── s7client.js                  # S7-1200 Protocol Communication (nodes7 wrapper)
│   └── simulator.js                 # FOPDT Process Simulator (offline testing)
│
├── 📂 public/                       # Frontend HMI Web Interface
│   ├── index.html                   # Main Kiosk Dashboard (Glassmorphism UI)
│   └── [assets, css, js]            # Chart.js, fonts, styling
│
├── 📂 data/                         # Persistent Application Data
│   ├── blocks.json                  # PID Loop configurations (DB numbers, offsets)
│   └── config.json                  # PLC IP, Rack, Slot settings
│
├── 📂 logs/                         # Runtime Log Files
│   └── [YYYY-MM-DD].log             # Daily rotating logs
│
├── 📂 node_modules/                 # NPM Dependencies (do NOT copy to IOT2050)
│
├── 📄 server.js                     # Main Application Entry Point (Express + WS)
│
│   ── IT Document Generation Layer ──────────────────────────────────
│
├── 📄 generate_manual.js            # IT: Generates HTML User Manual (Thai language)
│                                    #     Gate Valve Control procedures
│
├── 📄 generate_detailed_manual.js   # IT: Detailed Thai-language manual with
│                                    #     14 operational sections & UI descriptions
│
├── 📄 generate_final_manual_merged.js # IT: Merges all manual sections into
│                                    #     single export-ready HTML document
│
├── 📄 embed_and_convert.js          # IT: Embeds images (logo, gate valve photos)
│                                    #     as Base64 into standalone HTML file
│
├── 📄 Stability_Test_Report.html    # IT: Standalone HTML stability test report
│                                    #     (no server required, self-contained)
│
│   ── Deploy & Operations ───────────────────────────────────────────
│
├── 📄 Deploy-Patch.ps1              # Windows PowerShell deployment script
│                                    #     Patches IOT2050 via SSH/SCP
│
└── 📄 README.md                     # This file
```

---

## ✨ Key Features

### OT Control Features

| Feature | Description |
|---|---|
| **Real-time Trend Graph** | Live Chart.js plotting of SP / PV / Output % with configurable time window |
| **Multi-Loop Management** | Add, edit, delete multiple PID loops (each mapped to a PLC Data Block) |
| **Live PID Parameter R/W** | Read and write Kp, Ti, Td directly to PLC `PIDCompact V2` memory blocks |
| **Mode Control** | Switch each loop between **Auto / Manual / Inactive** modes |
| **Quick Setpoint** | Operator-facing instant setpoint input with immediate PLC write |
| **Process Simulation** | FOPDT (First-Order Plus Dead Time) offline simulator for safe loop testing |
| **Auto-Tune Calculator** | IMC (Internal Model Control) method — recommends Kp, Ti, Td from process characteristics |
| **Performance Dashboard** | Shows Error, Overshoot %, Rise Time, Settling Time, IAE, ISE, RMSE metrics |
| **Data Logging** | 5-second interval CSV-compatible log with export function |
| **S7 Protocol** | Full `nodes7` library integration with configurable Rack/Slot/DB Offsets |

### IT Document Features

| Feature | Description |
|---|---|
| **Thai User Manual** | Auto-generated A4 HTML manual with 14 operational sections in Thai |
| **PDF-Ready Export** | Print-to-PDF formatted layouts using CSS `@media print` |
| **Image Embedding** | `embed_and_convert.js` embeds all images as Base64 for standalone distribution |
| **Stability Report** | Self-contained HTML report documenting system stability test results |
| **Merged Manual** | Final production manual combining all sections into one distributable file |

### Edge & Deploy Features

| Feature | Description |
|---|---|
| **PowerShell Deployer** | `Deploy-Patch.ps1` — patches IOT2050 over SSH without full re-deploy |
| **Kiosk Mode** | Chromium runs fullscreen on IOT2050 display port (no desktop environment needed) |
| **systemd Service** | Node.js app runs as a service with auto-restart on crash |
| **Anti-Corruption** | Chromium profile cleanup on startup to prevent display lock-ups |
| **RAM Guard** | WebSocket backpressure limit (1MB) to prevent OOM on 1GB RAM device |

---

## 📋 IT Layer — Document & Report System

The V3 IT layer is a standalone document generation pipeline that does **not** require the server to be running. All scripts run on a Windows development PC and output standalone HTML files for distribution.

### Document Generation Pipeline

```
generate_manual.js
       │
       ▼ (14 operational sections in Thai)
generate_detailed_manual.js
       │
       ▼ (merge all sections)
generate_final_manual_merged.js
       │
       ▼ (embed images as Base64)
embed_and_convert.js
       │
       ▼
📄 User_Manual_xDev_embedded.html  ← Final distributable document
```

### Manual Sections (14 sections — Thai language)

1. หน้าต่างกราฟแนวโน้ม (Trend Dashboard)
2. หน้าต่างตั้งค่าพารามิเตอร์ (Parameter Configuration)
3. หน้าต่างจำลองการทำงาน (Simulation Mode)
4. หน้าต่างแผงควบคุมหลัก (Main Dashboard)
5. หน้าต่างบันทึกข้อมูล (Data Logging)
6. การเพิ่มลูปควบคุม (Add PID Loop)
7. การเลือกโหมดการควบคุม (Control Mode Selection)
8. การเชื่อมต่อระบบ (System Connection)
9. สถานะของ PID Loop (Loop Status Indicators)
10. การตั้งค่าเป้าหมายเร่งด่วน (Quick Setpoint)
11. การตั้งค่าวันและเวลา (System Time Configuration)
12. แถบเมนูจัดการระบบ (System Management Bar)
13. การกำหนดตำแหน่งหน่วยความจำ (Data Block Offsets)
14. ระบบรักษาความปลอดภัย (Security PIN Unlock)

### Running Document Generation

```bash
# On Windows development PC
node generate_manual.js
node generate_detailed_manual.js
node generate_final_manual_merged.js
node embed_and_convert.js
```

---

## ⚙️ OT Layer — PLC Control System

### S7 Communication (`src/s7client.js`)

Wraps the `nodes7` library to communicate with Siemens S7-1200 via S7 Protocol over TCP/IP (port 102).

**Default Data Block Offsets (PIDCompact V2):**

| Variable | DB Byte Offset | Type | Description |
|---|---|---|---|
| Setpoint (SP) | Configurable | REAL | Target process value |
| Process Value (PV) | Configurable | REAL | Measured process value |
| Output | Configurable | REAL | Controller output % |
| Mode | Configurable | INT | 0=Inactive, 1=Manual, 2=Auto |
| Kp | Configurable | REAL | Proportional gain |
| Ti | Configurable | REAL | Integral time (seconds) |
| Td | Configurable | REAL | Derivative time (seconds) |

### FOPDT Simulator (`src/simulator.js`)

Implements First-Order Plus Dead Time mathematical model for safe offline loop testing:

```
Output(t) = Kp × (1 - e^(-(t-θ)/τ))

Where:
  Kp = Process gain
  τ  = Time constant (seconds)
  θ  = Dead time (seconds)
```

---

## 🖥 Hardware Requirements

| Component | Specification |
|---|---|
| **Edge Gateway** | Siemens SIMATIC IOT2050 (Advanced or Basic variant) |
| **RAM** | Minimum 1 GB (swap file strongly recommended) |
| **Storage** | Minimum 4 GB eMMC / SD Card |
| **OS** | Debian-based (SIMATIC IOT2050 Example Image) |
| **PLC** | Siemens S7-1200 (any firmware) with PIDCompact V2 blocks configured in TIA Portal |
| **Network** | Ethernet connection between IOT2050 and PLC (same subnet or routed) |
| **Display** | HDMI monitor for Kiosk mode (optional — remote browser access also supported) |

---

## 📦 Software Dependencies

### Runtime (Node.js — IOT2050)

```json
{
  "express": "^4.x",
  "ws": "^8.x",
  "nodes7": "^0.x"
}
```

### Frontend (CDN — no build step)

- **Chart.js** — Real-time trend graphing
- **Google Fonts (Sarabun)** — Thai language typography
- **Vanilla CSS** — Glassmorphism design system

### Development (Windows PC — Document Generation)

- Node.js 18+ (for running manual generation scripts)
- Any modern browser (for reviewing generated HTML)

---

## 🚀 Installation & Deployment

### Step 1: Prepare the IOT2050

> ⚠️ **IMPORTANT:** The IOT2050 has limited RAM (1GB). Create swap before installing dependencies.

```bash
# SSH into IOT2050
ssh root@<IOT2050-IP>

# Create 1GB swap file
fallocate -l 1G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile

# Make swap permanent
echo "/swapfile none swap sw 0 0" >> /etc/fstab
```

### Step 2: Transfer Repository

```bash
# On Windows PC — copy files (EXCLUDING node_modules)
scp -r ./SIMATIC-IOT2050-PID-TUNING-APPLICATION-V3 root@<IOT2050-IP>:/opt/pid-tuning-app

# OR use Deploy-Patch.ps1 (PowerShell)
# Right-click Deploy-Patch.ps1 → Run with PowerShell
```

### Step 3: Install Dependencies on IOT2050

```bash
cd /opt/pid-tuning-app
npm install
```

### Step 4: Configure systemd Service

```bash
# Create service file
cat > /etc/systemd/system/pid-tuning.service << 'EOF'
[Unit]
Description=PID Tuning Application
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/opt/pid-tuning-app
ExecStart=/usr/bin/node server.js
Restart=always
RestartSec=5
Environment=NODE_ENV=production PORT=3000

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl enable pid-tuning
systemctl start pid-tuning
```

### Step 5: Configure Kiosk Mode

```bash
# Create kiosk startup script
cat > /usr/local/bin/start-kiosk.sh << 'EOF'
#!/bin/bash
# Clean Chromium profile to prevent corruption
rm -rf /root/.config/chromium/Singleton*
# Start Chromium in kiosk mode
chromium-browser --kiosk --no-sandbox --disable-infobars \
  --disable-session-crashed-bubble \
  --app=http://localhost:3000
EOF

chmod +x /usr/local/bin/start-kiosk.sh
```

### Step 6: Verify Installation

```bash
# Check service status
systemctl status pid-tuning

# View live logs
journalctl -fu pid-tuning

# Test API
curl http://localhost:3000/api/status
```

---

## ⚙️ Configuration

### PLC Connection Settings

Configure via the web interface at `http://<IOT2050-IP>:3000` or edit `data/config.json` directly:

```json
{
  "plcIp": "192.168.1.10",
  "plcRack": 0,
  "plcSlot": 0
}
```

### PID Loop Configuration

Stored in `data/blocks.json`. Each loop entry:

```json
{
  "id": "loop-001",
  "name": "Gate Valve Loop 1",
  "dbNumber": 10,
  "pvUnit": "%",
  "spUnit": "%",
  "outputUnit": "%",
  "offsets": {
    "sp": 2,
    "pv": 6,
    "output": 10,
    "mode": 14,
    "kp": 18,
    "ti": 22,
    "td": 26
  }
}
```

---

## 🔌 API Reference

### Connection

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/connect` | Connect to PLC `{ ip, rack, slot }` |
| `DELETE` | `/api/connect` | Disconnect from PLC |
| `GET` | `/api/status` | System status, mode, block count |

### PID Loops

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/blocks` | List all PID loops |
| `POST` | `/api/blocks` | Add new PID loop |
| `PUT` | `/api/blocks/:id` | Update loop configuration |
| `DELETE` | `/api/blocks/:id` | Remove a loop |

### Parameters & Control

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/blocks/:id/params` | Read PID parameters from PLC |
| `POST` | `/api/blocks/:id/params` | Write PID parameters to PLC |
| `POST` | `/api/blocks/:id/mode` | Set loop mode (Auto/Manual/Inactive) |
| `POST` | `/api/blocks/:id/setpoint` | Write setpoint value |

### Data & Logs

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/blocks/:id/history` | Get historical trend data |
| `DELETE` | `/api/blocks/:id/history` | Clear history for a loop |
| `GET` | `/api/logs` | List available log files |
| `GET` | `/api/logs/:filename` | Download a specific log file |

### WebSocket

Connect to `ws://<IOT2050-IP>:3000` for real-time data push:

```json
// Incoming messages (server → browser)
{ "type": "poll", "blockId": "loop-001", "sp": 75.0, "pv": 74.8, "output": 52.3, "mode": 2, "timestamp": 1234567890 }
{ "type": "status", "connected": true, "mode": "plc", "plcIp": "192.168.1.10" }
```

---

## 🔒 Security

- **PIN Protection:** Parameter editing is locked behind a 4-digit PIN to prevent unauthorized changes by operators
- **Mode Safeguard:** PLC mode changes require confirmation to prevent accidental control mode switching
- **Network:** Application is intended for **isolated OT/plant network** — not designed for internet exposure
- **No Authentication:** Web interface has no user login; rely on network-level access control (VLAN/firewall)

---

## 🔧 Troubleshooting

### PLC Connection Fails

```bash
# Check network reachability
ping 192.168.1.10

# Check S7 port (102)
nc -zv 192.168.1.10 102

# Common causes:
# - PLC firewall / "Allow PUT/GET Communication" not enabled in TIA Portal
# - Wrong Rack/Slot number (S7-1200: Rack=0, Slot=1 or Slot=0)
# - PLC already has maximum active connections
```

### IOT2050 Runs Out of Memory

```bash
# Check current memory
free -h

# Check if swap is active
swapon --show

# View top memory consumers
top -o %MEM
```

### Chromium Kiosk Won't Start / Black Screen

```bash
# Clean corrupted profile
rm -rf /root/.config/chromium/Singleton*
rm -rf /root/.config/chromium/Default/Cache

# Restart kiosk
/usr/local/bin/start-kiosk.sh
```

### Node.js Service Not Starting

```bash
journalctl -fu pid-tuning --since "5 min ago"
```

---

---
---

# SIMATIC IOT2050 PID Tuning Application — V3 (ภาษาไทย)

> **[English version above ⬆️]**

---

## 📋 สารบัญ

- [ภาพรวมระบบ](#-ภาพรวมระบบ)
- [สถาปัตยกรรมระบบ](#-สถาปัตยกรรมระบบ)
- [โครงสร้างไฟล์ใน Repository](#-โครงสร้างไฟล์ใน-repository)
- [ฟีเจอร์หลัก](#-ฟีเจอร์หลัก)
- [ชั้น IT — ระบบสร้างเอกสาร](#-ชั้น-it--ระบบสร้างเอกสาร)
- [ชั้น OT — ระบบควบคุม PLC](#-ชั้น-ot--ระบบควบคุม-plc)
- [ความต้องการด้านฮาร์ดแวร์](#-ความต้องการด้านฮาร์ดแวร์)
- [การติดตั้งและ Deploy](#-การติดตั้งและ-deploy)
- [การตั้งค่า](#-การตั้งค่า)
- [การแก้ปัญหาเบื้องต้น](#-การแก้ปัญหาเบื้องต้น)

---

## 🌐 ภาพรวมระบบ

**SIMATIC IOT2050 PID Tuning Application V3** เป็น Web Application อุตสาหกรรมแบบ Full-Stack ที่ทำงานบน **Siemens SIMATIC IOT2050** Edge Gateway โดยตรง พัฒนาเพื่อ **โครงการควบคุมและตรวจสอบ Gate Valve ของโรงงาน Mitr Phol Pin Mill Plant** โดยเฉพาะ

ระบบทำหน้าที่เชื่อมโลก **OT (Operational Technology)** ของ PLC Siemens S7-1200 เข้ากับความสามารถด้าน **IT** ได้แก่ หน้าจอ HMI แบบ Real-time, การปรับจูน PID Parameter จากระยะไกล, การบันทึกข้อมูล (Data Logging) และการสร้างเอกสารอัตโนมัติ

### ความแตกต่างของ V3

| ชั้นระบบ | ความสามารถ |
|---|---|
| **OT (ควบคุม)** | ตรวจสอบ PID Loop แบบ Live, อ่าน/เขียนผ่าน S7 Protocol, โหมด Simulation |
| **IT (เอกสาร)** | สร้างคู่มือภาษาไทย/อังกฤษอัตโนมัติ, Export PDF/HTML, รายงานการทดสอบ |
| **Edge (Deploy)** | PowerShell Patch Deployment, systemd Service, Kiosk Mode บนหน้าจอ IOT2050 |

---

## 🏗 สถาปัตยกรรมระบบ

```
┌──────────────────────────────────────────────────────────────────────┐
│                  เครือข่าย IT (สำนักงาน / วิศวกรรม)                 │
│  ┌──────────────┐   HTTP/WS    ┌─────────────────────────────────┐  │
│  │  PC วิศวกร  │ ◄──────────► │  SIMATIC IOT2050 (Edge Gateway) │  │
│  │  (Browser)  │             │                                  │  │
│  └──────────────┘             │  ┌────────────┐  ┌────────────┐ │  │
│                               │  │ Node.js    │  │ Chromium   │ │  │
│                               │  │ Express    │  │ Kiosk Mode │ │  │
│                               │  │ WebSocket  │  │ (หน้าจอ)  │ │  │
│                               │  └─────┬──────┘  └────────────┘ │  │
│                               │        │ S7 Protocol             │  │
└───────────────────────────────┼────────┼─────────────────────────┘  │
                                │        │                              │
┌───────────────────────────────┼────────┼──────────────────────────────┐
│                  เครือข่าย OT │(Field/ │Plant)                        │
│                               │        ▼                             │
│                        ┌──────┴───────────────────┐                 │
│                        │  Siemens S7-1200 PLC      │                 │
│                        │  PIDCompact V2 Blocks     │                 │
│                        │  (หลาย DB Number)         │                 │
│                        └───────────┬───────────────┘                 │
│                                    │ สัญญาณ I/O                     │
│                         ┌──────────▼──────────────┐                 │
│                         │  Gate Valve Actuators    │                 │
│                         │  (4-20mA / Digital)      │                 │
│                         └────────────────────────-─┘                 │
└──────────────────────────────────────────────────────────────────────┘
```

---

## 📁 โครงสร้างไฟล์ใน Repository

```
SIMATIC-IOT2050-PID-TUNING-APPLICATION-V3/
│
├── 📂 src/                           # แกนหลัก OT — Backend Logic
│   ├── s7client.js                   # การสื่อสาร S7-1200 Protocol (Wrapper ของ nodes7)
│   └── simulator.js                  # ตัวจำลอง FOPDT (ทดสอบแบบ Offline)
│
├── 📂 public/                        # หน้าจอ HMI Frontend
│   ├── index.html                    # Dashboard หลัก (ออกแบบ Glassmorphism)
│   └── [assets, css, js]             # Chart.js, fonts, styling
│
├── 📂 data/                          # ข้อมูล Application ที่บันทึกถาวร
│   ├── blocks.json                   # การตั้งค่า PID Loop (DB Number, Offset)
│   └── config.json                   # IP PLC, Rack, Slot
│
├── 📂 logs/                          # ไฟล์ Log การทำงาน
│   └── [YYYY-MM-DD].log              # Log รายวัน
│
├── 📄 server.js                      # จุดเริ่มต้นของ Application (Express + WebSocket)
│
│   ── ชั้น IT: ระบบสร้างเอกสาร ──────────────────────────────────────
│
├── 📄 generate_manual.js             # สร้างคู่มือผู้ใช้ HTML ภาษาไทย
│                                     # ขั้นตอนการควบคุม Gate Valve
│
├── 📄 generate_detailed_manual.js    # สร้างคู่มือภาษาไทยแบบละเอียด 14 หัวข้อ
│                                     # พร้อมคำอธิบาย UI ทุกส่วน
│
├── 📄 generate_final_manual_merged.js # รวมทุก Section เป็น HTML ไฟล์เดียว
│                                     # พร้อม Export สำหรับแจกจ่าย
│
├── 📄 embed_and_convert.js           # Embed รูปภาพ (Logo, Gate Valve)
│                                     # เป็น Base64 สำหรับไฟล์ Standalone
│
├── 📄 Stability_Test_Report.html     # รายงานทดสอบเสถียรภาพ (Standalone HTML)
│
│   ── Deploy & Operations ────────────────────────────────────────────
│
├── 📄 Deploy-Patch.ps1               # Script PowerShell สำหรับ Patch IOT2050
│                                     # ผ่าน SSH จาก Windows PC
│
└── 📄 README.md                      # ไฟล์นี้
```

---

## ✨ ฟีเจอร์หลัก

### ฟีเจอร์ฝั่ง OT (ควบคุม)

| ฟีเจอร์ | คำอธิบาย |
|---|---|
| **กราฟแนวโน้ม Real-time** | Plot กราฟ SP / PV / Output % แบบ Live ด้วย Chart.js |
| **จัดการหลาย PID Loop** | เพิ่ม/แก้ไข/ลบหลาย Loop แต่ละ Loop แมปกับ Data Block ของ PLC |
| **อ่าน/เขียน PID Parameter** | อ่านและเขียนค่า Kp, Ti, Td ลงหน่วยความจำ `PIDCompact V2` ของ PLC โดยตรง |
| **สลับโหมดควบคุม** | เปลี่ยนแต่ละ Loop ระหว่าง **Auto / Manual / Inactive** |
| **Quick Setpoint** | ช่องกรอก Setpoint ด่วนสำหรับ Operator พร้อม Write ไป PLC ทันที |
| **Process Simulation** | จำลองระบบด้วย FOPDT (ทดสอบ Loop โดยไม่ต้องต่อ PLC จริง) |
| **Auto-Tune Calculator** | แนะนำค่า Kp, Ti, Td โดยวิธี IMC (Internal Model Control) |
| **Performance Dashboard** | แสดงค่า Error, Overshoot %, Rise Time, Settling Time, IAE, ISE, RMSE |
| **Data Logging** | บันทึกข้อมูลทุก 5 วินาที รองรับ Export ไฟล์ CSV |
| **S7 Protocol** | รองรับ `nodes7` library พร้อมตั้งค่า Rack/Slot/DB Offset ได้เอง |

### ฟีเจอร์ฝั่ง IT (เอกสาร)

| ฟีเจอร์ | คำอธิบาย |
|---|---|
| **คู่มือภาษาไทย** | สร้างคู่มือ HTML ขนาด A4 อัตโนมัติ 14 หัวข้อเป็นภาษาไทย |
| **Export PDF** | Layout จัด A4 สำหรับ Print เป็น PDF ผ่าน CSS `@media print` |
| **Embed รูปภาพ** | `embed_and_convert.js` แปลง Logo และรูปภาพ Gate Valve เป็น Base64 |
| **รายงานเสถียรภาพ** | Standalone HTML รายงานผลทดสอบระบบ ไม่ต้องพึ่ง Server |
| **คู่มือฉบับสมบูรณ์** | รวมทุก Section เป็นไฟล์เดียวสำหรับแจกจ่ายให้ทีมงาน |

### ฟีเจอร์ Edge & Deploy

| ฟีเจอร์ | คำอธิบาย |
|---|---|
| **PowerShell Deployer** | `Deploy-Patch.ps1` อัปเดต IOT2050 ผ่าน SSH โดยไม่ต้อง Re-deploy ทั้งหมด |
| **Kiosk Mode** | Chromium ทำงาน Fullscreen บนพอร์ต Display ของ IOT2050 |
| **systemd Service** | Node.js รันเป็น Service พร้อม Auto-restart หากเกิด Crash |
| **ป้องกัน Chromium เสีย** | ล้าง Profile ของ Chromium ทุกครั้งที่เปิดระบบ เพื่อป้องกันหน้าจอค้าง |
| **RAM Guard** | จำกัด WebSocket Buffer ไว้ที่ 1MB เพื่อป้องกัน Out-of-Memory บน RAM 1GB |

---

## 📋 ชั้น IT — ระบบสร้างเอกสาร

Pipeline สร้างเอกสารของ V3 ทำงานบน **Windows PC ของวิศวกร** แยกออกจาก Server บน IOT2050 โดยสมบูรณ์ ผลลัพธ์คือไฟล์ HTML แบบ Standalone พร้อมแจกจ่ายโดยไม่ต้องอาศัย Server

### ขั้นตอนการสร้างเอกสาร

```
generate_manual.js
       │
       ▼ (14 หัวข้อการใช้งาน ภาษาไทย)
generate_detailed_manual.js
       │
       ▼ (รวมทุก Section)
generate_final_manual_merged.js
       │
       ▼ (Embed รูปภาพเป็น Base64)
embed_and_convert.js
       │
       ▼
📄 User_Manual_xDev_embedded.html  ← ไฟล์คู่มือฉบับสมบูรณ์
```

### 14 หัวข้อในคู่มือภาษาไทย

1. **หน้าต่างกราฟแนวโน้ม (Trend Dashboard)** — แสดง SP, PV, Output แบบ Real-time
2. **หน้าต่างตั้งค่าพารามิเตอร์ (Parameter Configuration)** — ปรับ Kp, Ti, Td และ Limit
3. **หน้าต่างจำลองการทำงาน (Simulation Mode)** — ทดสอบ FOPDT ก่อนใช้งานจริง
4. **หน้าต่างแผงควบคุมหลัก (Main Dashboard)** — แสดงค่า SP/PV/Output ขนาดใหญ่
5. **หน้าต่างบันทึกข้อมูล (Data Logging)** — ดู Historical Data, Export CSV/PDF
6. **การเพิ่มลูปควบคุม (Add PID Loop)** — ระบุ Loop Name และ DB Number
7. **การเลือกโหมดการควบคุม (Control Mode Selection)** — Auto / Manual / Inactive
8. **การเชื่อมต่อระบบ (System Connection)** — ใส่ IP, Rack, Slot ของ PLC
9. **สถานะของ PID Loop (Loop Status Indicators)** — สีเขียว/แดง/เทา บอกสถานะ
10. **การตั้งค่าเป้าหมายเร่งด่วน (Quick Setpoint)** — กรอก Setpoint ได้ทันที
11. **การตั้งค่าวันและเวลา (System Time Configuration)** — Sync เวลา IOT2050
12. **แถบเมนูจัดการระบบ (System Management Bar)** — Lock, Restart, Power Off
13. **การกำหนดตำแหน่งหน่วยความจำ (Data Block Offsets)** — จับคู่ Address กับ TIA Portal
14. **ระบบรักษาความปลอดภัย (Security PIN Unlock)** — ป้องกันด้วยรหัส PIN 4 หลัก

### คำสั่งสร้างเอกสาร

```bash
# บน Windows PC ของวิศวกร
node generate_manual.js
node generate_detailed_manual.js
node generate_final_manual_merged.js
node embed_and_convert.js
```

---

## ⚙️ ชั้น OT — ระบบควบคุม PLC

### การสื่อสาร S7 (`src/s7client.js`)

ห่อหุ้ม library `nodes7` เพื่อสื่อสารกับ Siemens S7-1200 ผ่าน S7 Protocol บน TCP/IP (Port 102)

**ตำแหน่งหน่วยความจำ Data Block ค่าเริ่มต้น (PIDCompact V2):**

| ตัวแปร | Byte Offset | ชนิดข้อมูล | คำอธิบาย |
|---|---|---|---|
| Setpoint (SP) | ปรับได้ | REAL | ค่าเป้าหมาย |
| Process Value (PV) | ปรับได้ | REAL | ค่าที่วัดได้จริง |
| Output | ปรับได้ | REAL | Output ของ Controller (%) |
| Mode | ปรับได้ | INT | 0=Inactive, 1=Manual, 2=Auto |
| Kp | ปรับได้ | REAL | Proportional Gain |
| Ti | ปรับได้ | REAL | Integral Time (วินาที) |
| Td | ปรับได้ | REAL | Derivative Time (วินาที) |

### ตัวจำลอง FOPDT (`src/simulator.js`)

จำลองกระบวนการทางคณิตศาสตร์ First-Order Plus Dead Time เพื่อทดสอบ Loop แบบปลอดภัย:

```
Output(t) = Kp × (1 - e^(-(t-θ)/τ))

โดยที่:
  Kp = Process Gain (อัตราขยาย)
  τ  = Time Constant (ค่าคงที่เวลา, วินาที)
  θ  = Dead Time (ช่วงเวลาหน่วง, วินาที)
```

---

## 🖥 ความต้องการด้านฮาร์ดแวร์

| ส่วนประกอบ | ข้อกำหนด |
|---|---|
| **Edge Gateway** | Siemens SIMATIC IOT2050 (Advanced หรือ Basic) |
| **RAM** | ขั้นต่ำ 1 GB (แนะนำให้สร้าง Swap File) |
| **Storage** | ขั้นต่ำ 4 GB eMMC / SD Card |
| **OS** | Debian-based (SIMATIC IOT2050 Example Image) |
| **PLC** | Siemens S7-1200 (ทุก Firmware) ที่กำหนด PIDCompact V2 ไว้ใน TIA Portal แล้ว |
| **เครือข่าย** | Ethernet เชื่อมต่อ IOT2050 กับ PLC (Subnet เดียวกัน หรือผ่าน Router) |
| **จอแสดงผล** | Monitor HDMI สำหรับ Kiosk (ไม่บังคับ — เข้าผ่าน Browser จากระยะไกลได้) |

---

## 🚀 การติดตั้งและ Deploy

### ขั้นตอนที่ 1: เตรียม IOT2050

> ⚠️ **สำคัญ:** IOT2050 มี RAM จำกัด (1GB) ควรสร้าง Swap ก่อน Install dependencies

```bash
# SSH เข้า IOT2050
ssh root@<IP-ของ-IOT2050>

# สร้าง Swap 1GB
fallocate -l 1G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile

# ตั้งให้ Swap เปิดถาวรหลัง Reboot
echo "/swapfile none swap sw 0 0" >> /etc/fstab
```

### ขั้นตอนที่ 2: โอนไฟล์ไปยัง IOT2050

```bash
# บน Windows — ใช้ SCP (ไม่รวม node_modules)
scp -r ./SIMATIC-IOT2050-PID-TUNING-APPLICATION-V3 root@<IP>:/opt/pid-tuning-app

# หรือใช้ Deploy-Patch.ps1 (Windows PowerShell)
# คลิกขวา Deploy-Patch.ps1 → Run with PowerShell
```

### ขั้นตอนที่ 3: ติดตั้ง Dependencies

```bash
cd /opt/pid-tuning-app
npm install
```

### ขั้นตอนที่ 4: ตั้งค่า systemd Service

```bash
cat > /etc/systemd/system/pid-tuning.service << 'EOF'
[Unit]
Description=PID Tuning Application
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/opt/pid-tuning-app
ExecStart=/usr/bin/node server.js
Restart=always
RestartSec=5
Environment=NODE_ENV=production PORT=3000

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl enable pid-tuning
systemctl start pid-tuning
```

### ขั้นตอนที่ 5: ตรวจสอบการติดตั้ง

```bash
# ตรวจสอบสถานะ Service
systemctl status pid-tuning

# ดู Log แบบ Live
journalctl -fu pid-tuning

# ทดสอบ API
curl http://localhost:3000/api/status
```

---

## ⚙️ การตั้งค่า

### ตั้งค่าการเชื่อมต่อ PLC

ตั้งค่าผ่านหน้า Web ที่ `http://<IP-IOT2050>:3000` หรือแก้ไขไฟล์ `data/config.json` โดยตรง:

```json
{
  "plcIp": "192.168.1.10",
  "plcRack": 0,
  "plcSlot": 0
}
```

### ตั้งค่า PID Loop

บันทึกใน `data/blocks.json` แต่ละ Loop:

```json
{
  "id": "loop-001",
  "name": "Gate Valve Loop 1",
  "dbNumber": 10,
  "pvUnit": "%",
  "spUnit": "%",
  "outputUnit": "%",
  "offsets": {
    "sp": 2,
    "pv": 6,
    "output": 10,
    "mode": 14,
    "kp": 18,
    "ti": 22,
    "td": 26
  }
}
```

---

## 🔒 ความปลอดภัย

- **PIN Protection:** การแก้ไข Parameter ถูกล็อคด้วย PIN 4 หลัก ป้องกัน Operator เปลี่ยนค่าโดยไม่ได้รับอนุญาต
- **Mode Safeguard:** การเปลี่ยนโหมด PLC ต้องมีการยืนยันก่อน ป้องกันการกดผิดพลาด
- **Network:** ออกแบบสำหรับ **เครือข่าย OT แบบแยก (Isolated)** ไม่ควรเปิดให้เข้าถึงจาก Internet
- **ไม่มี Login:** Web Interface ไม่มีระบบ Login ผู้ใช้ — ควรควบคุมการเข้าถึงด้วย Firewall / VLAN แทน

---

## 🔧 การแก้ปัญหาเบื้องต้น

### เชื่อมต่อ PLC ไม่ได้

```bash
# ทดสอบ Network
ping 192.168.1.10

# ทดสอบ S7 Port (102)
nc -zv 192.168.1.10 102

# สาเหตุที่พบบ่อย:
# - ยังไม่เปิด "Allow PUT/GET Communication" ใน TIA Portal
# - ใส่ Rack/Slot ผิด (S7-1200: Rack=0, Slot=1 หรือ Slot=0)
# - PLC มี Connection เต็มแล้ว
```

### IOT2050 หน่วยความจำเต็ม (Out of Memory)

```bash
# ตรวจสอบ Memory ปัจจุบัน
free -h

# ตรวจสอบ Swap
swapon --show

# ดูโปรแกรมที่กิน RAM มากที่สุด
top -o %MEM
```

### Chromium Kiosk เปิดไม่ได้ / หน้าจอดำ

```bash
# ล้าง Profile ที่เสียหาย
rm -rf /root/.config/chromium/Singleton*
rm -rf /root/.config/chromium/Default/Cache

# เปิด Kiosk ใหม่
/usr/local/bin/start-kiosk.sh
```

### Node.js Service ไม่ Start

```bash
journalctl -fu pid-tuning --since "5 min ago"
```

---

<div align="center">

**พัฒนาโดย xFreedom777 สำหรับ Mitr Phol Pin Mill Plant**

*Siemens SIMATIC IOT2050 × S7-1200 × PIDCompact V2*

*Last Updated: August 2026*

</div>

