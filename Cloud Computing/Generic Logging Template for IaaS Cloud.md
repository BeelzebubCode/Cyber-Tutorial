
# 📦 A Generic Logging Template for IaaS Cloud

> **ผู้เขียน:** Winai Wongthai, Francisco Liberal Rocha, Aad van Moorsel  
> **สังกัด:** School of Computing Science, Newcastle University  
> **ปีที่ตีพิมพ์:** 2013  
> **วัตถุประสงค์:** เสนอ template การออกแบบ Logging System สำหรับ IaaS เพื่อช่วยตรวจสอบความรับผิดชอบ และลดความเสี่ยงจากภัยคุกคาม Cloud

---

## 📚 สารบัญ

- [1. บทนำ](#1-บทนำ)
- [2. สถาปัตยกรรม IaaS](#2-สถาปัตยกรรม-iaas)
- [3. ภัยคุกคาม CSA ทั้ง 7 ข้อ](#3-ภัยคุกคาม-csa-ทั้ง-7-ข้อ)
- [4. Logging System Template](#4-logging-system-template)
- [5. องค์ประกอบ Px และ Fy](#5-องค์ประกอบ-px-และ-fy)
- [6. กรณีศึกษา: ตรวจจับ Spam Mail](#6-กรณีศึกษา-ตรวจจับ-spam-mail)
- [7. วิเคราะห์ความปลอดภัย](#7-วิเคราะห์ความปลอดภัย)
- [8. สรุปและประโยชน์](#8-สรุปและประโยชน์)

---

## 1. บทนำ

Logging system คือระบบที่บันทึกพฤติกรรมใน IaaS cloud เพื่อ:
- ตรวจสอบย้อนหลัง
- พิสูจน์ความรับผิดชอบ
- แก้ปัญหาและตรวจจับภัยคุกคาม

Logging System = Logging Processes (Px) + Log Files (Fy)

---

## 2. สถาปัตยกรรม IaaS

ประกอบด้วย 2 ฝั่งหลัก:

- **Provider Side**: hw → hypervisor → dom0 → domUs  
- **Customer Side**: ผู้ใช้งานเช่า domU ผ่าน Internet

🖼️ *Figure 1: The IaaS architecture*  
![Figure 1](94fc84ba-2463-46d6-935e-649fa44071a5.png)

---

## 3. ภัยคุกคาม CSA ทั้ง 7 ข้อ (ขยายความ)

| # | ชื่อภัยคุกคาม | ลักษณะการโจมตี | ตัวอย่างใน IaaS |
|--:|----------------|-----------------|------------------|
| 1 | **Abuse and Nefarious Use of Cloud Services** | ผู้ไม่หวังดีใช้ VM ที่เช่ามาทำกิจกรรมผิดกฎหมาย | สร้าง botnet, ส่ง spam mail, สแกนพอร์ต |
| 2 | **Insecure Interfaces and APIs** | ช่องโหว่ของ API ที่เปิดให้ผู้ใช้เรียกใช้งานระบบ | โจมตีผ่าน API ที่ไม่มีการ auth (unauthenticated REST endpoint) |
| 3 | **Malicious Insiders** | พนักงานในองค์กร cloud ใช้สิทธิ์ผิดวัตถุประสงค์ | Admin ของ dom0 ดู log ลูกค้า หรือแอบแก้ไขข้อมูล |
| 4 | **Shared Technology Issues** | การใช้ฮาร์ดแวร์ร่วมกันที่ไม่มีการแบ่งแยกอย่างปลอดภัย | side-channel attack เช่น Meltdown, Spectre |
| 5 | **Data Loss or Leakage** | ข้อมูลสูญหายหรือถูกขโมยจากระบบ cloud | VM crash แล้ว data บน disk หาย, การ config S3 ผิด |
| 6 | **Account or Service Hijacking** | บัญชีผู้ใช้ถูกแฮ็กหรือขโมย token access | ขโมย SSH key → สร้าง VM โดยไม่รู้ตัว |
| 7 | **Unknown Risk Profile** | ผู้ใช้ไม่รู้ว่า provider ปกป้องข้อมูลแค่ไหน | ไม่รู้ว่า log เก็บไว้ที่ไหน? ใครมีสิทธิ์ดู? หรือ VM แชร์กับใคร?

---

## 4. Logging System Template

Template ประกอบด้วยตำแหน่งวาง Logging Process (Px) และ Log File (Fy)  
เพื่อตรวจจับพฤติกรรมผิดปกติในระดับต่าง ๆ เช่น domU, dom0, hypervisor

🖼️ *Figure 2: Logging System Template*  
![Figure 2](1fcdfacd-03ed-41a6-9cd0-acc93e4a4fe2.png)

---

---

## 5. องค์ประกอบ Px และ Fy

### Px – Logging Process

| Px | ตำแหน่งในระบบ | ตัวอย่าง | หน้าที่หลัก |
|----|------------------|----------|--------------|
| P1 | dom0 user level | libVMI | อ่าน memory ของ VM |
| P2 | dom0 kernel | Flogger | ดัก syscall, file, network |
| P3 | domU kernel | Flogger | ดัก syscall ใน VM |
| P4 | hypervisor | Logger | ดัก network ระหว่าง domUs |

### Fy – Log File

| Fy | ตำแหน่งจัดเก็บ | ประเภท | อธิบาย |
|----|------------------|--------|---------|
| F1 | diskU | ชั่วคราว | log ชั่วคราวจาก domU |
| F2 | memU | ชั่วคราว | log ใน memory domU |
| F3 | disk0 | ถาวร | log ถาวรใน provider |
| F4 | mem0 | ชั่วคราว | log ชั่วคราวใน dom0 |

> ⚠️ ไม่มี Px และ Fy ใน domU user-level เพราะอาจถูก user ปิดหรือหลีกเลี่ยงได้ง่าย  
> Fy ที่อยู่ใน diskU หรือ memU มีความเสี่ยงด้านความมั่นคงปลอดภัย  
> Fy ใน disk0 (F3) เป็น log ถาวร แต่ควรมีมาตรการป้องกันไม่ให้ provider อ่านโดยพลการ

🖼️ *Figure 3: Mapping HP Flogger P2 and P3 on template*  
![Figure 3](8e777ab6-225a-464b-8aea-1dff702245a9.png)

---

## 6. กรณีศึกษา: ตรวจจับ Spam Mail

### 🎯 เป้าหมาย:
จับคำสั่งที่ส่ง spam mail จาก domU → วิเคราะห์ → บันทึกเป็นหลักฐาน

### 📥 คำสั่งต้องสงสัย:
```bash
mail -s spamSubject winai.wongthai@ncl.ac.uk
```

### 🧠 อธิบายการทำงาน:
- ใช้ `libVMI` (P1) ใน dom0 เพื่ออ่าน memU
- ดึงค่าพารามิเตอร์จากคำสั่ง mail:
  - ag1 = `-s`
  - ag2 = `spamSubject`
  - ag3 = `winai.wongthai@ncl.ac.uk`
- บันทึกลงฐานข้อมูลใน Fy = F3 (disk0)

🖼️ *Figure 4: Logging system เพื่อตรวจจับ spam (CSA Threat #1)*  
![Figure 4](1cdc66ee-34ee-4958-83a5-ced47a94d645.png)

🖼️ *Figure 5: ตัวอย่าง output จาก Logging App (LA)*  
![Figure 5](c6677e1a-644f-4553-bc6d-8a417ac00397.png)

### Px – Logging Process

| Px | ตำแหน่งในระบบ | ตัวอย่าง | หน้าที่หลัก |
|----|------------------|----------|--------------|
| P1 | dom0 user level | libVMI | อ่าน memory ของ VM |
| P2 | dom0 kernel | Flogger | ดัก syscall, file, network |
| P3 | domU kernel | Flogger | ดัก syscall ใน VM |
| P4 | hypervisor | Logger | ดัก network ระหว่าง domUs |

### Fy – Log File

| Fy | ตำแหน่งจัดเก็บ | ประเภท | อธิบาย |
|----|------------------|--------|---------|
| F1 | diskU | ชั่วคราว | log ชั่วคราวจาก domU |
| F2 | memU | ชั่วคราว | log ใน memory domU |
| F3 | disk0 | ถาวร | log ถาวรใน provider |
| F4 | mem0 | ชั่วคราว | log ชั่วคราวใน dom0 |

---

### 🧠 อธิบายการทำงานแบบละเอียดของ Logging กรณี spam

เมื่อผู้ใช้บน VM (domU) ป้อนคำสั่งต้องสงสัย:

```bash
mail -s spamSubject winai.wongthai@ncl.ac.uk
```

ระบบ Logging ที่ออกแบบไว้จะทำงานตามลำดับขั้นตอนดังนี้:

---

#### 🧩 Step-by-step การทำงานของ Logging Template:

1. **ผู้ใช้ใน domU ป้อนคำสั่ง**
   - คำสั่ง mail มี argument ดังนี้:
     - `ag1 = -s`
     - `ag2 = spamSubject` (หัวข้ออีเมล)
     - `ag3 = winai.wongthai@ncl.ac.uk` (อีเมลผู้รับ)

2. **libVMI (P1 ใน dom0) ทำ introspection**
   - ไม่ต้องติดตั้ง agent ใน domU
   - ใช้ดูข้อมูล memory ของ domU (memU) จากภายนอก

3. **libVMI ตรวจพบ process "mail" ใน memory**
   - ดึง argument ทั้ง 3 ค่า ag1–ag3 ออกมา
   - วิเคราะห์ว่าเป็นคำสั่งส่งอีเมล

4. **Logging App (เช่น WallDo/LA) ดึงค่า ag2, ag3**
   - เขียนค่าดังกล่าวลงฐานข้อมูล
   - อาจรวม metadata เช่น user ที่ใช้, timestamp

5. **เก็บ log ลง Fy = F3 (disk0)**
   - เพื่อการเก็บแบบถาวรที่ auditor สามารถตรวจสอบได้ภายหลัง

---

#### 🔐 จุดเด่นของ libVMI ในการ Logging

| ข้อดี | อธิบาย |
|-------|--------|
| 📦 ไม่ต้องติดตั้งใน domU | ป้องกันการถูกปิดจาก attacker |
| 🔐 ทำงานใน dom0 | สิทธิสูง มองเห็นทุก domU |
| 📖 ดึงข้อมูลจาก memory โดยตรง | แม้ไม่มี log file ก็เห็นพฤติกรรมได้ |

---

#### 🔁 สรุปภาพการทำงาน

```text
User (domU)
 ↓
[mail -s spamSubject email] ← Suspicious
 ↓
Memory in domU (memU)
 ↓
libVMI (P1 in dom0) อ่าน memU
 ↓
ดึง ag2, ag3 ออกมา
 ↓
Logging App (LA) → บันทึกข้อมูล
 ↓
Fy = F3 (disk0) ← เก็บ log ถาวร
```

---

## 7. วิเคราะห์ความปลอดภัย

- log (F3) ถูกเก็บที่ disk0 → ผู้ให้บริการมีสิทธิเข้าถึง
- ความเสี่ยง: ผู้ให้บริการอาจอ่าน/แก้ไข log
- จำเป็นต้องเสริมความปลอดภัย เช่น encryption, access control

---

## 8. สรุปและประโยชน์

✅ Template นี้ช่วยให้:
- ออกแบบ logging system ได้ตามความต้องการ
- วิเคราะห์ความปลอดภัยก่อนใช้งานจริง
- ตรวจจับ threat ของ CSA ได้อย่างมีระบบ

> ⚙️ สามารถนำไปประยุกต์ใช้กับระบบจริงหรือวิจัยในอนาคตเกี่ยวกับ cloud accountability และ forensic investigation
