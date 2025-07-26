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

### 🗂 อุปกรณ์และโปรโตคอลในแต่ละชั้นของ OSI Model
| Layer | ชื่อ | อุปกรณ์ที่เกี่ยวข้อง | Protocols ที่เกี่ยวข้อง |
|-------|------|-----------------------|---------------------------|
| **7** | Application | Proxy Server, Load Balancer, Web Server | HTTP, HTTPS, FTP, SMTP, DNS |
| **6** | Presentation | Gateway (บางกรณี) | SSL/TLS, JPEG, MPEG |
| **5** | Session | Gateway, Application Server | NetBIOS, RPC |
| **4** | Transport | Firewall (ตรวจ TCP/UDP), Load Balancer | TCP, UDP |
| **3** | Network | Router, Layer 3 Switch | IP, ICMP, ARP, RIP, OSPF |
| **2** | Data Link | Switch, Bridge | Ethernet, PPP, HDLC |
| **1** | Physical | Hub, Repeater, สาย UTP, Fiber | ไม่มี (ใช้สัญญาณไฟฟ้าหรือแสง) |

---

## 🌍 TCP/IP Model
TCP/IP Model มี 4 ชั้น (บางตำราแยกเป็น 5):

| ชั้น | ชื่อ | Mapping กับ OSI |
|------|------|-----------------|
| **4** | Application | OSI Layer 5-7 |
| **3** | Transport | OSI Layer 4 |
| **2** | Internet | OSI Layer 3 |
| **1** | Network Access | OSI Layer 1-2 |

### 🗂 อุปกรณ์และโปรโตคอลในแต่ละชั้นของ TCP/IP Model
| Layer (TCP/IP)   | Mapping OSI       | อุปกรณ์ที่เกี่ยวข้อง | Protocols ที่เกี่ยวข้อง |
|-------------------|--------------------|------------------------|---------------------------|
| Application       | Layer 5-7         | Proxy, Load Balancer, Web Server | HTTP, HTTPS, FTP, SMTP, DNS |
| Transport         | Layer 4           | Firewall (Port Filter), Load Balancer | TCP, UDP |
| Internet          | Layer 3           | Router, Layer 3 Switch | IP, ICMP, ARP |
| Network Access    | Layer 1-2         | Switch, Hub, NIC, Access Point | Ethernet, PPP |

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

📌 **Tip จำง่าย OSI**:  
`Please Do Not Throw Sausage Pizza Away`  
(Physical, Data Link, Network, Transport, Session, Presentation, Application)

---

## 🔍 การทำงาน End-to-End (Sender → Receiver) ตาม OSI Model

### ✅ ขั้นตอนฝั่งผู้ส่ง (Sender) – Encapsulation
1. **Application Layer (ชั้น 7):**  
   แอปพลิเคชันสร้างข้อมูล เช่น ผู้ใช้ส่งข้อความผ่าน HTTP  
   **Output:** Data

2. **Presentation Layer (ชั้น 6):**  
   แปลงข้อมูล → Encoding, Compression, Encryption  
   **Output:** Data ที่ถูกเข้ารหัส

3. **Session Layer (ชั้น 5):**  
   จัดการเซสชัน สร้างการเชื่อมต่อกับผู้รับ  
   **Output:** Data พร้อม Session Info

4. **Transport Layer (ชั้น 4):**  
   แบ่งข้อมูลเป็น **Segment** (TCP) หรือ **Datagram** (UDP) และใส่ **Port**  
   **Output:** Segment (มี TCP/UDP Header)

5. **Network Layer (ชั้น 3):**  
   เพิ่ม **IP Header** (Source IP, Destination IP) → ได้ **Packet**  
   **Output:** Packet

6. **Data Link Layer (ชั้น 2):**  
   เพิ่ม **MAC Header และ Trailer (FCS)** → ได้ **Frame**  
   **Output:** Frame

7. **Physical Layer (ชั้น 1):**  
   แปลงเป็น **Bits (0/1)** ส่งออกผ่านสาย (UTP, Fiber) หรือ Wireless  
   **Output:** Binary Signal

---

### ✅ ขั้นตอนฝั่งผู้รับ (Receiver) – Decapsulation
1. **Physical Layer:** รับสัญญาณไฟฟ้าหรือแสง → แปลงเป็น Bits  
2. **Data Link Layer:** ตรวจสอบ FCS, MAC Address → ถอด Frame Header → ได้ Packet  
3. **Network Layer:** ตรวจสอบ IP Address → ถอด IP Header → ได้ Segment  
4. **Transport Layer:** รวม Segment → ตรวจสอบ Port → ได้ Data  
5. **Session Layer:** จัดการเซสชัน → ส่งต่อ Data  
6. **Presentation Layer:** ถอดรหัส, Decompression → ได้ข้อมูลพร้อมใช้งาน  
7. **Application Layer:** ส่งให้แอปพลิเคชันแสดงผล เช่น เว็บเพจ, ข้อความ  

---

### ✅ ภาพรวม Encapsulation & Decapsulation
Sender: Data → Segment → Packet → Frame → Bits
Receiver: Bits → Frame → Packet → Segment → Data

---

### ✅ OSI vs TCP/IP
- การทำงานที่อธิบายด้านบนเป็น **ตาม OSI Model (7 ชั้น)**  
- ใน **TCP/IP Model (4 ชั้น)** จะรวม:
  - Application = OSI Layer 5-7  
  - Transport = OSI Layer 4  
  - Internet = OSI Layer 3  
  - Network Access = OSI Layer 1-2  

---

📌 **สรุป:**  
- Encapsulation = ฝั่งผู้ส่ง → เพิ่ม Header ในแต่ละชั้น  
- Decapsulation = ฝั่งผู้รับ → ถอด Header ในแต่ละชั้น  

