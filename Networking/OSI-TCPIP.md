# 📡 สรุป OSI Model และ TCP/IP Model

## 🌐 OSI Model (Open Systems Interconnection)
OSI Model มี 7 ชั้น (Layer) โดยแต่ละชั้นมีหน้าที่ดังนี้:

| ชั้น (Layer) | ชื่อ | หน้าที่หลัก |
|-------------|------|-------------|
| **7** | Application | ติดต่อกับโปรแกรม เช่น HTTP, FTP, SMTP |
| **6** | Presentation | แปลงข้อมูล (Encoding/Decoding), Compression, Encryption |
| **5** | Session | จัดการเซสชันการสื่อสาร |
| **4** | Transport | ส่งข้อมูลให้ถูกต้อง (TCP → เชื่อมต่อ, UDP → ไม่เชื่อมต่อ) |
| **3** | Network | กำหนดเส้นทาง (Routing), ใช้ IP Address |
| **2** | Data Link | จัดการ Frame, ใช้ MAC Address |
| **1** | Physical | สัญญาณไฟฟ้า, สาย LAN, Wi-Fi |

**ตัวอย่าง Protocol ที่ใช้ในแต่ละ Layer**
- Application → HTTP, FTP, SMTP
- Transport → TCP, UDP
- Network → IP, ICMP
- Data Link → Ethernet
- Physical → สาย UTP, Fiber

---

## 🌍 TCP/IP Model
TCP/IP Model มี 4 ชั้น (บางตำราแยกเป็น 5):

| ชั้น | ชื่อ | Mapping กับ OSI |
|------|------|-----------------|
| **4** | Application | OSI Layer 5-7 |
| **3** | Transport | OSI Layer 4 |
| **2** | Internet | OSI Layer 3 |
| **1** | Network Access | OSI Layer 1-2 |

**โปรโตคอลหลักใน TCP/IP**
- Application → HTTP, DNS, FTP
- Transport → TCP, UDP
- Internet → IP, ICMP
- Network Access → Ethernet, ARP

---

### ✅ สรุปสั้น
- **OSI = 7 ชั้น** (อธิบายละเอียด)
- **TCP/IP = 4 ชั้น** (ใช้งานจริงในอินเทอร์เน็ต)
- ทั้งสองโมเดลทำงานร่วมกัน แต่ TCP/IP ใช้จริงในโลกปัจจุบัน

---

📌 **Tip จำง่าย OSI**:  
`Please Do Not Throw Sausage Pizza Away`  
(Physical, Data Link, Network, Transport, Session, Presentation, Application)
