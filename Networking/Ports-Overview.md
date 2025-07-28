# 🔌 Ports Overview

## 📚 Table of Contents
- [พอร์ตคืออะไร](#พอร์ตคืออะไร)
- [พอร์ตยอดนิยมที่ใช้บ่อย](#พอร์ตยอดนิยมที่ใช้บ่อย)
- [พอร์ตที่นิยมใช้ในการโจมตี](#พอร์ตที่นิยมใช้ในการโจมตี)
- [TCP และ UDP](#tcp-และ-udp)
- [ตรวจสอบพอร์ตที่เปิดอยู่](#ตรวจสอบพอร์ตที่เปิดอยู่)
- [พอร์ตในแบบ Encapsulation ใน OSI/TCPIP](#พอร์ตในแบบ-encapsulation-ใน-ositcpip)
- [อ่านเพิ่มเติม/อ้างอิง](#อ่านเพิ่มเติมอ้างอิง)

---

## 🧠 พอร์ตคืออะไร

**Port** คือ จุดสื่อสร้าง logical endpoint ที่ใช้แยกบริการสื่อสารที่ใช้ IP เดียวกันได้

- มีทั้งหมด **65536 พอร์ต** (`0 - 65535`)
- แบ่งพอร์ต:
  - **Well-known (0-1023)**: พอร์ตมาตรฐาน
  - **Registered (1024-49151)**: ใช้เก็บแอพ/โปรแกรม
  - **Dynamic (49152-65535)**: สุ่มเพื่อใช้ชั่วคราว

---

## 📘 พอร์ตยอดนิยมที่ใช้บ่อย

| Port | Protocol | บริการ | หน้าที่ |
|------|----------|------------|-------------|
| 20   | TCP      | FTP Data   | รับส่งไฟล์ |
| 21   | TCP      | FTP Control| ควบคุม FTP |
| 22   | TCP      | SSH        | เข้าถึงเรมอต |
| 23   | TCP      | Telnet     | เข้าถึงไม่เบรียบรหรือ |
| 25   | TCP      | SMTP       | ส่งอีเมล |
| 53   | UDP/TCP  | DNS        | แปลงโดเมน เป็น IP |
| 67   | UDP      | DHCP Server| แจก IP ให้ client |
| 68   | UDP      | DHCP Client| รับ IP จาก server |
| 80   | TCP      | HTTP       | ใช้บ่อยเว็บ (http) |
| 110  | TCP      | POP3       | ดึงอีเมล |
| 123  | UDP      | NTP        | ซิงค์เวลา |
| 143  | TCP      | IMAP       | ซิงค์อีเมลแบบ online |
| 161  | UDP      | SNMP       | ตรวจเครื่อข่ายเครือข่าย |
| 443  | TCP      | HTTPS      | เว็บแบบเรา |
| 445  | TCP      | SMB        | แชร์ไฟล์/พรินเตอร์ |
| 3389 | TCP      | RDP        | Remote Desktop Protocol |

---

## 🚨 พอร์ตที่นิยมใช้ในการโจมตี

| Port | Protocol | ใช้โจมตีผ่าน | ตัวอย่างการโจมตี |
|------|----------|---------------|-------------------|
| 21   | TCP      | FTP           | Brute force, Anonymous login |
| 22   | TCP      | SSH           | Brute force SSH, key guessing |
| 23   | TCP      | Telnet        | ดักจับข้อมูล, ใช้งานไม่มีการเข้ารหัส |
| 25   | TCP      | SMTP          | Email spoofing, spam relay |
| 53   | UDP      | DNS           | DNS amplification, spoofing |
| 80   | TCP      | HTTP          | SQLi, XSS, Directory traversal |
| 135  | TCP      | RPC           | Windows exploits (DCOM) |
| 139  | TCP      | NetBIOS       | แชร์ไฟล์ที่ไม่มีการป้องกัน |
| 445  | TCP      | SMB           | EternalBlue, WannaCry |
| 3389 | TCP      | RDP           | Remote access brute force |

> ✅ หมายเหตุ: พอร์ตเหล่านี้ถูกใช้งานได้ถูกต้องตามปกติ แต่หากเปิดโดยไม่ควบคุม อาจกลายเป็นช่องโหว่ให้แฮ็กเกอร์โจมตีได้

---

## 🔐 TCP และ UDP

- **TCP:** มีการเช็คความถูก ปลอดภัย
- **UDP:** เร็ว ไม่มีการเช็คความถูก

---

## 🔍 ตรวจสอบพอร์ตที่เปิดอยู่

```bash
# Linux
sudo netstat -tulnp

# Windows
netstat -an | findstr LISTEN
```

---

## 📦 พอร์ตในแบบ Encapsulation ใน OSI/TCPIP

**Port** อยู่ที่ Layer 4 (Transport Layer)

- บ่ง TCP/IP และ OSI เพื่อระบุแอพที่ส่ง
- ช่วยให้แอพหลายแอพที่ต่างกันใช้ IP เดียวกันได้

---

## 🌐 อ่านเพิ่มเติม/อ้างอิง

- 🔗 [IANA Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)
- 🔗 [RFC 6335 — Internet Assigned Numbers Authority (IANA) Procedures for Ports](https://datatracker.ietf.org/doc/html/rfc6335)
- 🔗 [RFC 793 — Transmission Control Protocol (TCP)](https://datatracker.ietf.org/doc/html/rfc793)
- 🔗 [OWASP Port Attack Surface Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Port_Attack_Surface_Cheat_Sheet.html)
- 🔗 [Wikipedia: List of TCP and UDP port numbers](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers)
