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

---

## 🕵️‍♂️ Spoofing & Evasion with Nmap

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
