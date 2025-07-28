# 🌐 Nmap - Network Mapper (Advanced Guide)

> `nmap` คือเครื่องมือสแกนเครือข่ายที่ถูกใช้มากที่สุดในโลก Cyber Security  
> สามารถใช้เพื่อสแกนพอร์ต, fingerprint OS/Service, ตรวจหา CVE และหลอกการตรวจจับได้ด้วย

---

## 🧰 การติดตั้ง

```bash
sudo apt update
sudo apt install nmap
```

---

## ⚙️ โครงสร้างคำสั่งทั่วไป

```bash
nmap [TARGET] [OPTIONS]
```

ตัวอย่าง:
```bash
nmap -sS -p- -sV -O -T4 -Pn <IP>
```

---

## 📦 หมวด Option สำคัญ

### 🧪 โหมดการสแกน (Scan Type)

| คำสั่ง | ความหมาย |
|--------|----------|
| `-sS` | SYN Scan (Stealth) ✅ ใช้บ่อยที่สุด |
| `-sT` | TCP Connect (ไม่ stealth, ใช้เมื่อไม่ใช้ root) |
| `-sU` | UDP Scan |
| `-sN` | Null Scan (ไม่มี flag ใด ๆ) |
| `-sF` | FIN Scan |
| `-sX` | Xmas Scan |

---

### 🌐 Target & Port

| คำสั่ง | ความหมาย |
|--------|----------|
| `-p-` | สแกนทุกพอร์ต (1–65535) |
| `-p 22,80,443` | สแกนเฉพาะพอร์ตที่ระบุ |
| `-F` | สแกนพอร์ตยอดนิยม (100 พอร์ตแรก) |

---

### 🔍 Service Detection

| คำสั่ง | ความหมาย |
|--------|----------|
| `-sV` | ตรวจ version ของ service |
| `--version-intensity <0-9>` | ความลึกในการตรวจ version (default: 7) |

---

### 🧠 OS Detection

| คำสั่ง | ความหมาย |
|--------|----------|
| `-O` | ตรวจระบบปฏิบัติการ (OS Fingerprinting) |
| `--osscan-guess` | เดา OS แม้ไม่แม่นยำ |

---

### 🚀 Speed & Timing (`-T`)

| Level | คำอธิบาย |
|-------|-----------|
| `-T0` | Paranoid → ช้าที่สุด (หลีกเลี่ยง IDS) |
| `-T1` | Sneaky → ช้ามาก |
| `-T2` | Polite → ช้าปานกลาง |
| `-T3` | Normal (default) |
| `-T4` | Aggressive → เร็ว, ใช้บ่อย |
| `-T5` | Insane → เร็วมาก, เสี่ยงถูก detect |

> ✅ ใช้ `-T4` เป็น default หากไม่กลัวถูก IDS จับ

---

### 🕵️‍♂️ Spoofing & Evasion

| คำสั่ง | ความหมาย |
|--------|----------|
| `-D RND:10` | สร้าง IP ปลอม (decoy) 10 IP |
| `--spoof-mac 0` | ปลอม MAC address แบบสุ่ม |
| `-S <IP>` | ปลอม source IP (ต้องตั้ง routing ด้วย) |
| `-Pn` | ไม่ส่ง ICMP ping → ใช้กับเป้าหมายที่ block ping |
| `--source-port <port>` | เปลี่ยน port ต้นทาง เช่น 53 เพื่อหลอกเป็น DNS |

---

## 🧨 รวมคำสั่งใช้งานจริง

### 🔥 สแกนทุกพอร์ต + OS + Service + version

```bash
sudo nmap -sS -p- -sV -O -T4 -Pn <IP>
```

### 🧪 ตรวจหา CVE (vuln scan)

```bash
nmap -sV --script vuln <IP>
```

### 🔐 ตรวจ SSL Certificate

```bash
nmap --script ssl-cert -p 443 <IP>
```

### 🕸️ ตรวจ directory hidden บน web

```bash
nmap --script http-enum -p 80 <IP>
```

### 🧟 Zombie Scan (idle scan ไม่เปิดเผยตัวจริง)

```bash
sudo nmap -sI <zombie_ip> <target_ip>
```

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

---

## 🧠 ใช้ใน CTF หรือ OSCP ยังไง

- เริ่มจาก `nmap -sS -p- -T4 -Pn <IP>` → ดูว่าอะไรเปิด
- ต่อด้วย `-sV -sC` → ดู service และ run script พื้นฐาน
- ถ้าเป็นเว็บ → ใช้ `http-enum`, `http-title`, `http-methods`
- หา CVE → ใช้ `--script vuln`, หรือ export service ไปหา PoC เพิ่มเติม

---

## 🛡️ หมายเหตุ

- อย่าใช้กับเป้าหมายจริงโดยไม่มีสิทธิ์เด็ดขาด
- การใช้ script อาจกระทบระบบจริง
- ควรใช้ใน lab, CTF หรือระบบที่คุณควบคุมเท่านั้น

---

> 💣 Nmap ไม่ใช่แค่ scanner — มันคือเรดาร์ของนักเจาะระบบทุกคน  
> ใช้คล่อง = เห็นทุกทางเข้า ใช้ไม่คล่อง = เดินเข้ากับดัก 🕵️
