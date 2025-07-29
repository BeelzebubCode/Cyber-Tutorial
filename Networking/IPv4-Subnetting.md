# 📡 IPv4 Subnetting

## 📋 สารบัญ
- [🔎 Subnetting คืออะไร](#subnetting-คืออะไร)
- [📊 โครงสร้าง IP Address](#โครงสร้าง-ip-address)
- [📚 IP Classes](#ip-classes)
- [🧠 แนวคิดการ Subnet](#แนวคิดการ-subnet)
- [📐 สูตรสำคัญที่ใช้](#สูตรสำคัญที่ใช้)
- [📈 ตาราง Cheat Sheet Subnet](#ตาราง-cheat-sheet-subnet)
- [🧪 ตัวอย่างโจทย์ Subnetting](#ตัวอย่างโจทย์-subnetting)
- [🔧 ขั้นตอนการทำ Subnetting](#ขั้นตอนการทำ-subnetting)
- [📌 ข้อควรจำ](#ข้อควรจำ)
- [📝 โจทย์ฝึกหัด (5 ข้อ)](#โจทย์ฝึกหัด-5-ข้อ)
- [💡 วิธีทำโจทย์](#วิธีทำโจทย์)
- [🔗 แหล่งอ้างอิง](#แหล่งอ้างอิง)

---

## 🔎 Subnetting คืออะไร

**Subnetting** คือการแบ่งเครือข่ายหลัก (Network) ออกเป็นเครือข่ายย่อย (Subnets) เพื่อจัดการ IP address ได้อย่างมีประสิทธิภาพ และลดการกระจายของ broadcast traffic ภายในเครือข่าย

---

## 📊 โครงสร้าง IP Address

IPv4 มีความยาว 32 บิต แบ่งเป็น 4 Octet (8 บิต ต่อ Octet) เช่น:
```bash
192.168.1.10 = 11000000.10101000.00000001.00001010
```
- **Network Portion**: บอกว่าอยู่เครือข่ายใด
- **Host Portion**: บอกว่าเครื่องใดในเครือข่ายนั้น

---

## 📚 IP Classes

| Class | Range       | Network Bits | Host Bits | Hosts/Net   |
|-------|-------------|--------------|-----------|-------------|
| A     | 1 - 126     | 8            | 24        | 16,777,214  |
| B     | 128 - 191   | 16           | 16        | 65,534      |
| C     | 192 - 223   | 24           | 8         | 254         |

---

## 🧠 แนวคิดการ Subnet

การ Subnet คือการ "ขโมย" บิตจากส่วน Host มาเป็น Network Bits:

```text
Original: /24 → Network 24 bits, Host 8 bits
Borrow 2: /26 → Network 26 bits, Host 6 bits
```

ยิ่งขโมยบิตมาก Subnet ได้มาก แต่ Host ต่อ Subnet จะลดน้อยลง

---

## 📐 สูตรสำคัญที่ใช้

- **จำนวน Subnets**: `2^n ≥ #Subnets` (n = บิตที่ยืมจาก Host)
- **Host usable**: `2^h - 2` (h = บิต Host หลัง Subnetting)
- **Subnet Mask**: prefix เดิม + n บิต เช่น `/24 → /26`
- **Block Size**: `256 - Decimal(mask octet)`
- **Range**:
  - Network: Host bits = 0
  - Broadcast: Host bits = 1
  - Usable IPs: ระหว่าง Network และ Broadcast

---

## 📈 ตาราง Cheat Sheet Subnet

| CIDR | Subnet Mask     | #IPs | Hosts/Net | Wildcard   |
|------|-----------------|------|-----------|------------|
| /24  |255.255.255.0    | 256  | 254       |0.0.0.255   |
| /25  |255.255.255.128  | 128  | 126       |0.0.0.127   |
| /26  |255.255.255.192  | 64   | 62        |0.0.0.63    |
| /27  |255.255.255.224  | 32   | 30        |0.0.0.31    |
| /28  |255.255.255.240  | 16   | 14        |0.0.0.15    |
| /29  |255.255.255.248  | 8    | 6         |0.0.0.7     |
| /30  |255.255.255.252  | 4    | 2         |0.0.0.3     |

---

## 🧪 ตัวอย่างโจทย์ Subnetting

**โจทย์**: แบ่ง `192.168.1.0/24` เป็น 4 Subnet

1. `2^n ≥ 4` → n = 2 → prefix = `/26`
2. Block Size = `256 - 192` = 64
3. Subnets:
   - 192.168.1.0/26  → .1 ถึง .62 (.63 broadcast)
   - 192.168.1.64/26 → .65 ถึง .126
   - 192.168.1.128/26→ .129 ถึง .190
   - 192.168.1.192/26→ .193 ถึง .254

---

## 🔧 ขั้นตอนการทำ Subnetting

1. อ่านโจทย์: #Subnets, #Hosts
2. class ของ IP
3. คำนวณ n จาก `2^n ≥ #Subnets`
4. prefix ใหม่ = เดิม + n
5. Block Size = `256 - mask octet`
6. แบ่งช่วงตาม Block Size
7. ระบุ Network / Usable / Broadcast

---

## 📌 ข้อควรจำ

- Network Address: Host = 0
- Broadcast: Host = 1
- Usable: ระหว่างทั้งสอง
- CIDR → `/xx`

---

## 📝 Quick Subnet Method

| Mask (decimal) | 128 | 192 | 224 | 240 | 248 | 252 | 254 | 255 |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|
| Block Size     | 128 | 64  | 32  | 16  | 8   | 4   | 2   | 1   |

---

## 📝 โจทย์ฝึกหัด (5 ข้อ) (5 ข้อ)

**1. Subnet 2 Subnets จาก 192.168.0.0/24**

**โจทย์:** แบ่ง 192.168.0.0/24 ให้ได้ 2 Subnets ขนาดเท่ากัน

**วิธีทำ:**
1. ต้องการ 2 Subnets → `2^n ≥ 2` → n = 1
2. Prefix ใหม่ = `/24 + 1` = `/25`
3. Mask Octet (4th) = 128 → Block Size = 256 - 128 = 128
4. Subnets:
   - 192.168.0.0/25 → Hosts .1 ถึง .126, Broadcast .127
   - 192.168.0.128/25 → Hosts .129 ถึง .254, Broadcast .255

---

**2. Subnet 4 Subnets จาก 10.0.0.0/24**

**โจทย์:** แบ่ง 10.0.0.0/24 ให้ได้ 4 Subnets

**วิธีทำ:**
1. `2^n ≥ 4` → n = 2 → prefix = `/26`
2. Mask Octet = 192 → Block = 256 - 192 = 64
3. Subnets:
   - 10.0.0.0/26 → .1–.62, .63 brd
   - 10.0.0.64/26 → .65–.126, .127
   - 10.0.0.128/26 → .129–.190, .191
   - 10.0.0.192/26 → .193–.254, .255

---

**3. หาจำนวน Host usable จาก 172.16.0.0/20**

**โจทย์:** ใช้ prefix /20

**วิธีทำ:**
1. Host bits = 32 - 20 = 12
2. usable = 2^12 - 2 = 4096 - 2 = 4094 hosts ต่อ Subnet

---

**4. VLSM บน 192.168.2.0/24**

**โจทย์:** ต้องการรองรับ
- 50 Hosts
- 20 Hosts
- 10 Hosts

**วิธีทำ:**
1. เรียงจากต้องการสูงสุด → 50 hosts
2. หา prefix สำหรับ 50 → `2^h - 2 ≥ 50` → h = 6 → usable 62 → prefix = /26
   - Subnet1: 192.168.2.0/26 → .1–.62, .63 brd
3. ถัดมา 20 hosts → `2^h - 2 ≥ 20` → h = 5 → usable 30 → prefix = /27
   - Subnet2: ถัดจาก .63 → 192.168.2.64/27 → .65–.94, .95 brd
4. 10 hosts → `2^h - 2 ≥ 10` → h = 4 → usable 14 → prefix = /28
   - Subnet3: 192.168.2.96/28 → .97–.110, .111 brd

---

**5. Subnet 10 Subnets จาก 10.0.0.0/16**

**โจทย์:** ต้องการ 10 Subnets เท่ากัน

**วิธีทำ:**
1. `2^n ≥ 10` → n = 4 (2^4 = 16)
2. Prefix ใหม่ = `/16 + 4` = `/20`
3. Mask Octet (3rd) = 240 → Block Size = 256 - 240 = 16 (สำหรับ third octet)
4. Subnets (16 possible, ใช้ 10 แรก):
   - 10.0.0.0/20 → network 10.0.0.0, range .1–.4094, brd .4095
   - 10.0.16.0/20
   - 10.0.32.0/20
   - … (เพิ่มทีละ 16) …
   - 10.0.144.0/20 (10th subnet)

---

## 💡 วิธีทำโจทย์
- ใช้สูตรคำนวณเบื้องต้นแล้วนำไปจัดเรียงช่วง IP ตาม Block Size
- VLSM เริ่มจาก Subnet ใหญ่สุดก่อน ลดการเสีย IP

---

## 🔗 แหล่งอ้างอิง
- [RFC 950 – Internet Standard Subnetting Procedure](https://datatracker.ietf.org/doc/html/rfc950)
- [StationX Cheat Sheet](https://www.stationx.net/category/cheat-sheets/)
