# 📡 สรุป OSI Model และ TCP/IP Model

### 🌐 OSI Model (Open Systems Interconnection)
OSI Model มี 7 ชั้น (Layer) โดยแต่ละชั้นมีหน้าที่ดังนี้:

| ชั้น (Layer) | ชื่อ        | หน้าที่หลัก                                                      |
|--------------|-------------|-------------------------------------------------------------------|
| **7**        | Application | ติดต่อกับโปรแกรม เช่น HTTP, FTP, SMTP                          |
| **6**        | Presentation | แปลงข้อมูล (Encoding/Decoding), Compression, Encryption         |
| **5**        | Session      | จัดการเซสชันการสื่อสาร                                           |
| **4**        | Transport    | ส่งข้อมูลให้ถูกต้อง (TCP → เชื่อมต่อ, UDP → ไม่เชื่อมต่อ)       |
| **3**        | Network      | กำหนดเส้นทาง (Routing), ใช้ IP Address                          |
| **2**        | Data Link    | จัดการ Frame, ใช้ MAC Address                                    |
| **1**        | Physical     | สัญญาณไฟฟ้า, สาย LAN, Wi-Fi                                      |

---

### **1. Physical Layer (ชั้น 1)**
   - **หน้าที่หลัก**: ส่งข้อมูลในรูปแบบสัญญาณไฟฟ้าหรือแสงผ่านช่องทางทางกายภาพ
   - **รายละเอียด**: ชั้นนี้เกี่ยวข้องกับการถ่ายโอนข้อมูลที่แท้จริงระหว่างอุปกรณ์ในเครือข่าย เช่น สาย LAN, Wi-Fi, หรือ Fiber Optic โดยการแปลงข้อมูลจากรูปแบบดิจิตอลให้เป็นสัญญาณไฟฟ้าหรือแสง และส่งผ่านช่องทางทางกายภาพ เช่น สายเคเบิลหรือคลื่นวิทยุ
   - **อุปกรณ์ที่เกี่ยวข้อง**: Hub, Repeater

### **2. Data Link Layer (ชั้น 2)**
   - **หน้าที่หลัก**: การจัดการ Frame, ใช้ MAC Address
   - **รายละเอียด**: ชั้นนี้ทำหน้าที่ในการส่งข้อมูลระหว่างเครื่องในเครือข่ายเดียวกัน โดยแยกข้อมูลเป็น Frame และใช้งาน MAC Address เพื่อตรวจสอบและกำหนดอุปกรณ์ปลายทาง เช่น การใช้ Ethernet, PPP ในการส่งข้อมูลภายใน LAN
   - **อุปกรณ์ที่เกี่ยวข้อง**: Switch, Bridge

   ### **Frame คืออะไร?**
   - สำหรับรายละเอียดเพิ่มเติมเกี่ยวกับ **Frame** โปรดอ่าน [คำอธิบายเพิ่มเติมเกี่ยวกับ Frame](./Frame_Explanation.md)

### **3. Network Layer (ชั้น 3)**
   - **หน้าที่หลัก**: กำหนดเส้นทาง (Routing), ใช้ IP Address
   - **รายละเอียด**: ชั้นนี้จะจัดการการส่งข้อมูลจากเครื่องต้นทางไปยังปลายทางผ่านเครือข่ายต่าง ๆ โดยใช้ IP Address เพื่อกำหนดที่อยู่ของเครื่องในเครือข่าย และทำหน้าที่เลือกเส้นทาง (Routing) ที่ดีที่สุดในการส่งข้อมูล
   - **อุปกรณ์ที่เกี่ยวข้อง**: Router, Layer 3 Switch

### **4. Transport Layer (ชั้น 4)**
   - **หน้าที่หลัก**: ส่งข้อมูลให้ถูกต้อง (TCP → เชื่อมต่อ, UDP → ไม่เชื่อมต่อ)
   - **รายละเอียด**: ชั้นนี้ทำหน้าที่ในการแบ่งข้อมูลเป็นส่วนเล็ก ๆ (Segment) และตรวจสอบความถูกต้องในการส่งข้อมูลระหว่างเครื่องปลายทาง เช่น การตรวจสอบความสมบูรณ์ของข้อมูลผ่าน TCP หรือการส่งข้อมูลแบบไม่เชื่อมต่อด้วย UDP
   - **อุปกรณ์ที่เกี่ยวข้อง**: Firewall (ตรวจสอบ TCP/UDP)

### **5. Session Layer (ชั้น 5)**
   - **หน้าที่หลัก**: จัดการเซสชันการสื่อสาร
   - **รายละเอียด**: ชั้นนี้ทำหน้าที่ในการสร้าง, จัดการ และสิ้นสุดเซสชันระหว่างแอปพลิเคชันหรือเครื่องที่สื่อสารกัน เช่น การควบคุมการเปิดและปิดการเชื่อมต่อระหว่างสองเครื่อง
   - **อุปกรณ์ที่เกี่ยวข้อง**: Gateway, Application Server

### **6. Presentation Layer (ชั้น 6)**
   - **หน้าที่หลัก**: แปลงข้อมูล (Encoding/Decoding), Compression, Encryption
   - **รายละเอียด**: ชั้นนี้ทำหน้าที่ในการแปลงข้อมูลให้อยู่ในรูปแบบที่แอปพลิเคชันสามารถเข้าใจได้ เช่น การเข้ารหัส (Encryption), การบีบอัดข้อมูล (Compression), และการแปลงรูปแบบของข้อมูล (Encoding/Decoding) เช่น จาก ASCII เป็น EBCDIC
   - **อุปกรณ์ที่เกี่ยวข้อง**: Gateway (บางกรณี)

### **7. Application Layer (ชั้น 7)**
   - **หน้าที่หลัก**: ติดต่อกับโปรแกรม เช่น HTTP, FTP, SMTP
   - **รายละเอียด**: ชั้นนี้ทำหน้าที่เป็นอินเตอร์เฟซระหว่างผู้ใช้กับแอปพลิเคชันที่ใช้งาน เช่น เว็บเบราว์เซอร์, โปรแกรมอีเมล, หรือ FTP Client โดยการทำงานในชั้นนี้จะช่วยให้ผู้ใช้สามารถเข้าถึงข้อมูลจากแอปพลิเคชันต่าง ๆ บนเครือข่าย
   - **อุปกรณ์ที่เกี่ยวข้อง**: Proxy Server, Load Balancer, Web Server

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

### **1. Network Access Layer (ชั้น 1-2)**
   - **หน้าที่หลัก**: การส่งข้อมูลผ่านช่องทางทางกายภาพและการจัดการ Frame, ใช้ MAC Address
   - **รายละเอียด**: ชั้นนี้ประกอบด้วยการส่งข้อมูลผ่านสื่อกลางทางกายภาพ เช่น สาย LAN, Wi-Fi, หรือ Fiber Optic รวมถึงการจัดการการเข้าถึงเครือข่าย เช่น การกำหนด MAC Address ใน Data Link Layer และการส่งข้อมูลผ่านช่องทางทางกายภาพใน Physical Layer
   - **อุปกรณ์ที่เกี่ยวข้อง**: Hub, Switch, Repeater, Network Interface Card (NIC)

### **2. Internet Layer (ชั้น 3)**
   - **หน้าที่หลัก**: การกำหนดเส้นทาง (Routing), ใช้ IP Address
   - **รายละเอียด**: ชั้นนี้รับผิดชอบในการกำหนดเส้นทางข้อมูลระหว่างเครื่องต่าง ๆ ในเครือข่าย โดยใช้อุปกรณ์เช่น Router และการใช้ที่อยู่ IP Address เพื่อนำข้อมูลจากเครื่องต้นทางไปยังปลายทางผ่านเส้นทางที่เหมาะสม
   - **อุปกรณ์ที่เกี่ยวข้อง**: Router, Layer 3 Switch
   - **โปรโตคอลที่เกี่ยวข้อง**: IP, ICMP

### **3. Transport Layer (ชั้น 4)**
   - **หน้าที่หลัก**: ส่งข้อมูลให้ถูกต้อง (TCP → เชื่อมต่อ, UDP → ไม่เชื่อมต่อ)
   - **รายละเอียด**: ชั้นนี้ทำหน้าที่ในการควบคุมการส่งข้อมูลระหว่างเครื่องต้นทางและปลายทาง โดยการแบ่งข้อมูลเป็นส่วนเล็ก ๆ (Segment) และทำการตรวจสอบความถูกต้อง เช่น การตรวจสอบความสมบูรณ์ของข้อมูลผ่าน TCP หรือการส่งข้อมูลแบบไม่เชื่อมต่อผ่าน UDP
   - **อุปกรณ์ที่เกี่ยวข้อง**: Firewall (ตรวจสอบ TCP/UDP)
   - **โปรโตคอลที่เกี่ยวข้อง**: TCP, UDP

### **4. Application Layer (ชั้น 5-7)**
   - **หน้าที่หลัก**: ติดต่อกับโปรแกรม เช่น HTTP, FTP, SMTP
   - **รายละเอียด**: ชั้นนี้เป็นที่ที่โปรแกรมต่าง ๆ เช่น เว็บเบราว์เซอร์, โปรแกรมอีเมล หรือ FTP Client ติดต่อกับเครือข่าย โดยทำหน้าที่ในการรับส่งข้อมูลระหว่างแอปพลิเคชันที่ทำงานในเครื่องกับระบบเครือข่าย
   - **อุปกรณ์ที่เกี่ยวข้อง**: Proxy Server, Load Balancer, Web Server
   - **โปรโตคอลที่เกี่ยวข้อง**: HTTP, FTP, SMTP, DNS
---

### 🗂 อุปกรณ์และโปรโตคอลในแต่ละชั้นของ TCP/IP Model
| Layer (TCP/IP)   | Mapping OSI       | อุปกรณ์ที่เกี่ยวข้อง | Protocols ที่เกี่ยวข้อง |
|-------------------|--------------------|------------------------|---------------------------|
| Application       | Layer 5-7         | Proxy, Load Balancer, Web Server | HTTP, HTTPS, FTP, SMTP, DNS |
| Transport         | Layer 4           | Firewall (Port Filter), Load Balancer | TCP, UDP |
| Internet          | Layer 3           | Router, Layer 3 Switch | IP, ICMP |
| Network Access    | Layer 1-2         | Switch, Hub, NIC, Access Point | Ethernet, PPP, ARP |

**โปรโตคอลหลักใน TCP/IP**
- Application → HTTP, DNS, FTP
- Transport → TCP, UDP
- Internet → IP, ICMP
- Network Access → Ethernet, ARP

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

