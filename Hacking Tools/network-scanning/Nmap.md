# 🌐 Nmap - Network Mapper (Advanced Guide)

> `nmap` คือเครื่องมือสแกนเครือข่ายที่ถูกใช้มากที่สุดในโลก Cyber Security  
> สามารถใช้เพื่อสแกนพอร์ต, fingerprint OS/Service, ตรวจหา CVE และหลอกการตรวจจับได้ด้วย

---

## 📚 สารบัญ

### 📖 บทนำ
- [📖 ภาพรวมของ Nmap](#-ภาพรวมของ-nmap)
- [📖 ทำไมต้องใช้ Nmap?](#-ทำไมต้องใช้-nmap)

### 🔰 พื้นฐาน
- [🧰 การติดตั้ง](#-การติดตั้ง)
- [⚙️ โครงสร้างคำสั่งทั่วไป](#-โครงสร้างคำสั่งทั่วไป)
- [📦 หมวด Option สำคัญ](#-หมวด-option-สำคัญ)
  - [🧪 โหมดการสแกน (Scan Type)](#-โหมดการสแกน-scan-type)
  - [🌐 Target & Port](#-target--port)
  - [🔍 Service Detection](#-service-detection)
  - [🧠 OS Detection](#-os-detection)
  - [🚀 Speed & Timing (-T)](#-speed--timing--t)
  - [📄 การบันทึกผลลัพธ์](#-การบันทึกผลลัพธ์)

### 🧠 ระดับกลางถึงสูง
- [🕵️‍♂️ Spoofing & Evasion with Nmap](#-spoofing--evasion-with-nmap)
- [🧟 Zombie Scan (Idle Scan)](#-zombie-scan-idle-scan)
- [🎭 Firewall Evasion Techniques](#-firewall-evasion-techniques)
- [🧪 NSE Script Deep Dive](#-nse-script-deep-dive)
- [🔍 OS Fingerprinting Techniques](#-os-fingerprinting-techniques)
- [🧨 รวมคำสั่งใช้งานจริง](#-รวมคำสั่งใช้งานจริง)
- [📂 ตัวอย่าง NSE Script เด่น ๆ](#-ตัวอย่าง-nse-script-เด่น-ๆ)
- [🧰 เทคนิคการใช้งาน](#-เทคนิคการใช้งาน)
- [🧠 ใช้ใน CTF หรือ OSCP ยังไง](#-ใช้ใน-ctf-หรือ-oscp-ยังไง)
- [⚙️ การเรียงลำดับ Option ของ Nmap (Best Practice for Professionals)](#การเรียงลำดับ-option-ของ-nmap)
- [📚 วิธีศึกษาเพิ่มเติม](#-วิธีศึกษาเพิ่มเติม)
- [🛡️ หมายเหตุ](#-หมายเหตุ)

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

<sub>[⬆ Home](#-สารบัญ)</sub>

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

<sub>[⬆ Home](#-สารบัญ)</sub>

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

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🧟 Zombie Scan (Idle Scan)

> Zombie Scan หรือ Idle Scan เป็นเทคนิคการสแกนพอร์ตแบบ **ไม่เปิดเผยตัวตน** โดยใช้เครื่องอื่น (Zombie) เป็นตัวกลางในการสแกน
> เทคนิคนี้ช่วยให้ผู้โจมตีไม่ต้องเปิดเผย IP จริงให้เป้าหมายเห็นเลยแม้แต่น้อย

### 🧠 แนวคิด

แทนที่เราจะส่ง packet ไปยังเป้าหมายโดยตรง (ซึ่งอาจถูก firewall, IDS, หรือ log จับได้)  
เรากลับให้ **Zombie** เป็นตัวส่ง probe ไปแทน

เราควบคุม Zombie ด้วย packet พิเศษ และสังเกตการเปลี่ยนแปลงเล็ก ๆ น้อย ๆ ในพฤติกรรมของมัน เช่น **IP ID field** เพื่อเดาว่าเป้าหมายเปิดพอร์ตหรือไม่

### 🔧 คำสั่งพื้นฐาน
```bash
sudo nmap -sI <zombie_ip> <target_ip>
```

ตัวอย่าง:
```bash
sudo nmap -sI 10.10.10.50 10.10.10.20
```
- `10.10.10.50` คือ Zombie (เครื่องที่มี IP ID predictable)
- `10.10.10.20` คือ Target ที่เราจะสแกน

### 🔍 การทำงานเบื้องหลัง
1. **เราส่ง probe ไปหา Zombie** เพื่อดูค่า IP ID ปัจจุบันของมัน
2. **เราสั่ง Zombie ไปเปิด connection กับ Target**
3. **Target ตอบกลับ** ไปหา Zombie (ถ้า port open จะส่ง SYN-ACK, ถ้า closed จะส่ง RST)
4. **Zombie ตอบกลับให้เราเห็น IP ID ใหม่**
5. เราเปรียบเทียบว่า IP ID เพิ่มขึ้นหรือไม่ → ถ้าเพิ่มขึ้น = port นั้น "open"

### ✅ เงื่อนไขที่ Zombie ต้องมี

| เงื่อนไข | เหตุผล |
|----------|--------|
| 🟢 มี IP ID ที่ predictable | เพื่อให้ดูการเปลี่ยนแปลงหลังส่ง packet |
| 🟢 มี traffic ต่ำ (idle) | ไม่งั้น IP ID จะเปลี่ยนเองตลอดเวลา |
| 🟢 เปิด TCP port (เช่น 80) | เพื่อให้เราติดต่อ Zombie ได้ |
| 🟢 อยู่ใน subnet ที่เราควบคุม หรือ reachable | Zombie ต้อง reachable และสามารถรับ packet จากเป้าหมายได้ |

### 🧪 หา Zombie อย่างไร?
ใช้คำสั่ง:
```bash
nmap -Pn -O -p 80 <ip>
```
- ตรวจว่าเปิดพอร์ต TCP ที่เราติดต่อได้
- ต้องลองหลาย IP เพื่อหาเครื่องที่ idle จริง ๆ

### 💡 ตัวอย่างเต็มการใช้งาน
```bash
sudo nmap -sI 192.168.1.100 10.10.10.20
```
ผลลัพธ์:
```
Interesting ports on 10.10.10.20:
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```
→ เป้าหมายไม่รู้เลยว่า "เราคือใคร" เพราะ packet มาจาก 192.168.1.100

### 🧩 ประโยชน์ของ Zombie Scan

| ข้อดี | รายละเอียด |
|-------|-------------|
| 🕵️‍♂️ **Stealth** | เป้าหมายไม่เห็น IP เรา |
| 🛡️ **Bypass Firewall/IDS** | ระบบตรวจจับไม่สามารถเชื่อมโยงกับ IP เราได้ |
| 💥 **ใช้กับ Target สำคัญได้** | เหมาะกับระบบที่ log ทุก connection |

### ⚠️ ข้อจำกัด

| ข้อจำกัด | รายละเอียด |
|----------|-------------|
| ❌ หา Zombie ที่ดีได้ยาก | Zombie ต้อง idle และ predictable จริง ๆ |
| ❌ Zombie ต้อง reachable จากเป้าหมาย | เป้าหมายต้องสามารถส่ง response กลับไปยัง Zombie |
| ❌ IP ID ถูกสุ่มในระบบใหม่ ๆ | OS ใหม่มักสุ่ม IP ID → Zombie ไม่เหมาะสม |
| ❌ ช้า | ต้องจับหลายรอบเพื่อดู IP ID change |

### 🚨 ข้อควรระวัง
- อย่าใช้เทคนิคนี้ในระบบจริงที่คุณ **ไม่มีสิทธิ์**
- ใช้ใน CTF / TryHackMe / HackTheBox เท่านั้น
- Zombie ควรอยู่ในเครือข่ายที่คุณควบคุมได้ เช่น lab, VM, หรือ subnet ปลอม

<sub>[⬆ Home](#-สารบัญ)</sub>

---
## 🎭 Firewall Evasion Techniques

> เทคนิคหลีกเลี่ยง Firewall และระบบตรวจจับ (IDS/IPS) เพื่อให้สามารถสแกนพอร์ตหรือบริการได้สำเร็จ

### 🔧 เทคนิคและคำสั่งที่ใช้ได้

| คำสั่ง | ความหมาย |
|--------|----------|
| `-f` | Fragment packet → แบ่ง packet ออกเป็นชิ้นเล็ก ๆ เพื่อหลบ firewall, ทำให้ IDS ยากต่อการตรวจจับลำดับ packet ที่แท้จริง |
| `--mtu <val>` | กำหนดขนาด MTU (Maximum Transmission Unit) → ปกติใช้ 8, 16, 32, 64 เพื่อบังคับให้ fragment packet |
| `--data-length <len>` | ใส่ข้อมูล padding เข้าไปใน packet เช่น 50, 100, 200 byte เพื่อเปลี่ยน signature ของ packet |
| `--ttl <val>` | ปรับค่า TTL (Time To Live) เช่น 1–255 → ใช้หลบ routing path หรือ signature IDS ที่ filter ตาม TTL ที่ใช้บ่อย |
| `--source-port <port>` | กำหนดพอร์ตต้นทางของ packet เช่น 53 (DNS), 123 (NTP), 443 (HTTPS) เพื่อหลอก firewall ให้คิดว่ามาจาก traffic ปกติ |
| `-D RND:10` | สร้าง IP ปลอม 10 IP โดยสุ่ม (random) เป็น decoy → ช่วยกลบ IP ผู้โจมตีจริง |
| `--spoof-mac <mac|0>` | ปลอม MAC address → ใส่ `0` เพื่อให้สุ่มอัตโนมัติ เช่น `--spoof-mac 0`, หรือกำหนดแบบ `00:11:22:33:44:55` |
| `--badsum` | ส่ง packet ที่มี checksum ผิดจงใจ → IDS บางตัวตอบกลับโดยไม่ตรวจ checksum ทำให้หลอกได้ว่าเปิดพอร์ต |
| `--randomize-hosts` | สุ่มลำดับของ IP ที่สแกน → ป้องกัน IDS ตรวจจับ pattern เช่นเรียง IP ทีละตัว |
| `--scan-delay <t>` | กำหนดเวลาหน่วงระหว่าง probe → เช่น `--scan-delay 100ms` หรือ `--scan-delay 1s` เพื่อลด rate ให้ stealth มากขึ้น |

### 📊 ค่าที่ใช้บ่อยใน <val>, <t>, <len>

| Parameter | ตัวอย่างที่ใช้ได้ | หมายเหตุ |
|-----------|------------------|----------|
| `--mtu` | `8`, `16`, `32`, `64` | ต่ำลง = fragment เยอะขึ้น (stealth มากขึ้น แต่ช้าลง) |
| `--data-length` | `50`, `100`, `200` | ยิ่งมากยิ่งเบี่ยง signature ได้ดี |
| `--ttl` | `65`, `128`, `255` | บาง IDS ใช้ TTL เป็น signature detection |
| `--scan-delay` | `100ms`, `500ms`, `1s`, `5s` | ใช้เพื่อ bypass rate-limiting firewall/IPS |
| `--source-port` | `53`, `80`, `123`, `443` | เลียนแบบ traffic ปกติ เช่น DNS, HTTP, HTTPS |

### 💡 หมายเหตุ:
- แนะนำให้ใช้คู่กับ `-sS` (SYN Scan) และ `-Pn` (No Ping) เพื่อหลบ ICMP Block
- ควรทดลองบนเครื่อง Lab ก่อนใช้งานจริง
- บาง firewall หรือ IPS ที่มี Deep Packet Inspection (DPI) อาจยังสามารถตรวจจับได้

### 🧪 ตัวอย่างชุดคำสั่งหลบ firewall:
```bash
sudo nmap -sS -Pn -f --mtu 16 --data-length 50 --ttl 65 --source-port 53 -D RND:10 <target-ip>
```

> 🔒 เหมาะสำหรับการทำ Recon ในสถานการณ์ที่ต้องการปิดบังตัวตนหรือหลบการตรวจจับจากระบบป้องกัน

### ⚠️ หมายเหตุ:
- Firewall บางตัวสามารถ reassemble fragmented packet ได้ → เทคนิคนี้อาจไม่ได้ผลเสมอไป
- บางเทคนิคอาจใช้ไม่ได้กับระบบสมัยใหม่ที่มี Deep Packet Inspection (DPI)

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🧪 NSE Script Deep Dive

> Nmap Scripting Engine (NSE) คือระบบที่ช่วยให้ Nmap ทำงานมากกว่าการสแกนพอร์ต เช่น ตรวจหา CVE, ตรวจ SSL, ตรวจ auth method เป็นต้น

> 📌 NSE Scripts ถูกเขียนด้วยภาษา *[Lua](https://www.lua.org/)* ซึ่งเป็นภาษา lightweight ที่ทำงานได้เร็วและเหมาะกับการฝังในเครื่องมืออย่าง Nmap

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

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🔍 OS Fingerprinting Techniques

> การตรวจหา OS ของเป้าหมายมีความสำคัญมากในกระบวนการ Reconnaissance เพื่อวางแผนโจมตีหรือป้องกันต่อไป

### 🔧 เทคนิคหลักของ Nmap

| คำสั่ง | ความหมาย |
|--------|----------|
| `-O` | ตรวจสอบระบบปฏิบัติการโดยใช้ TCP/IP stack fingerprinting |
| `--osscan-guess` | เดา OS แม้ข้อมูลไม่เพียงพอ (อาจไม่แม่น) |
| `--osscan-limit` | จำกัดเฉพาะ host ที่มี port ที่ช่วยให้ fingerprint ได้ดี |

```bash
sudo nmap -O 10.10.10.10
sudo nmap -O --osscan-guess 10.10.10.10
```

### 🔬 วิธีการทำงาน
Nmap จะใช้ชุด probe (packet พิเศษ) ส่งไปยังพอร์ตต่าง ๆ แล้วดูว่าเป้าหมายตอบสนองอย่างไร เช่น:
- ขนาดของ TTL
- ขนาด window size
- การตอบสนอง ICMP
- ลักษณะ TCP flag response

ระบบปฏิบัติการแต่ละตัวมี “ลายเซ็น” ที่ต่างกัน เช่น Windows กับ Linux ตอบ TCP flags ไม่เหมือนกัน

### 💡 คำแนะนำ
- ควรใช้ควบคู่กับ `-sS -sV` เพื่อเพิ่มความแม่น
- หากใช้ `-O` ไม่ได้ผล ให้ลองเปิดพอร์ตเพิ่มเติม เช่น `-p-`
- ใช้ร่วมกับ `-T4 -Pn` เพื่อหลีกเลี่ยง rate-limit/firewall

```bash
sudo nmap -sS -sV -O -Pn -T4 <IP>
```

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🧨 รวมคำสั่งใช้งานจริง

> 🔧 หมวดนี้คือการรวมชุดคำสั่ง Nmap ที่ใช้งานบ่อย พร้อมคำอธิบายว่าแต่ละคำสั่งเหมาะกับสถานการณ์ใด เพื่อให้เข้าใจและนำไปใช้ได้จริง

### 🔥 สแกนทุกพอร์ต + OS + Service + version
```bash
sudo nmap -sS -p- -sV -O -T4 -Pn <IP>
```
📌 ใช้เมื่อคุณต้องการรวบรวมข้อมูลแบบละเอียดที่สุดในครั้งเดียว: 
- `-p-` สแกนพอร์ตทั้งหมด
- `-sV` ตรวจเวอร์ชัน service
- `-O` เดา OS
- `-T4` เพิ่มความเร็ว
- `-Pn` ไม่ ping ก่อนสแกน → เหมาะกับ target ที่ block ICMP

---

### 🧪 ตรวจหา CVE (vuln scan)
```bash
nmap -sV --script vuln <IP>
```
🔎 ตรวจหาช่องโหว่ที่เป็นที่รู้จัก (CVE) จาก service version ที่เปิดอยู่ โดยใช้สคริปต์ `vuln` ซึ่งใช้ฐานข้อมูลของ Nmap เองในการเปรียบเทียบ

---

### 🔐 ตรวจ SSL Certificate
```bash
nmap --script ssl-cert -p 443 <IP>
```
🔐 ใช้ดูรายละเอียดใบรับรอง SSL เช่นวันหมดอายุ, issuer, CN เป็นต้น → เหมาะกับการตรวจสอบความถูกต้องของ HTTPS/web service

---

### 🕸️ ตรวจ directory hidden บน web
```bash
nmap --script http-enum -p 80 <IP>
```
🧱 ค้นหา directory หรือ endpoint ที่ถูกซ่อนอยู่บน web server เช่น `/admin`, `/backup`, `/test` โดยไม่ต้องใช้เครื่องมือ brute force เพิ่มเติม

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 📂 ตัวอย่าง NSE Script เด่น ๆ

| Script | ความสามารถ |
|--------|-------------|
| `http-enum` | สำรวจ directory/web page ที่ซ่อนอยู่ |
| `ftp-anon` | ตรวจว่า FTP login แบบ anonymous ได้ไหม |
| `ssh-auth-methods` | ตรวจว่า SSH รองรับการ auth แบบใด |
| `vuln` | รวมการตรวจหาช่องโหว่ทั่วไป |
| `smb-os-discovery` | ตรวจ OS ผ่าน SMB |
| `ssl-heartbleed` | ตรวจช่องโหว่ Heartbleed |

ค้นหา script เพิ่มเติม:
```bash
ls /usr/share/nmap/scripts/
```

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🧰 เทคนิคการใช้งาน

- ใช้ `-oN` บันทึกผลลัพธ์ลงไฟล์:
```bash
nmap -sV -oN result.txt <IP>
```

- ค้นหา host ทั้ง subnet:
```bash
nmap -sn 192.168.1.0/24
```

- ใช้ร่วมกับ wordlist (จำกัดเฉพาะบางเป้าหมาย):
```bash
nmap -iL target_list.txt -sV
```

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🧠 ใช้ใน CTF หรือ OSCP ยังไง

1. เริ่มจาก `nmap -sS -p- -T4 -Pn <IP>` → ดูว่าอะไรเปิด
2. ต่อด้วย `-sV -sC` → ดู service และ run script พื้นฐาน
3. ถ้าเป็นเว็บ → ใช้ `http-enum`, `http-title`, `http-methods`
4. หา CVE → ใช้ `--script vuln`, หรือ export service ไปหา PoC เพิ่มเติม

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## การเรียงลำดับ Option ของ Nmap

> การเรียง Option ในคำสั่ง Nmap ไม่จำเป็นต้องเรียงเป๊ะเสมอไปเพราะ `nmap` จะประมวลผลตามชนิดของ flag  
> แต่การเรียงอย่างเป็นระเบียบจะช่วยให้ **อ่านง่าย**, **วิเคราะห์เร็ว**, และ **ลดโอกาสพลาด**

## ✅ ลำดับที่มือโปรนิยมใช้

```bash
nmap [Scan Type] [Port Selection] [Timing] [Ping Option] [Version/OS Detect] [Evasion] [Script] [Output] <target>
```

### ตัวอย่าง:
```bash
sudo nmap -sS -p- -T4 -Pn -sV -O -D RND:10 --source-port 53 -f --mtu 16 --script vuln -oA result <target>
```

---

## 🔢 ลำดับที่แนะนำ

| ลำดับ | หมวด | ตัวอย่าง | หมายเหตุ |
|-------|------|----------|----------|
| 1 | Scan Type | `-sS`, `-sU`, `-sT` | วิธีสแกน |
| 2 | Port | `-p-`, `-F` | ระบุพอร์ต |
| 3 | Timing | `-T0` ถึง `-T5` | ความเร็วในการสแกน |
| 4 | Ping | `-Pn`, `-sn` | ตรวจว่า host up |
| 5 | Service/OS Detect | `-sV`, `-O`, `--version-intensity` | ตรวจหา service version หรือ OS |
| 6 | Evasion | `-D`, `--spoof-mac`, `--source-port`, `--mtu`, `-f` | ปลอมแปลงเพื่อหลบการตรวจจับ |
| 7 | NSE Script | `--script vuln`, `--script http-enum` | ใช้สคริปต์ตรวจเจาะเฉพาะ |
| 8 | Output | `-oN`, `-oX`, `-oA` | บันทึกผลลัพธ์ |

---

## 🎯 เหตุผลที่ควรเรียง

- อ่านง่าย: วิเคราะห์เร็วโดยไม่ต้องไล่หา option
- แก้ไขสะดวก: อยากเปลี่ยนอะไรรู้ว่าตรงไหน
- สื่อสารได้ดี: เวลาแชร์คำสั่งให้คนอื่นอ่าน

## 🧠 สรุปแบบจำ
> **Scan → Port → Timing → Ping → Detect → Evasion → Script → Output**

ลองยึดรูปแบบนี้ไว้ แล้วต่อไปคุณจะเรียกใช้ Nmap แบบโปรได้คล่องกว่าเดิม 🚀

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 📚 วิธีศึกษาเพิ่มเติม

หากคุณต้องการฝึกฝนเพิ่มเติมเกี่ยวกับการใช้ Nmap อย่างมีประสิทธิภาพ แนะนำแหล่งเรียนรู้ดังต่อไปนี้:

- 🌐 [Nmap Official Book](https://nmap.org/book/) — คู่มืออย่างเป็นทางการโดยผู้พัฒนา
- 📘 [Nmap Reference Guide](https://nmap.org/man/) — man page อย่างละเอียดจากผู้พัฒนา
- 💡 [TryHackMe: Nmap Room](https://tryhackme.com/room/nmap) — ห้องเรียนออนไลน์พร้อม lab ให้ลองจริง
- 💡 [Hack The Box: Starting Point - Nmap](https://www.hackthebox.com/) — ภารกิจแนะนำสำหรับมือใหม่
  
### 📖 การศึกษาเพิ่มเติมในคำสั่ง

### 🔍 ใช้ man page และ help command

Nmap มีเอกสารในเครื่องที่สามารถเรียนรู้ได้โดยตรง:

```bash
man nmap
```
จะเปิดหน้า manual ที่ละเอียดที่สุด ครอบคลุมทุก option พร้อมตัวอย่าง

```bash
nmap --help
```
แสดงสรุปคำสั่งที่ใช้บ่อย รวมถึงตัวเลือกหลัก ๆ

### 💡 Tip:
- ใช้ `man` ร่วมกับ `/` เพื่อค้นหาคำภายในหน้า เช่น `/spoof` เพื่อหา spoofing
- ใช้ `q` เพื่อออกจาก man page
- ใช้ `h` เพื่อดู help ภายใน `man`
- 
<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🛡️ หมายเหตุ

- อย่าใช้กับเป้าหมายจริงโดยไม่มีสิทธิ์เด็ดขาด
- การใช้ script อาจกระทบระบบจริง
- ควรใช้ใน lab, CTF หรือระบบที่คุณควบคุมเท่านั้น

> 💣 Nmap ไม่ใช่แค่ scanner — มันคือเรดาร์ของนักเจาะระบบทุกคน  
> ใช้คล่อง = เห็นทุกทางเข้า ใช้ไม่คล่อง = เดินเข้ากับดัก 🕵️

<sub>[⬆ Home](#-สารบัญ)</sub>

