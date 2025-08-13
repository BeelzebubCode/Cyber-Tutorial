# 🧱 A Generic Logging Template for IaaS Cloud

> จากงานวิจัย: *"A Generic Logging Template for Infrastructure as a Service Cloud"*  
> โดย Winai Wongthai, Francisco Liberal Rocha, Aad van Moorsel (Newcastle University)

---

## 📑 สารบัญ

- [📌 ภาพรวม Logging Template](#-ภาพรวม-logging-template)
- [🖼️ แผนภาพสถาปัตยกรรม IaaS (Figure 1)](#-แผนภาพสถาปัตยกรรม-iaas-figure-1)
- [🧩 องค์ประกอบของ Template (Figure 2)](#-องค์ประกอบของ-template-figure-2)
  - [📦 ชุด IaaS Components](#-ชุด-iaas-components)
  - [⚙️ Logging Processes (Px)](#️-logging-processes-px)
  - [🗃️ Log Files (Fy)](#️-log-files-fy)
- [🗂️ ความสำคัญของ Critical Files และ Threat 1](#-ความสำคัญของ-critical-files-และ-threat-1)
  - [📁 Customers’ Critical Files in domUs](#-customers-critical-files-in-domus)
  - [🚨 Threat 1: Abuse of Cloud to Attack](#-threat-1-abuse-of-cloud-to-attack)
- [📈 เทคนิค Logging ขั้นสูงเพื่อจัดการ Threat 1](#-เทคนิค-logging-ขั้นสูงเพื่อจัดการ-threat-1)
  - [📁 A. History of Critical Files](#-a-history-of-critical-files-file-centric-log)
  - [⚙️ B. Process Behaviour Log](#-b-process-behaviour-log)
- [✉️ Case Study: จับคำสั่ง Spam ด้วย Behaviour Log](#-case-study-จับคำสั่ง-spam-ด้วย-behaviour-log)
- [🏗️ System Architecture ของ Logging Solution](#-system-architecture-ของ-logging-solution)
- [🔒 การวิเคราะห์ความปลอดภัย](#-การวิเคราะห์ความปลอดภัย)
- [✅ สรุป](#-สรุป)
- [🔗 อ้างอิง](#-อ้างอิง)

---

## 📌 ภาพรวม Logging Template

Template นี้ออกแบบมาเพื่อวิเคราะห์และสร้างระบบ Logging ที่สามารถใช้ตรวจสอบพฤติกรรมในระบบ IaaS และช่วยลดความเสี่ยงจากภัยคุกคามต่าง ๆ ที่กำหนดโดย CSA (Cloud Security Alliance)

จุดประสงค์หลัก:
- ลดความเสี่ยงจาก 7 อันดับภัยคุกคามของ CSA
- วิเคราะห์ความมั่นคงปลอดภัยของระบบ Logging ก่อนใช้งาน
- สนับสนุนแนวคิด Accountability และการตรวจสอบย้อนหลัง (Auditability)

---

## 🖼️ แผนภาพสถาปัตยกรรม IaaS (Figure 1)

> IaaS แบบ Xen-based ประกอบด้วย dom0, domU, hypervisor และ hardware

![Figure 1 - IaaS Architecture](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/Img-Logging/figure1.png)

- `dom0` เป็นระบบที่มีสิทธิ์สูง ควบคุมการสร้าง/ลบ domU  
- `domU` เป็น VM ที่ลูกค้าเช่าใช้งาน  
- `hypervisor` เป็นซอฟต์แวร์จัดการ VM  
- `hw` คือ physical server จริง

---

## 🧩 องค์ประกอบของ Template (Figure 2)

แสดงตำแหน่งของ Logging Process (Px) และ Log File (Fy) บนโครงสร้าง IaaS  
ใช้เพื่อสร้างระบบ Logging ใหม่ และวิเคราะห์ความเสี่ยงจากการจัดวาง

![Figure 2 - Logging Template](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/Img-Logging/figure2.png)

---

### 📦 ชุด IaaS Components

| Component | คำอธิบาย | ตำแหน่งใน IaaS |
|----------|------------|----------------|
| `hw0`    | ฮาร์ดแวร์จริงของระบบ | Provider |
| `disk0`  | Physical Disk ของ hw0 | hw0 |
| `mem0`   | Main Memory ของ hw0 | hw0 |
| `hypervisor` | ซอฟต์แวร์จัดการ VM | ระหว่าง hw0 ↔ dom0 |
| `dom0`   | ระบบปฏิบัติการที่มีสิทธิ์ Root | Provider |
| `app0`   | Logging App ที่รันใน dom0 | dom0 |
| `domU`   | Guest OS ที่ลูกค้าใช้งาน | User |
| `appU`   | แอปของลูกค้าใน domU | User |
| `hwU`    | ฮาร์ดแวร์เสมือนของ domU | Virtual Layer |
| `diskU`  | Virtual Disk ใน domU | บน disk0 จริง |
| `memU`   | Virtual Memory ใน domU | บน mem0 จริง |

---

### ⚙️ Logging Processes (Px)

| Px | ตำแหน่ง | หน้าที่ |
|----|----------|----------|
| `P1` | dom0 user | อ่าน memU ผ่าน libVMI |
| `P2` | dom0 kernel | ดักจับ network/file ops |
| `P3` | domU kernel | ดักจับ system call ของ domU |
| `P4` | hypervisor | ดัก network packet ของ domU |

---

### 🗃️ Log Files (Fy)

| Fy | ตำแหน่ง | ลักษณะข้อมูล |
|----|----------|--------------|
| `F1` | diskU | log ชั่วคราว |
| `F2` | memU | log ชั่วคราว |
| `F3` | disk0 | log ถาวร |
| `F4` | mem0 | log ชั่วคราว |

---

## 🗂️ ความสำคัญของ Critical Files และ Threat 1

### 📁 Customers’ Critical Files in domUs

- ลูกค้าเก็บไฟล์สำคัญไว้ใน `diskU` เช่น:
  - source code, database, binary, การทดลอง
- การเข้าถึงหรือรั่วไหลของข้อมูลถือเป็นภัยคุกคามร้ายแรง

![Figure 3 - Critical File in diskU](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/Img-Logging/figure3.png)

---

### 🚨 Threat 1: Abuse of Cloud to Attack

#### A. ผลกระทบต่อผู้ใช้

| การโจมตี | รายละเอียด |
|----------|-------------|
| domU → domU | VM ที่เป็นอันตรายแอบสแกนหรือโจมตี VM อื่น |
| domU → dom0 | ใช้ช่องโหว่ hypervisor เพื่อเจาะเข้า dom0 |
| dom0 → domU | ผู้ให้บริการหรือ attacker แอบอ่าน memory / log ใน domU |

#### B. ผลกระทบต่อผู้ให้บริการ

| การโจมตี | รายละเอียด |
|----------|-------------|
| domU → dom0 | หาก domU เจาะ hypervisor ได้ จะสามารถควบคุม dom0 |
| ข้าม host | ข้าม physical boundary ระหว่าง host ได้ |

---

## 📈 เทคนิค Logging ขั้นสูงเพื่อจัดการ Threat 1

### 📁 A. History of Critical Files (File-centric Log)

แนวคิดคือบันทึก "ประวัติของไฟล์สำคัญแต่ละไฟล์" โดยละเอียด ตั้งแต่:
- ใครเป็นผู้สร้าง (`p_id`)
- ใครเข้าใช้งาน (`p_nm`)
- เวลา (`created`, `read`, `write`, `delete`)

📊 **Table 1: ตัวอย่าง Log ประวัติไฟล์ `.s.txt`**

| ชื่อไฟล์ | p_id | p_nm  | p_ownid |
|----------|------|-------|---------|
| s.txt    | 6424 | cat   | 1002 (Bob) |

---

### ⚙️ B. Process Behaviour Log

บันทึกพฤติกรรมของ process เช่น ใช้ command อะไร, เข้าถึงไฟล์ไหน, มีพารามิเตอร์อะไร

📊 **Table 2: ตัวอย่าง Log พฤติกรรม Process**

| p_nm | cat | file | addr.txt | p_ownid |
|------|-----|------|----------|---------|
| cat  | 2400| addr.txt | 1002 (Bob) |

---

## ✉️ Case Study: จับคำสั่ง Spam ด้วย Behaviour Log

```bash
mail -s "spam a@b.c" addr.txt
```

- ส่งเมล spam ไปยัง address ใน `addr.txt`
- ใช้ log ของ `cat`, `mail`, และ `addr.txt` เป็นหลักฐานประกอบ

---

## 🏗️ System Architecture ของ Logging Solution

ใช้ libVMI ดึงข้อมูลจาก memU ของ domU → บันทึก log ลง disk0

![Figure 5 - System Architecture](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/Img-Logging/figure5.png)

---

## 🔒 การวิเคราะห์ความปลอดภัย

- domU **ไม่สามารถแก้ไข** logger ที่อยู่ใน dom0 ได้
- ใช้ `libVMI` ช่วยลด TCB (Trusted Computing Base)
- ข้อมูล log ถูกเก็บใน `disk0` ซึ่งทนต่อการลบหรือแก้ไขโดยลูกค้า

---

## ✅ สรุป

Template นี้สามารถใช้พัฒนา Logging System ที่:
- ลด Trusted Computing Base
- สนับสนุนทั้ง File-centric และ Process-centric log
- ตรวจจับพฤติกรรมผิดปกติ เช่น spam, data exfiltration
- สร้างความเชื่อมั่นให้กับทั้งผู้ให้บริการและผู้ใช้งาน

---

---

## 📥 การอ่านไฟล์จริงจาก memU ด้วย libVMI

การทำงานของ `logger` (รันใน dom0) จะใช้ `libVMI` อ่าน memory ของ domU เพื่อ:

- ตรวจสอบว่า process ใดกำลังอ่านไฟล์อะไร
- ดึงชื่อไฟล์ (string), ชื่อ process, PID, UID ฯลฯ

🖼️ **Figure 6:** ตัวอย่าง logger รันใน dom0 แล้วได้ผลลัพธ์:

```bash
$ ./logger
s.txt, by_proc_id: 4624, by_proc_name: read, by_proc_owner_id: 1002
```

🖼️ **Figure 7:** แสดง read command เริ่มอ่าน s.txt

```bash
$ ./read
...start reading... s.txt
```

---

## 🧠 วิธีดึงชื่อไฟล์จาก kernel structure (VMI)

libVMI จะตาม pointer chain ใน kernel ดังนี้:

1. `task_struct` → `files`
2. `files` → `fdt` → `fd` → `file`
3. `file` → `f_path` → `dentry` → `d_name` → `*name`

🖼️ **Figure 8 – 9:** โครงสร้าง kernel ที่ใช้ดึงชื่อไฟล์ `s.txt`

- Figure 8: ชี้ pointer chain ของ file descriptor
- Figure 9: โครงสร้าง dentry → string `s.txt`

---

## 🧪 การตีความผลลัพธ์ และตรวจจับความผิดปกติ

ในกรณี Alice เป็นผู้ใช้ระบบ และมีไฟล์สำคัญ `s.txt`  
→ เราสามารถวิเคราะห์จากตาราง log ได้ว่าใครเคยเข้าถึงไฟล์นี้บ้าง

📊 **Table III: ตัวอย่างผลลัพธ์ของประวัติไฟล์ `s.txt`**

| Time  | p_nm   | p_id | file  | p_ownid | Note |
|-------|--------|------|-------|---------|------|
| t_01  | read   | 4624 | s.txt | 1002    | ✔️   |
| t_02  | read   | 4624 | s.txt | 1002    | ✔️   |
| t_03  | cat    | 7000 | s.txt | 1002    | ❌   |
| t_04  | read   | 4624 | s.txt | 1002    | ✔️   |
| t_05  | mail   | 9999 | s.txt | 1002    | ❌   |

🔍 จุดผิดปกติ:
- `t_03`: ใช้ `cat` แทน `read` → พฤติกรรมเปลี่ยน
- `t_05`: ใช้ `mail` อ่าน `s.txt` → อาจส่ง spam

📌 สังเกตว่าข้อมูลเหล่านี้นำไปสู่:
- การจับ **attack behavior**
- การสร้าง **ความรับผิดชอบ (accountability)**
- การแจ้งเตือนเหตุการณ์ผิดปกติให้ผู้ใช้หรือ auditor

---


