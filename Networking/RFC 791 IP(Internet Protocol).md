# RFC 791 – Internet Protocol (IPv4) Summary

## 1. Overview (ภาพรวม)
- กำหนดมาตรฐาน **IPv4** (ชั้น Network ใน OSI Model)
- หน้าที่:
  - **Addressing (การระบุตำแหน่ง):** ใช้ IP ระบุแหล่งที่มาและปลายทาง
  - **Routing (การเลือกเส้นทาง):** เลือกเส้นทางที่เหมาะสมเพื่อส่ง Packet
  - **Fragmentation (การแบ่งชิ้น):** แบ่งข้อมูลถ้า MTU ของเครือข่ายเล็กเกินไป

**ลักษณะของ IPv4:**
- Connectionless (ไม่ต้องสร้างการเชื่อมต่อก่อน)
- Best-Effort Delivery (ไม่รับประกันถึงปลายทาง)
- Stateless (ไม่มีการจำสถานะการสื่อสาร)

---

## 2. IPv4 Header Structure (โครงสร้าง Header)
ความยาว: 20–60 Bytes

```
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |Type of Service|          Total Length         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|      Fragment Offset    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |         Header Checksum       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Source Address                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Destination Address                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options (if any)         |    Padding      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

---

### คำอธิบาย Field
| Field                | ขนาด    | คำอธิบาย                                           |
|----------------------|---------|------------------------------------------------------|
| Version             | 4 bits   | หมายเลขเวอร์ชัน (IPv4 = 4)                         |
| IHL                 | 4 bits   | ความยาว Header (หน่วย 32 บิต word)                |
| Type of Service     | 8 bits   | การตั้งค่าคุณภาพบริการ (QoS, DSCP)                |
| Total Length        | 16 bits  | ความยาวรวมของ Packet (Header + Data)               |
| Identification      | 16 bits  | ใช้สำหรับ Fragmentation                             |
| Flags               | 3 bits   | ควบคุม Fragment (DF = ห้ามแตก, MF = มีส่วนต่อ)    |
| Fragment Offset     | 13 bits  | ตำแหน่งของ Fragment ใน Datagram                     |
| TTL                 | 8 bits   | อายุของ Packet ลดลงเมื่อผ่าน Router                 |
| Protocol            | 8 bits   | โปรโตคอลที่อยู่ใน Payload (TCP=6, UDP=17, ICMP=1) |
| Header Checksum     | 16 bits  | ตรวจสอบความถูกต้องของ Header                        |
| Source Address      | 32 bits  | IP ต้นทาง                                           |
| Destination Address | 32 bits  | IP ปลายทาง                                          |
| Options + Padding   | Var.(แปรผัน)  | ใช้สำหรับการทำงานพิเศษ                              |

---

## 3. IPv4 Key Mechanisms (กลไกสำคัญตาม RFC 791)

## 1. Type of Service (ToS) – บอกคุณภาพของบริการ
- **ตำแหน่งใน Header:** 8 บิต (ปัจจุบันใช้ DSCP)
- **หน้าที่:** ระบุความต้องการของแพ็กเก็ต เช่น:
  - Low Delay → สำหรับ VoIP, วิดีโอคอล
  - High Throughput → สำหรับโหลดไฟล์ใหญ่
- **การทำงาน:** Router ใช้ค่านี้ช่วยเลือกเส้นทางหรือวิธีส่งที่เหมาะสม

**ตัวอย่างการใช้งาน:**
- VoIP → ToS = Low Delay
- FTP Download → ToS = High Throughput

---

## 2. TTL (Time To Live) – ป้องกันการวนลูป
- **ตำแหน่งใน Header:** 8 บิต
- **หน้าที่:** จำกัดอายุของแพ็กเก็ต ป้องกันการวนลูป (Routing Loop)
- **วิธีทำงาน:** ค่า TTL ลดลง 1 ทุกครั้งที่ผ่าน Router
- ถ้า TTL = 0 → Router ทิ้งแพ็กเก็ต และส่ง ICMP Error กลับ

**ตัวอย่าง:**
- เริ่ม TTL = 64 → ผ่าน 5 Router → TTL = 59
- ถ้าวนลูปจน TTL = 0 → Packet ถูก Drop

---

## 3. Fragmentation – แบ่งแพ็กเก็ตให้เหมาะกับ MTU
- **เหตุผล:** แต่ละเครือข่ายมีขนาด MTU ต่างกัน (Ethernet = 1500 bytes)
- ถ้า Packet ใหญ่เกิน MTU → ต้องแตกเป็น Fragment
- ใช้ 3 ฟิลด์ใน IPv4 Header:
  - Identification → หมายเลขของ Datagram เดิม
  - Flags → DF (Don't Fragment), MF (More Fragment)
  - Fragment Offset → ระบุตำแหน่งของชิ้นส่วนในข้อมูลเดิม

**ตัวอย่างการแตก Fragment:**
- ข้อมูล 4000 bytes, MTU = 1500 bytes
- จะได้:
  - Fragment 1 = 1480 bytes (20 bytes เป็น Header)
  - Fragment 2 = 1480 bytes
  - Fragment 3 = 1040 bytes

---

## 4. Header Checksum – ตรวจสอบความถูกต้องของ Header
- **ตำแหน่งใน Header:** 16 บิต
- **หน้าที่:** ตรวจสอบเฉพาะ IPv4 Header (ไม่รวม Data)
- **การทำงาน:** ทุกครั้งที่ Packet ผ่าน Router → ต้องคำนวณใหม่เพราะ TTL ลดลง
- ถ้า Checksum ไม่ตรง → Packet เสีย → Drop

**วิธีคำนวณ:**
- บวกทุกค่าของ Header (เป็น 16 บิต)
- ทำ One's Complement
- เปรียบเทียบกับค่าในฟิลด์ Checksum

---

### สรุปสั้นๆ
| กลไก            | หน้าที่                                      |
|------------------|---------------------------------------------|
| ToS             | ระบุคุณภาพบริการ (QoS)                    |
| TTL             | จำกัดอายุแพ็กเก็ต ป้องกัน Loop            |
| Fragmentation   | แบ่ง/รวมแพ็กเก็ตให้เหมาะกับ MTU          |
| Header Checksum | ตรวจสอบความถูกต้องของ Header              |

---

## 4. ตัวอย่างจาก Wireshark
```
Internet Protocol Version 4, Src: 192.168.1.10, Dst: 8.8.8.8
    Version: 4
    Header Length: 20 bytes
    Total Length: 60
    TTL: 64
    Protocol: TCP (6)
    Header checksum: 0xb1e6 [correct]
    Source: 192.168.1.10
    Destination: 8.8.8.8
```

---

## 5. จุดเด่นและข้อจำกัด
**ข้อดี:**
- ทำงานได้หลากหลายเครือข่าย (Flexible, Interoperable)

**ข้อจำกัด:**
- Address จำกัด (~4.3 พันล้าน IP)
- ไม่มีระบบความปลอดภัยในตัว → ต้องใช้ IPsec
- ไม่รับประกันการส่งถึงและลำดับข้อมูล

---

## 6. RFC ที่เกี่ยวข้อง
- RFC 768 – UDP
- RFC 793 – TCP
- RFC 792 – ICMP
- RFC 1918 – Private IP
- RFC 2474 – DSCP
