
# 📦 A Generic Logging Template for IaaS Cloud

> **ผู้เขียน:** Winai Wongthai, Francisco Liberal Rocha, Aad van Moorsel  
> **สังกัด:** School of Computing Science, Newcastle University  
> **ปีที่ตีพิมพ์:** 2013  
> **วัตถุประสงค์:** เสนอ template การออกแบบ Logging System สำหรับ IaaS เพื่อช่วยตรวจสอบความรับผิดชอบ และลดความเสี่ยงจากภัยคุกคาม Cloud

---

## 📚 สารบัญ

- [1. บทนำ](#1-บทนำ)
- [2. สถาปัตยกรรม IaaS](#2-สถาปัตยกรรม-iaas)
- [3. ภัยคุกคาม CSA ทั้ง 7 ข้อ](#3-ภัยคุกคาม-csa-ทั้ง-7-ข้อ-ขยายความ)
- [4. Logging System Template](#4-logging-system-template)
  - [🔍 เสริมความเข้าใจ: libVMI คืออะไร?](#-เสริมความเข้าใจ-libvmi-คืออะไร)
- [5. องค์ประกอบ Px และ Fy](#5-องค์ประกอบ-px-และ-fy)
- [6. กรณีศึกษา: ตรวจจับ Spam Mail](#6-กรณีศึกษา-ตรวจจับ-spam-mail)
  - [🧠 อธิบายการทำงานแบบละเอียดของ Logging กรณี spam](#-อธิบายการทำงานแบบละเอียดของ-logging-กรณี-spam)
  - [🔁 สรุปภาพการทำงาน](#-สรุปภาพการทำงาน)
  - [🔍 ลำดับการทำงานของ Logging System กรณีคำสั่ง spam (แบบมีคำอธิบายละเอียด)](#-ลำดับการทำงานของ-logging-system-กรณีคำสั่ง-spam-แบบมีคำอธิบายละเอียด)
- [7. วิเคราะห์ความปลอดภัย](#7-วิเคราะห์ความปลอดภัย)
- [8. สรุปและประโยชน์](#8-สรุปและประโยชน์)
- [🔗 อ้างอิง](#🔗-อ้างอิง)


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

![Figure 1](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/Img-Logging/figure1.png)

🖼️ *Figure 1: The IaaS architecture*  

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

![Figure 2](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/Img-Logging/figure2.png)

🖼️ *Figure 2: Logging System Template*  

### 🧩 อธิบายองค์ประกอบต่าง

| Component | Description/Functions | Examples | Locations in IaaS environment |
|-----------|------------------------|----------|-------------------------------|
| **hw0**   | Physical hardware โดย provider | a PC/server | in the provider side infrastructure |
| **disk0** | Physical disk ของ hw0 (ถูกจัดการโดย dom0) | server’s disks | physically in hw0 |
| **mem0**  | Main memory ของ hw0 | server’s main memory | physically in hw0 |
| **hypervisor** | รัน dom0 และ domU | Xen | on top of hw0 |
| **dom0** | รันโดย provider (มี app0) | Fedora 16 dom0 | on top of the hypervisor |
| **app0** | แอปของ dom0 | logging-related application | in dom0 user level |
| **domU** | VM ของลูกค้า (มี appU, hwU) | Fedora 16 domU | on top of the hypervisor |
| **appU** | แอปของ user ใน domU | mail command | in domU user level |
| **hwU** | virtual hardware ของ domU | - | virtually in domU |
| **diskU** | virtual disk ของ domU | - | virtually in hwU |
| **memU** | virtual memory ของ domU | - | virtually in hwU |

---

## 🔍 เสริมความเข้าใจ: libVMI คืออะไร?

**libVMI** (Virtual Machine Introspection Library) คือเครื่องมือ open-source ที่ใช้สำหรับอ่านข้อมูลใน memory ของ VM **โดยไม่ต้องเข้าไปใน VM นั้นโดยตรง**

### ✅ จุดเด่นของ libVMI
- ทำงานจาก **dom0 หรือ hypervisor layer**
- **มองเห็น process, memory และกิจกรรมใน VM ได้แบบ stealthy**
- เหมาะสำหรับงาน **forensics, malware analysis, และการตรวจสอบพฤติกรรมต้องสงสัย**

### 🧠 ใช้อย่างไรในเอกสารนี้?
ในระบบ Logging Template:
- **P1 (Logging Process)** ใช้ libVMI เพื่อดึงข้อมูลจาก memU (memory ของ domU)
- ใช้ตรวจจับคำสั่งต้องสงสัย เช่น `mail -s spamSubject ...` ที่อยู่ใน memory
- ไม่มีความจำเป็นต้องติดตั้ง logging agent ภายใน domU → ป้องกันการถูกปิดหรือหลบเลี่ยง

> ⚠️ อย่างไรก็ตาม การใช้งาน libVMI ใน production จริงยังคงมีข้อจำกัดด้าน performance และความซับซ้อนในการตั้งค่า

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

![Figure 3](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/Img-Logging/figure3.png)

🖼️ *Figure 3: Mapping HP Flogger P2 and P3 on template*  

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

![Figure 4](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/Img-Logging/figure4.png)

🖼️ *Figure 4: Logging system เพื่อตรวจจับ spam (CSA Threat #1)*  

![Figure 5](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/Img-Logging/figure5.png)

🖼️ *Figure 5: ตัวอย่าง output จาก Logging App (LA)*  

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

### 🔍 ลำดับการทำงานของ Logging System กรณีคำสั่ง spam (แบบมีคำอธิบายละเอียด)

```text
User (domU)
↓
[mail -s spamSubject email] ← Suspicious
  ผู้ใช้ภายใน VM (domU) รันคำสั่งส่งอีเมล โดยใช้คำสั่ง `mail` พร้อม argument เช่น
  - ag1 = -s (ระบุหัวข้อ)
  - ag2 = spamSubject (หัวข้ออีเมล)
  - ag3 = winai.wongthai@ncl.ac.uk (ผู้รับ)
↓
Memory in domU (memU)
  เมื่อคำสั่งถูกรัน ข้อมูลที่เกี่ยวข้องจะถูกโหลดเข้าสู่ RAM ของ domU (เรียกว่า memU)
  ซึ่งตรงนี้คือแหล่งข้อมูลที่ยังไม่ถูกเขียนลง disk และสามารถถูกอ่านโดยภายนอกได้
↓
libVMI (P1 in dom0) อ่าน memU
  Px = P1 ทำงานใน dom0 (user-level) โดยใช้ library ชื่อ `libVMI` เพื่อ introspect หรือแอบดูข้อมูลใน memU
  ซึ่งไม่ต้องเข้าไปใน VM ก็สามารถสืบข้อมูลในหน่วยความจำของ VM ได้
↓
ดึง ag2, ag3 ออกมา
  libVMI ตรวจพบว่า process `mail` กำลังทำงาน
  จึงดึงข้อมูล argument ที่สำคัญออกมาจาก memory ได้แก่:
  - ag2 = spamSubject
  - ag3 = winai.wongthai@ncl.ac.uk
↓
Logging App (LA) → บันทึกข้อมูล
  แอปพลิเคชัน Logging App (เช่น WallDo) บน dom0 จะจัดเก็บค่าที่ดึงได้ลงในระบบฐานข้อมูล
  พร้อม timestamp, process ID, และข้อมูลอื่น ๆ ประกอบการพิสูจน์
↓
Fy = F3 (disk0) ← เก็บ log ถาวร
  ข้อมูล log เหล่านี้จะถูกจัดเก็บอย่างถาวรไว้ใน Fy = F3 ซึ่งคือ disk0 บนฝั่งของ provider
  เพื่อรองรับการ audit หรือตรวจสอบย้อนหลัง เช่น forensic investigation
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

---

## 🔗 อ้างอิง
- [libVMI – GitHub](https://github.com/libvmi/libvmi)
- [CSA Top Threats 2013](https://cloudsecurityalliance.org/artifacts/the-notorious-nine-cloud-computing-top-threats-in-2013/)
