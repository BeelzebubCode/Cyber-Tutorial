# 📡 IPv4 Subnetting

## 📋 สารบัญ
- [🔎 Subnetting คืออะไร](#subnetting-คืออะไร)
- [📊 โครงสร้าง IP Address](#โครงสร้าง-ip-address)
- [📚 IP Classes (ละเอียด)](#ip-classes-ละเอียด)
- [🧠 แนวคิด Subnetting](#แนวคิด-subnetting)
- [📐 สูตรสำคัญ](#สูตรสำคัญ)
- [📝 ตาราง Quick Subnet Method](#ตาราง-quick-subnet-method)
- [🧪 ตัวอย่างโจทย์ Subnetting](#ตัวอย่างโจทย์-subnetting)
- [🔧 ขั้นตอนการ Subnet](#ขั้นตอนการ-subnet)
- [🥇 โจทย์ฝึกหัด (5 ข้อ)](#โจทย์ฝึกหัด-5-ข้อ)
- [🔗 แหล่งอ้างอิง](#แหล่งอ้างอิง)

---

## 🔎 Subnetting คืออะไร
**Subnetting** คือการแบ่งเครือข่ายหลัก (Network) ออกเป็นเครือข่ายย่อย (Subnet) เพื่อจัดการ IP address ได้มีประสิทธิภาพ และลดปริมาณ broadcast ภายในเครือข่าย

---

## 📊 โครงสร้าง IP Address
IPv4 มีความยาว 32 บิต แบ่งเป็น 4 Octet:
```text
192.168.1.10 = 11000000.10101000.00000001.00001010
```
- **Network Portion**: บอกว่าอยู่เครือข่ายใด
- **Host Portion**: บอกว่าเครื่องใดในเครือข่ายนั้น

---

## 📚 IP Classes (ละเอียด)
| Class | Range               | Default Mask    | Mask (Binary)             | Network Bits | Host Bits | Hosts/Net   | Special Use                |
|-------|---------------------|-----------------|---------------------------|--------------|-----------|-------------|----------------------------|
| A     | 1.0.0.0 - 126.255.255.255 | /8 (255.0.0.0)   | 11111111.00000000.00000000.00000000 | 8            | 24        | 16,777,214 | เครือข่ายขนาดใหญ่ (Loopback 127.0.0.0/8) |
| B     | 128.0.0.0 - 191.255.255.255 | /16 (255.255.0.0) | 11111111.11111111.00000000.00000000 | 16           | 16        | 65,534     | เครือข่ายขนาดกลาง       |
| C     | 192.0.0.0 - 223.255.255.255 | /24 (255.255.255.0)| 11111111.11111111.11111111.00000000 | 24           | 8         | 254        | เครือข่ายขนาดเล็ก (LAN)   |
| D     | 224.0.0.0 - 239.255.255.255 | N/A             | N/A                       | Multicast    | Multicast | Multicast  | IP Multicast               |
| E     | 240.0.0.0 - 255.255.255.255 | N/A             | N/A                       | Experimental | Experimental| Experimental| สำหรับการทดลอง            |

---

## 🧠 แนวคิด Subnetting
การ Subnet จะ "ขโมย" บิตจาก Host Portion มาเพิ่มใน Network Portion:
```text
เดิม: /24  -> 24 บิต Network, 8 บิต Host
ยืม 2 บิต: /26 -> 26 บิต Network, 6 บิต Host
```
<br/>
- ยืมบิตมาก → ได้ Subnet มาก แต่ Host ต่อ Subnet จะลดลง

---

## 📐 สูตรสำคัญ
- **จำนวน Subnets**: `2^n ≥ #Subnets`  
  (n = บิตที่ยืมจาก Host)
- **จำนวน Host usable ต่อ Subnet**: `2^h - 2`  
  (h = บิต Host หลัง Subnetting)
- **Subnet Mask**: prefix เดิม + n เช่น `/24` → `/26`
- **Block Size**: `256 - (decimal value ของ mask octet ที่เปลี่ยน)`
- **IP Range**:
  - Network Address: Host bits = 0
  - Broadcast Address: Host bits = 1
  - Usable Hosts: ระหว่าง Network และ Broadcast

---

## 📝 ตาราง Quick Subnet Method
ใช้จำ Mask (decimal) กับ Block Size แบบชัดเจน

| Mask (decimal) | 255 | 254 | 252 | 248 | 240 | 224 | 192 | 128 |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|
| Block Size     | 1   | 2   | 4   | 8   | 16  | 32  | 64  | 128 |

---

## 🧪 ตัวอย่างโจทย์ Subnetting
**โจทย์**: แบ่ง `192.168.1.0/24` เป็น 4 Subnets
1. `2^n ≥ 4` → n = 2 → `/26`
2. Block Size = `256 - 192` = 64
3. Subnets:
   - `192.168.1.0/26` → Hosts .1–.62, Broadcast .63
   - `192.168.1.64/26` → Hosts .65–.126, Broadcast .127
   - `192.168.1.128/26` → Hosts .129–.190, Broadcast .191
   - `192.168.1.192/26` → Hosts .193–.254, Broadcast .255

---

## 🔧 ขั้นตอนการ Subnet
1. อ่านโจทย์: #Subnets, #Hosts
2. เลือก Class IP
3. คำนวณ n จาก `2^n ≥ #Subnets`
4. Prefix ใหม่ = เดิม + n
5. คำนวณ Block Size
6. แบ่ง Subnet ตาม Block Size
7. ระบุ Network / Usable / Broadcast

---

## 🥇 โจทย์ฝึกหัด (5 ข้อ)
1. **2 Subnets จาก 192.168.0.0/24**
2. **4 Subnets จาก 10.0.0.0/24**
3. **Host usable จาก 172.16.0.0/20**
4. **VLSM บน 192.168.2.0/24 (50,20,10 hosts)**
5. **10 Subnets จาก 10.0.0.0/16**

---

## 🔗 แหล่งอ้างอิง
- [RFC 950 – Internet Standard Subnetting Procedure](https://datatracker.ietf.org/doc/html/rfc950)
- [StationX Cheat Sheet](https://www.stationx.net/category/cheat-sheets/)
