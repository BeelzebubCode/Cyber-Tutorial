# 🛠 Aircrack-ng Suite – Wireless Attacks Toolkit

## 🧭 สารบัญ
- [ภาพรวม](#ภาพรวม)
- [airmon-ng – เปิด/ปิด Monitor Mode](#airmon-ng--เปิดปิด-monitor-mode)
- [airodump-ng – สแกน Wi-Fi และดักจับ Packet](#airodump-ng--สแกน-wi-fi-และดักจับ-packet)
- [aireplay-ng – ยิง Packet และโจมตี](#aireplay-ng--ยิง-packet-และโจมตี)
- [aircrack-ng – ถอดรหัส WPA Handshake](#aircrack-ng--ถอดรหัส-wpa-handshake)

---

## 📦 ภาพรวม

ชุดเครื่องมือ `aircrack-ng` คือหนึ่งในเครื่องมือหลักที่ใช้ใน **Wireless Penetration Testing** โดยเน้นการ:
- ตรวจสอบสภาพเครือข่ายไร้สาย
- สแกน Access Point และ Client
- ดักจับ Handshake
- ยิง Packet (เช่น deauth)
- Brute-force หารหัสผ่านจาก Handshake

---

## 🛰 airmon-ng – เปิด/ปิด Monitor Mode

ใช้เพื่อเปลี่ยน Wi-Fi interface ให้อยู่ใน **Monitor Mode** และจัดการ process ที่รบกวนการดักจับ packet

### 🔧 คำสั่งหลัก
```bash
sudo airmon-ng start wlan1        # เปิด Monitor Mode → wlan1mon
sudo airmon-ng stop wlan1mon      # ปิด Monitor Mode กลับสู่ Managed Mode
sudo airmon-ng check              # ตรวจสอบ process ที่รบกวน เช่น NetworkManager
sudo airmon-ng check kill         # ฆ่า process ที่รบกวนทั้งหมด
```

### 📌 หมายเหตุ
- ใช้งานสะดวกกว่าการใช้ `ip` และ `iw`
- ชื่อ interface อาจเปลี่ยน เช่น `wlan1` → `wlan1mon`

---

## 🔍 airodump-ng – สแกน Wi-Fi และดักจับ Packet

ใช้เพื่อ:
- แสดงข้อมูล Access Point และ Client
- บันทึก packet เป็นไฟล์ `.cap`
- ใช้ร่วมกับ `aircrack-ng` หรือ `hashcat` เพื่อถอดรหัส

### 🔧 คำสั่งพื้นฐาน
```bash
sudo airodump-ng wlan1mon
```

### 🔧 คำสั่งขั้นสูง (เฉพาะเจาะจง BSSID และ Channel)
```bash
sudo airodump-ng --bssid <BSSID> --channel <CH> -w capture wlan1mon
```

### 🧾 Options สำคัญ
| Option | ความหมาย |
|--------|-----------|
| `--bssid <BSSID>` | ระบุ Access Point เป้าหมาย |
| `--channel <CH>` | ระบุ channel ให้ตรงกับ AP |
| `-w <prefix>` | บันทึกข้อมูลเป็นไฟล์ capture (เช่น `.cap`) |
| `--write-interval 1` | บันทึกทุก ๆ วินาที (default 5s) |
| `--ignore-negative-one` | ปิด warning ช่องสัญญาณไม่ตรง (บาง adapter ต้องใช้) |

---

## 💥 aireplay-ng – ยิง Packet และโจมตี

ใช้เพื่อ:
- ยิง deauth packet ไล่ client ออก
- ทดลอง packet injection
- Replay authentication / ARP request ฯลฯ

### 🔧 คำสั่งยิง deauth
```bash
# ไล่ client รายตัว
sudo aireplay-ng --deauth 10 -a <BSSID> -c <Client MAC> wlan1mon

# ไล่ client ทั้งหมดใน AP
sudo aireplay-ng --deauth 10 -a <BSSID> wlan1mon
```

### 🧾 Options สำคัญ
| Option | ความหมาย |
|--------|-----------|
| `--deauth <n>` | จำนวน packet ที่จะยิง (0 = ไม่จำกัด) |
| `-a <BSSID>` | ระบุ Access Point เป้าหมาย |
| `-c <Client MAC>` | ระบุ MAC ของ client (ถ้าเจาะจง) |
| `--ignore-negative-one` | ใช้เมื่อมี error ช่องสัญญาณไม่ตรง |

### 📌 คำสั่งทดสอบว่า Adapter รองรับ injection หรือไม่
```bash
sudo aireplay-ng --test wlan1mon
```

---

## 🔐 aircrack-ng – ถอดรหัส WPA Handshake

ใช้ถอดรหัสไฟล์ `.cap` ที่ดักจับ handshake ได้ โดยใช้ wordlist

### 🔧 คำสั่งพื้นฐาน
```bash
aircrack-ng -w wordlist.txt -b <BSSID> capture.cap
```

### 🧾 Options สำคัญ
| Option | ความหมาย |
|--------|-----------|
| `-w <wordlist>` | ไฟล์รหัสผ่านสำหรับ brute-force |
| `-b <BSSID>` | ระบุ Access Point เป้าหมาย |
| `-e <ESSID>` | ระบุชื่อ Wi-Fi (ถ้าเจอหลายอัน) |
| `-l <output>` | บันทึกรหัสผ่านที่เจอไปยังไฟล์ |

### ✅ ตรวจสอบว่า handshake มีอยู่หรือไม่
```bash
aircrack-ng capture.cap
```
ถ้ามี handshake จะมีดาว (⭐) ขึ้นตรง `WPA handshake` กับ BSSID

---

📌 **หมายเหตุ:** การใช้งานเครื่องมือเหล่านี้ควรอยู่ภายใต้กฎหมาย และใช้เพื่อการศึกษาในแล็บเท่านั้น
