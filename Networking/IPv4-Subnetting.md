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

## 📝 ตาราง Quick Subnet Method (แนวนอน)

| Mask Decimal | 255 | 254 | 252 | 248 | 240 | 224 | 192 | 128 |
|--------------|-----|-----|-----|-----|-----|-----|-----|-----|
| Mask Binary  |11111111|11111110|11111100|11111000|11110000|11100000|11000000|10000000|
| Block Size   | 1   | 2   | 4   | 8   | 16  | 32  | 64  | 128 |

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

## 📝 โจทย์ฝึกหัด (5 ข้อ)
**ลำดับจากง่ายไปยาก พร้อมหัวข้อสรุปวิธีคิด**

1. 🏁 **โจทย์ 1: แบ่ง 2 Subnets จาก 192.168.0.0/24**
   - ต้องการ 2 เครือข่ายย่อยขนาดเท่ากัน

2. 🥈 **โจทย์ 2: แบ่ง 4 Subnets จาก 10.0.0.0/24**
   - ต้องการ 4 เครือข่ายย่อยขนาดเท่ากัน

3. 🥉 **โจทย์ 3: หาจำนวน Host usable ต่อ Subnet จาก 172.16.0.0/20**
   - เมื่อใช้ prefix /20 ให้หาจำนวน Host usable ต่อ Subnet

4. 🎯 **โจทย์ 4: VLSM บน 192.168.2.0/24**
   - สร้าง 3 เครือข่ายย่อย รองรับ 50 Hosts, 20 Hosts, และ 10 Hosts ตามลำดับ

5. 🔥 **โจทย์ 5: แบ่ง 10 Subnets จาก 10.0.0.0/16**
   - ต้องการ 10 เครือข่ายย่อยขนาดเท่ากัน จาก prefix /16

---

## 🔗 แหล่งอ้างอิง

- [RFC 950 – Internet Subnetting](https://datatracker.ietf.org/doc/html/rfc950)
- [StationX Cheat Sheet](https://www.stationx.net/category/cheat-sheets/)
