# 🌐 Nmap - Network Mapper (Advanced Guide)

> `nmap` คือเครื่องมือสแกนเครือข่ายที่ถูกใช้มากที่สุดในโลก Cyber Security  
> สามารถใช้เพื่อสแกนพอร์ต, fingerprint OS/Service, ตรวจหา CVE และหลอกการตรวจจับได้ด้วย

---

## 📚 สารบัญ

### 📖 บทนำ
- [ภาพรวมของ Nmap](#ภาพรวมของ-nmap)
- [ทำไมต้องใช้ Nmap?](#ทำไมต้องใช้-nmap)

### 🔰 พื้นฐาน
- [🧰 การติดตั้ง](#การติดตั้ง)
- [⚙️ โครงสร้างคำสั่งทั่วไป](#โครงสร้างคำสั่งทั่วไป)
- [📦 หมวด Option สำคัญ](#หมวด-option-สำคัญ)
  - [🧪 โหมดการสแกน (Scan Type)](#โหมดการสแกน-scan-type)
  - [🌐 Target & Port](#target--port)
  - [🔍 Service Detection](#service-detection)
  - [🧠 OS Detection](#os-detection)
  - [🚀 Speed & Timing (-T)](#speed--timing--t)
  - [📄 การบันทึกผลลัพธ์](#การบันทึกผลลัพธ์)

### 🧠 ระดับกลางถึงสูง
- [🕵️‍♂️ Spoofing & Evasion with Nmap](#spoofing--evasion-with-nmap)
- [🧟 Zombie Scan (Idle Scan)](#zombie-scan-idle-scan)
- [🎭 Firewall Evasion Techniques](#firewall-evasion-techniques)
- [🧪 NSE Script Deep Dive](#nse-script-deep-dive)
- [🔍 OS Fingerprinting Techniques](#os-fingerprinting-techniques)
- [🧨 รวมคำสั่งใช้งานจริง](#รวมคำสั่งใช้งานจริง)
- [📂 ตัวอย่าง NSE Script เด่น ๆ](#ตัวอย่าง-nse-script-เด่น-ๆ)
- [🧰 เทคนิคการใช้งาน](#เทคนิคการใช้งาน)
- [🧠 ใช้ใน CTF หรือ OSCP ยังไง](#ใช้ใน-ctf-หรือ-oscp-ยังไง)
- [🛡️ หมายเหตุ](#หมายเหตุ)

---

## 📖 ภาพรวมของ Nmap
Nmap (Network Mapper) เป็นเครื่องมือโอเพนซอร์สที่ใช้ในการสำรวจเครือข่ายและตรวจสอบความปลอดภัย มันสามารถสแกนพอร์ต, ระบุระบบปฏิบัติการ, ตรวจสอบ service version และค้นหาช่องโหว่ต่าง ๆ ได้

## 📖 ทำไมต้องใช้ Nmap?
- ใช้งานง่ายและยืดหยุ่น
- รองรับการสแกนขั้นสูง (stealth, spoofing, zombie)
- เป็นเครื่องมือมาตรฐานในวงการ Pentest/CTF/SOC

---

## 🧰 การติดตั้ง
```bash
sudo apt update
sudo apt install nmap
```

## ⚙️ โครงสร้างคำสั่งทั่วไป
```bash
nmap [TARGET] [OPTIONS]
```
**ตัวอย่าง:**
```bash
nmap -sS -p- -sV -O -T4 -Pn <IP>
```

---

## 📦 หมวด Option สำคัญ

### 🧪 โหมดการสแกน (Scan Type)
| คำสั่ง | ความหมาย |
|--------|----------|
| `-sS` | SYN Scan (Stealth) ✅ ใช้บ่อยที่สุด |
| `-sT` | TCP Connect Scan (ใช้เมื่อไม่ใช้ root) |
| `-sU` | UDP Scan |
| `-sN` | Null Scan (ไม่มี flag) |
| `-sF` | FIN Scan |
| `-sX` | Xmas Scan |

### 🌐 Target & Port
| คำสั่ง | ความหมาย |
|--------|----------|
| `-p-` | สแกนทุกพอร์ต |
| `-p 22,80,443` | สแกนเฉพาะพอร์ตที่ระบุ |
| `-F` | สแกนพอร์ตยอดนิยม (100 พอร์ตแรก) |

### 🔍 Service Detection
| คำสั่ง | ความหมาย |
|--------|----------|
| `-sV` | ตรวจ version ของ service |
| `--version-intensity <0-9>` | ความลึกในการตรวจ (default: 7) |

### 🧠 OS Detection
| คำสั่ง | ความหมาย |
|--------|----------|
| `-O` | ตรวจระบบปฏิบัติการ (OS Fingerprint) |
| `--osscan-guess` | เดา OS แม้ไม่แน่นอน |

### 🚀 Speed & Timing (-T)
| Level | คำอธิบาย |
|-------|-----------|
| `-T0` | Paranoid → ช้าที่สุด (เลี่ยง IDS) |
| `-T1` | Sneaky |
| `-T2` | Polite |
| `-T3` | Normal (default) |
| `-T4` | Aggressive → ใช้บ่อย |
| `-T5` | Insane → เร็วสุด เสี่ยงโดนจับ |

### 📄 การบันทึกผลลัพธ์
| คำสั่ง | ความหมาย |
|--------|----------|
| `-oN file.txt` | บันทึกเป็น text แบบอ่านง่าย |
| `-oX file.xml` | บันทึกเป็น XML (ใช้กับเครื่องมืออื่น) |
| `-oG file.gnmap` | Grepable Format |
| `-oA prefix` | บันทึกทุกฟอร์แมต (`prefix.nmap`, `.xml`, `.gnmap`) |

ตัวอย่าง:
```bash
nmap -sV -oA result <target>
```

---

## 🕵️‍♂️ Spoofing & Evasion with Nmap
เทคนิคหลอกระบบตรวจจับ (IDS/Firewall) ด้วยการปลอมแปลงพฤติกรรม:

| คำสั่ง | ความหมาย |
|--------|-----------|
| `-D RND:10` | สร้าง IP ปลอม (decoy) 10 IP รวมของเราด้วย เพื่อปิดบังตัวจริง |
| `--spoof-mac 0` | ปลอม MAC address แบบสุ่ม |
| `-S <IP>` | ปลอม Source IP (ต้องตั้ง routing กลับ) |
| `-Pn` | ไม่ส่ง ICMP ping เพื่อหลีกเลี่ยง block ping |
| `--source-port 53` | ใช้พอร์ตต้นทาง 53 (เหมือน DNS) เพื่อหลอก firewall |

ตัวอย่าง:
```bash
sudo nmap -sS -Pn -D RND:10 --spoof-mac 0 --source-port 53 10.10.234.123
```

---

## 🧟 Zombie Scan (Idle Scan)
การสแกนแบบไม่เปิดเผยตัว โดยใช้ IP อื่น (Zombie) เป็นตัวกลางในการ probe port บนเป้าหมาย

```bash
sudo nmap -sI <zombie_ip> <target_ip>
```

Zombie ต้อง:
- มี IP ID predictable
- traffic น้อย (idle)
- รับ packet จากเป้าหมายได้

ตัวอย่าง:
```bash
sudo nmap -sI 192.168.1.100 10.10.10.20
```

---
## 🎭 Firewall Evasion Techniques

> เทคนิคหลีกเลี่ยง Firewall และระบบตรวจจับ (IDS/IPS) เพื่อให้สามารถสแกนพอร์ตหรือบริการได้สำเร็จ

### 🔧 เทคนิคและคำสั่งที่ใช้ได้

| คำสั่ง | ความหมาย |
|--------|----------|
| `-f` | Fragment packet → แบ่ง packet ออกเป็นชิ้นเล็ก ๆ เพื่อหลบ firewall |
| `--mtu <val>` | ปรับขนาด MTU (เช่น 8, 16, 32) เพื่อ fragment packet แบบกำหนดเอง |
| `--data-length <len>` | ใส่ข้อมูลขยะลงใน packet เพื่อเปลี่ยน signature ของ packet |
| `--ttl <val>` | ตั้งค่า TTL (Time To Live) เพื่อหลบ routing หรือ IDS บางประเภท |
| `--source-port <port>` | เปลี่ยน port ต้นทางเพื่อหลอกว่า traffic มาจาก trusted port เช่น 53 (DNS), 443 (HTTPS) |
| `-D RND:10` | ใช้ IP ปลอม 10 IP เป็น decoy ปิดบังตัวจริง |

### 🧠 ตัวอย่างการใช้งาน

```bash
sudo nmap -sS -f -D RND:10 --source-port 53 10.10.10.10
```

หรือ

```bash
sudo nmap -sS --mtu 16 --data-length 100 10.10.10.10
```

### ⚠️ หมายเหตุ:
- Firewall บางตัวสามารถ reassemble fragmented packet ได้ → เทคนิคนี้อาจไม่ได้ผลเสมอไป
- บางเทคนิคอาจใช้ไม่ได้กับระบบสมัยใหม่ที่มี Deep Packet Inspection (DPI)

---

## 🧪 NSE Script Deep Dive

> Nmap Scripting Engine (NSE) คือระบบที่ช่วยให้ Nmap ทำงานมากกว่าการสแกนพอร์ต เช่น ตรวจหา CVE, ตรวจ SSL, ตรวจ auth method เป็นต้น

### 📁 หมวด script หลัก ๆ

| หมวด | ตัวอย่าง script |
|------|----------------|
| auth | ssh-auth-methods, ftp-anon |
| vuln | http-vuln-cve2006-3392, smb-vuln-ms17-010 |
| default | ใช้กับ `-sC` เช่น dns-brute, http-title |
| discovery | smb-os-discovery, dns-service-discovery |
| safe | ssl-cert, whois |

### 📌 คำสั่งใช้งาน
```bash
nmap -sV --script vuln <IP>
nmap --script ssl-cert -p 443 <IP>
nmap --script ftp-anon -p 21 <IP>
```

### 🔎 ดู script ที่มีทั้งหมด
```bash
ls /usr/share/nmap/scripts/
```

### ✏️ เขียน script เอง
ไฟล์ต้องลงท้าย `.nse` และอยู่ใน `/usr/share/nmap/scripts/`

ดูตัวอย่าง script:
```lua
id = "custom-script"
description = [[
  ตรวจหา banner ของ HTTP server
]]
author = "You"
license = "Same as Nmap"

portrule = shortport.http

action = function(host, port)
  return "Server banner: " .. http.get(host, port).header.server
end
```

จากนั้นสั่ง:
```bash
nmap --script custom-script <IP>
```

### 🧠 ใช้ NSE ยังไงให้คุ้ม
- `--script vuln` → หาช่องโหว่เบื้องต้น
- `--script default` → ใช้ควบคู่ `-sV` ได้เลย
- รันเฉพาะบาง script → ใช้ชื่อ script หรือหมวด เช่น `--script auth,vuln`

---
