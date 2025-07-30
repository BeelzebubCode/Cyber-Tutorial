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

## 3. ภัยคุกคาม CSA ทั้ง 7 ข้อ

1. Abuse of Cloud Resources (เช่น spam, botnet)
2. Insecure APIs
3. Malicious Insiders
4. Shared Technology Issues
5. Data Loss / Leakage
6. Account Hijacking
7. Unknown Risk Profile

---

## 4. Logging System Template

Template ประกอบด้วยตำแหน่งวาง Logging Process (Px) และ Log File (Fy)  
เพื่อตรวจจับพฤติกรรมผิดปกติในระดับต่าง ๆ เช่น domU, dom0, hypervisor

🖼️ *Figure 2: Logging System Template*

![Figure 2](1fcdfacd-03ed-41a6-9cd0-acc93e4a4fe2.png)

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

---

## 6. กรณีศึกษา: ตรวจจับ Spam Mail

### 🎯 เป้าหมาย:
จับคำสั่งที่ส่ง spam mail จาก domU → วิเคราะห์ → บันทึกเป็นหลักฐาน

### 📥 คำสั่งต้องสงสัย:
```bash
mail -s spamSubject winai.wongthai@ncl.ac.uk
```
→ ag1 = `-s`, ag2 = `spamSubject`, ag3 = `winai.wongthai@ncl.ac.uk`

### 🔧 วิธีการ:
- ใช้ `libVMI` (P1) ใน dom0 เพื่ออ่าน memU
- สกัดค่า ag2, ag3
- บันทึกลงฐานข้อมูล (F3)

🖼️ *Figure 4: Logging system เพื่อตรวจจับ spam (CSA Threat #1)*

![Figure 4](1cdc66ee-34ee-4958-83a5-ced47a94d645.png)

---

### 🧪 ผลลัพธ์:
เมื่อ domU ส่ง mail → LA (Logging App) ใน dom0 ดึงข้อมูลจาก memory  
แล้วบันทึก subject + email address เป็น log

🖼️ *Figure 6: ตัวอย่าง output จาก Logging App*

![Figure 6](c6677e1a-644f-4553-bc6d-8a417ac00397.png)

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

