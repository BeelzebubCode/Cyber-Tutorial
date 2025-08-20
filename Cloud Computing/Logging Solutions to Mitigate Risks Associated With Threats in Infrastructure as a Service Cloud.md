# 🧱 Logging Solutions to Mitigate Risks Associated With Threats in Infrastructure as a Service Cloud

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
- [📥 การอ่านไฟล์จริงจาก memU ด้วย libVMI](#-การอ่านไฟล์จริงจาก-memu-ด้วย-libvmi)
- [🧠 วิธีดึงชื่อไฟล์จาก kernel structure (VMI)](#-วิธีดึงชื่อไฟล์จาก-kernel-structure-vmi)
- [🧪 การตีความผลลัพธ์ และตรวจจับความผิดปกติ](#-การตีความผลลัพธ์-และตรวจจับความผิดปกติ)
- [🧪 วิเคราะห์เหตุการณ์ผิดปกติจากผลลัพธ์ที่ได้ (ต่อ)](#-วิเคราะห์เหตุการณ์ผิดปกติจากผลลัพธ์ที่ได้-ต่อ)
- [🤝 การช่วยลดความเสี่ยง Threat 1 ของผู้ให้บริการ](#-การช่วยลดความเสี่ยง-threat-1-ของผู้ให้บริการ)
- [📚 เปรียบเทียบกับงานที่เกี่ยวข้อง](#-เปรียบเทียบกับงานที่เกี่ยวข้อง)
- [🔐 ความเป็นส่วนตัว และความลับ](#-ความเป็นส่วนตัว-และความลับ)
- [🔐 ความเป็นส่วนตัวของข้อมูล Log](#-ความเป็นส่วนตัวของข้อมูล-log)
- [🧾 บทสรุป (Conclusions)](#-บทสรุป-conclusions)

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

![Figure 1 - IaaS Architecture](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/Img-Logging/figure6.png)

- `dom0` เป็นระบบที่มีสิทธิ์สูง ควบคุมการสร้าง/ลบ domU  
- `domU` เป็น VM ที่ลูกค้าเช่าใช้งาน  
- `hypervisor` เป็นซอฟต์แวร์จัดการ VM  
- `hw` คือ physical server จริง

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🧩 องค์ประกอบของ Template (Figure 2)

แสดงตำแหน่งของ Logging Process (Px) และ Log File (Fy) บนโครงสร้าง IaaS  
ใช้เพื่อสร้างระบบ Logging ใหม่ และวิเคราะห์ความเสี่ยงจากการจัดวาง

![Figure 2 - Logging Template](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/Img-Logging/figure7.png)

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

<sub>[⬆ Home](#-สารบัญ)</sub>

---

### ⚙️ Logging Processes (Px)

| Px | ตำแหน่ง | หน้าที่ |
|----|----------|----------|
| `P1` | dom0 user | อ่าน memU ผ่าน libVMI |
| `P2` | dom0 kernel | ดักจับ network/file ops |
| `P3` | domU kernel | ดักจับ system call ของ domU |
| `P4` | hypervisor | ดัก network packet ของ domU |

<sub>[⬆ Home](#-สารบัญ)</sub>

---

### 🗃️ Log Files (Fy)

| Fy | ตำแหน่ง | ลักษณะข้อมูล |
|----|----------|--------------|
| `F1` | diskU | log ชั่วคราว |
| `F2` | memU | log ชั่วคราว |
| `F3` | disk0 | log ถาวร |
| `F4` | mem0 | log ชั่วคราว |

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🗂️ ความสำคัญของ Critical Files และ Threat 1

### 📁 Customers’ Critical Files in domUs

- ลูกค้าเก็บไฟล์สำคัญไว้ใน `diskU` เช่น:
  - source code, database, binary, การทดลอง
- การเข้าถึงหรือรั่วไหลของข้อมูลถือเป็นภัยคุกคามร้ายแรง

![Figure 3 - Critical File in diskU](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/Img-Logging/figure8.png)

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

<sub>[⬆ Home](#-สารบัญ)</sub>

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

<sub>[⬆ Home](#-สารบัญ)</sub>

---

### ⚙️ B. Process Behaviour Log

บันทึกพฤติกรรมของ process เช่น ใช้ command อะไร, เข้าถึงไฟล์ไหน, มีพารามิเตอร์อะไร

📊 **Table 2: ตัวอย่าง Log พฤติกรรม Process**

| p_nm | cat | file | addr.txt | p_ownid |
|------|-----|------|----------|---------|
| cat  | 2400| addr.txt | 1002 (Bob) |

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## ✉️ Case Study: จับคำสั่ง Spam ด้วย Behaviour Log

```bash
mail -s "spam a@b.c" addr.txt
```

- ส่งเมล spam ไปยัง address ใน `addr.txt`
- ใช้ log ของ `cat`, `mail`, และ `addr.txt` เป็นหลักฐานประกอบ

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🏗️ System Architecture ของ Logging Solution

ใช้ libVMI ดึงข้อมูลจาก memU ของ domU → บันทึก log ลง disk0

![Figure 5 - System Architecture](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/Img-Logging/figure12.png)

---

## 🔒 การวิเคราะห์ความปลอดภัย

- domU **ไม่สามารถแก้ไข** logger ที่อยู่ใน dom0 ได้
- ใช้ `libVMI` ช่วยลด TCB (Trusted Computing Base)
- ข้อมูล log ถูกเก็บใน `disk0` ซึ่งทนต่อการลบหรือแก้ไขโดยลูกค้า

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## ✅ สรุป

Template นี้สามารถใช้พัฒนา Logging System ที่:
- ลด Trusted Computing Base
- สนับสนุนทั้ง File-centric และ Process-centric log
- ตรวจจับพฤติกรรมผิดปกติ เช่น spam, data exfiltration
- สร้างความเชื่อมั่นให้กับทั้งผู้ให้บริการและผู้ใช้งาน

<sub>[⬆ Home](#-สารบัญ)</sub>

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

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🧠 วิธีดึงชื่อไฟล์จาก kernel structure (VMI)

libVMI จะตาม pointer chain ใน kernel ดังนี้:

1. `task_struct` → `files`
2. `files` → `fdt` → `fd` → `file`
3. `file` → `f_path` → `dentry` → `d_name` → `*name`

🖼️ **Figure 8 – 9:** โครงสร้าง kernel ที่ใช้ดึงชื่อไฟล์ `s.txt`

- Figure 8: ชี้ pointer chain ของ file descriptor
- Figure 9: โครงสร้าง dentry → string `s.txt`

<sub>[⬆ Home](#-สารบัญ)</sub>

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

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🧪 วิเคราะห์เหตุการณ์ผิดปกติจากผลลัพธ์ที่ได้ (ต่อ)

### 2) การตรวจจับเหตุการณ์เมื่อ domU มีหลายผู้ใช้

หาก domU ของ Alice มีทั้ง root และผู้ใช้ Bob  
→ attacker อาจขโมย credentials ของ Bob เพื่อใช้ `read.appU` เปิด `s.txt`

📊 จาก **Table III** หาก `p_ownid` เป็น 1003 (Bob)  
→ แม้ใช้ `read` แต่ไม่ใช่เจ้าของไฟล์จริง (1002 = Alice)  
→ จึงอาจเป็นการ **แอบอ่าน** โดยไม่ได้รับอนุญาต

---

### 3) การปรับ Logger ให้ตรวจสอบ process อื่น

เช่น auditor ต้องการรู้ว่า `gedit` เคยเปิด `s.txt` หรือไม่  
สามารถบันทึกว่า `gedit.appU` เคยเข้าถึงไฟล์อะไรบ้าง

📊 **Table IV: ตัวอย่าง log การเปิดไฟล์ของ gedit**

| p_nm  | file  | p_ownid |
|-------|-------|---------|
| gedit | s.txt | 1002    |
| gedit | addr.txt | 1002 |

ประโยชน์:
- ขยายความสามารถของ auditor
- รองรับการวิเคราะห์ตาม process แทนไฟล์

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🤝 การช่วยลดความเสี่ยง Threat 1 ของผู้ให้บริการ

### วิเคราะห์จาก Table V:

📊 **Table V: พฤติกรรมส่ง spam โดยใช้ mail และ cat**

| p_nm  | file      | command                                  |
|-------|-----------|------------------------------------------|
| mail  | addr.txt  | `mail -s subject <cat addr.txt`          |

→ ระบุว่า domU (p_ownid = 1002) ใช้ 2 คำสั่งร่วมกันส่ง spam  
→ ช่วยให้ provider ระบุและปิดการใช้งาน domU นี้ได้ทันท่วงที

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 📚 เปรียบเทียบกับงานที่เกี่ยวข้อง

| เครื่องมือ | จุดเด่น | จุดด้อย |
|-------------|---------|---------|
| **Flogger** | จับ history log แบบกระจาย (distributed) | TCB ใหญ่ |
| **PASSXen** | รองรับ provenance แบบกระจายทั่วระบบ | TCB ใหญ่ |
| **งานนี้**  | ใช้ libVMI เฉพาะใน dom0, ลด TCB | ไม่รองรับ distributed logging |

💡 จุดแข็งของงานนี้คือใช้ TCB ขนาดเล็กเฉพาะ dom0 เท่านั้น  
→ ลดโอกาสที่ attacker ใน domU จะควบคุมระบบ logger ได้

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🔐 ความเป็นส่วนตัว และความลับ

- log files อาจเปิดเผยพฤติกรรมผู้ใช้ เช่น process ที่ใช้, ไฟล์ที่เปิด
- ควรใช้ encryption หรือ anonymization ก่อนเก็บ log ถาวร
- ควรมีนโยบายการเข้าถึง log อย่างชัดเจนและตรวจสอบได้

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🔐 ความเป็นส่วนตัวของข้อมูล Log

Log files ที่เก็บทั้งชื่อ process, file, และเวลาการใช้งาน  
→ อาจเป็นข้อมูลอ่อนไหวหากตกไปอยู่ในมือผู้ไม่พึงประสงค์

> *"The history information may disclose customer business activities or malicious activities inside provider’s infrastructure."*

คำแนะนำ:
- ควรควบคุมระดับการเปิดเผย (ระดับต่ำ = เวลาอย่างเดียว, ระดับสูง = ชื่อ process)
- อนุญาตให้เฉพาะบุคคลที่ได้รับสิทธิ์ตรวจสอบเท่านั้น
- พิจารณาการใช้ anonymization หรือ encryption

<sub>[⬆ Home](#-สารบัญ)</sub>

---

## 🧾 บทสรุป (Conclusions)

> *“Cloud Security Alliance Threat 1 (criminal activities in VMs) affects both customers and providers”*

- งานวิจัยนี้เสนอโครงสร้างระบบ Logging ที่:
  - ลด Trusted Computing Base (TCB)
  - เพิ่มความน่าเชื่อถือด้วย File-centric และ Process-centric logs
  - รองรับการวิเคราะห์ภัยจาก spam, การเข้าถึงไฟล์ลับ และการรันคำสั่งต้องสงสัย

- ระบบ Logging สามารถใช้เพื่อตรวจสอบย้อนหลัง
  - ว่าเกิดเหตุใด
  - ใครเป็นผู้กระทำ
  - กระทำผ่าน process อะไร
  - เข้าถึงไฟล์อะไร

- ผลลัพธ์ช่วย:
  - **เสริมความโปร่งใส (accountability)** ของ IaaS
  - **ลดความเสี่ยงจากภัยคุกคาม CSA Threat 1**
  - **สร้างความมั่นใจแก่ลูกค้าและผู้ให้บริการ**

งานวิจัยนี้แสดงให้เห็นว่า **การวาง logging system ที่เหมาะสม**  
สามารถช่วยให้ cloud infrastructure มีความปลอดภัย และตรวจสอบได้

<sub>[⬆ Home](#-สารบัญ)</sub>

---



