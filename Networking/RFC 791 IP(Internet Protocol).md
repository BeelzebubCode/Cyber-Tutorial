# RFC 791 – Internet Protocol (IPv4) Summary (พร้อมคำแปลภาษาไทย)

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
| Version             | 4 บิต   | หมายเลขเวอร์ชัน (IPv4 = 4)                         |
| IHL                 | 4 บิต   | ความยาว Header (หน่วย 32 บิต word)                |
| Type of Service     | 8 บิต   | การตั้งค่าคุณภาพบริการ (QoS, DSCP)                |
| Total Length        | 16 บิต  | ความยาวรวมของ Packet (Header + Data)               |
| Identification      | 16 บิต  | ใช้สำหรับ Fragmentation                             |
| Flags               | 3 บิต   | ควบคุม Fragment (DF = ห้ามแตก, MF = มีส่วนต่อ)    |
| Fragment Offset     | 13 บิต  | ตำแหน่งของ Fragment ใน Datagram                     |
| TTL                 | 8 บิต   | อายุของ Packet ลดลงเมื่อผ่าน Router                 |
| Protocol            | 8 บิต   | โปรโตคอลที่อยู่ใน Payload (TCP=6, UDP=17, ICMP=1) |
| Header Checksum     | 16 บิต  | ตรวจสอบความถูกต้องของ Header                        |
| Source Address      | 32 บิต  | IP ต้นทาง                                           |
| Destination Address | 32 บิต  | IP ปลายทาง                                          |
| Options + Padding   | แปรผัน  | ใช้สำหรับการทำงานพิเศษ                              |

---

## 3. Key Mechanisms (กลไกสำคัญ)
- **Type of Service (ToS):** บอกคุณภาพบริการที่ต้องการ
- **TTL:** ป้องกันการวนลูป (Loop) โดยตั้งอายุ Packet
- **Fragmentation:** แบ่ง Packet ให้เหมาะกับ MTU
- **Checksum:** ตรวจสอบความถูกต้องของ Header

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
