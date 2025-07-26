# บทที่ 3: ประเภทของ Cloud Computing ตามลักษณะบริการ (Service Models)

---

## **สารบัญ**
- [3.1 โครงสร้างพื้นฐานของ Cloud](#31-โครงสร้างพื้นฐานของ-cloud)
- [3.2 สภาพแวดล้อมพื้นฐานของ Cloud](#32-สภาพแวดล้อมพื้นฐานของ-cloud)
- [3.3 ประเภทของการให้บริการ (Service Models)](#33-ประเภทของการให้บริการ-service-models)
  - [🏠 การเปรียบเทียบ SaaS, PaaS, IaaS กับการสร้างบ้าน](#-การเปรียบเทียบ-saas-paas-iaas-กับการสร้างบ้าน)
  - [3.3.1 IaaS Architecture](#331-iaas-architecture)
  - [3.3.2 PaaS (Platform as a Service)](#332-paas-platform-as-a-service)
  - [3.3.3 SaaS (Software as a Service)](#333-saas-software-as-a-service)
  - [3.3.4 ความสัมพันธ์ของ IaaS, PaaS และ SaaS](#334-ความสัมพันธ์ของ-iaas-paas-และ-saas)
- [3.4 การประมวลผลแบบกลุ่มเมฆประเภทอื่น](#34-การประมวลผลแบบกลุ่มเมฆประเภทอื่น)
- [3.5 สรุป](#35-สรุป)
- [3.6 คำถามท้ายบท (พร้อมเฉลย ✅)](#36-คำถามท้ายบท-พร้อมเฉลย-)
  
---

## **3.1 โครงสร้างพื้นฐานของ Cloud**
Cloud Computing ประกอบด้วย 2 ชั้นหลัก:

- **ชั้นกายภาพ (Physical Layer)**  
  ➤ ฮาร์ดแวร์จริง เช่น Storage, Server, Network
- **ชั้นนามธรรม (Abstraction Layer)**  
  ➤ ซอฟต์แวร์ที่ซ่อนรายละเอียดฮาร์ดแวร์ ทำงานผ่าน Virtualization

![โครงสร้างพื้นฐานของ Cloud](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/figure3.1.png)  
*รูปที่ 3.1 โครงสร้างพื้นฐานของ Cloud Computing*

---

## **3.2 สภาพแวดล้อมพื้นฐานของ Cloud**
- **Provider Side** → Virtual Machine Management, Cloud Services  
- **Consumer Side** → เข้าถึงผ่านเครือข่ายเพื่อใช้งานบริการ  
- **เชื่อมโยงผ่าน Network** ทำให้ผู้ใช้สร้าง VM และเรียกใช้บริการได้

![สภาพแวดล้อมพื้นฐานของ Cloud](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/figure3.2.png)  
*รูปที่ 3.2 สภาพแวดล้อมพื้นฐานของ Cloud Computing*

---

## **3.3 ประเภทของการให้บริการ (Service Models)**

| ประเภท | ความหมาย | ตัวอย่าง |
|--------|-----------|----------|
| **SaaS** | ซอฟต์แวร์พร้อมใช้งานผ่านอินเทอร์เน็ต | Gmail, Google Docs, Microsoft 365 |
| **PaaS** | แพลตฟอร์มสำหรับนักพัฒนา (Deploy ได้โดยไม่ต้องดูแลโครงสร้างพื้นฐาน) | Google App Engine, Heroku |
| **IaaS** | โครงสร้างพื้นฐานแบบ Virtual เช่น VM, Storage, Network | AWS EC2, Google Compute Engine |

**สรุปสั้น:**  
✔ **SaaS** → ผู้ใช้ใช้งานซอฟต์แวร์  
✔ **PaaS** → นักพัฒนาพัฒนาและ Deploy แอป  
✔ **IaaS** → ให้ทรัพยากร Virtual สำหรับควบคุมเอง  

---

### ✅ ความสัมพันธ์ระหว่าง Service Models
![ความสัมพันธ์ SaaS, PaaS, IaaS](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/figure3.3.png)  
*รูปที่ 3.3 ความสัมพันธ์ของ Service Models*

---

## **🏠 การเปรียบเทียบ SaaS, PaaS, IaaS กับการสร้างบ้าน**

### **IaaS = พื้นดิน + สาธารณูปโภค**
- ผู้ให้บริการดูแล **Hardware และ Virtualization**
- ลูกค้าสร้าง OS และแอปเอง  
*(เหมือนคุณได้ที่ดินพร้อมสาธารณูปโภค)*  
**ตัวอย่าง:** AWS EC2, Google Compute Engine

### **PaaS = บ้านที่สร้างเสร็จ (ตกแต่งเอง)**
- Provider ให้ **Runtime + Database + เครื่องมือ Dev**  
- นักพัฒนา Deploy แอปได้ทันที  
**ตัวอย่าง:** Google App Engine, Heroku

### **SaaS = บ้านพร้อมอยู่**
- Provider จัดการทุกอย่าง → ผู้ใช้ใช้งานทันที  
**ตัวอย่าง:** Gmail, Google Docs

---

### ✅ ตารางสรุป House Analogy

| ประเภท | เปรียบเทียบ | Provider ดูแล | ผู้ใช้ทำ | ตัวอย่าง |
|--------|-------------|---------------|----------|-----------|
| **IaaS** | พื้นดิน 🌍 | Hardware + Virtualization | ติดตั้ง OS, แอป | AWS EC2 |
| **PaaS** | ตัวบ้าน 🏠 | Infrastructure + Runtime | เขียนโค้ด, Deploy | Heroku |
| **SaaS** | บ้านพร้อมอยู่ 🛋 | ทุกอย่าง (Infra → App) | ใช้งาน | Gmail |

---

## **3.3.1 IaaS Architecture**

![สถาปัตยกรรม IaaS](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/figure3.4.png)  
*รูปที่ 3.4 สถาปัตยกรรม IaaS*  

**องค์ประกอบ:**
- **Dom0** → จัดการ VM (Virtual Machine Management)  
- **DomU** → Virtual Machine ของผู้ใช้  
- **Hypervisor** → ตัวกลางระหว่าง Hardware และ VM  
- **Hardware** → Physical Server, CPU, Storage  
- **Consumer** → ควบคุม OS และแอปเอง  

**จุดเด่น:**  
✔ ควบคุมได้มากที่สุด  
✔ Scale ได้ง่าย  
✔ ลดต้นทุน Hardware

---

## **3.3.2 PaaS (Platform as a Service)**

![สถาปัตยกรรม PaaS](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/figure3.6.png)  
*รูปที่ 3.5 สถาปัตยกรรมของ PaaS*

สถาปัตยกรรม PaaS ประกอบด้วย 3 ส่วนหลัก:
- **Provider Side** → จัดการโครงสร้างพื้นฐาน เช่น Hardware, Hypervisor, Virtual Machine Management
- **Developer Side** → สร้างและ Deploy แอปพลิเคชัน
- **User Side** → ใช้งานแอปที่ถูก Deploy แล้ว

**คำอธิบายภาพ:**
- **Provider Side**  
  - **hw** → Hardware จริง (CPU, RAM, Storage)  
  - **Hypervisor** → จัดสรรทรัพยากร Virtual Machine  
  - **dom0** → Virtual Machine Management  
  - **domU** → VM ที่มี Web Server และโค้ดแอปพลิเคชัน  
- **Developer Side**  
  - Developer อัปโหลดโค้ดผ่าน Network มายัง PaaS  
- **User Side**  
  - User ใช้งานแอปที่ Developer สร้างขึ้น ผ่าน Web/Mobile Interface

---

### ✅ ความสัมพันธ์ของ Developer และ User
- **Developer** → ใช้ PaaS เพื่อ Deploy แอป
- **User** → ใช้แอปที่รันบน PaaS ผ่านอินเทอร์เน็ต (Web/Mobile)
- 
---

## **3.3.3 SaaS (Software as a Service)**

- ให้บริการซอฟต์แวร์สำเร็จรูปผ่าน Cloud  
- ผู้ใช้ไม่ต้องดูแลระบบใด ๆ  
- ตัวอย่าง: Gmail, Google Docs, Microsoft Office 365  

**จุดเด่น:**  
✔ ไม่มีการติดตั้ง  
✔ ใช้จากทุกอุปกรณ์ที่มีอินเทอร์เน็ต  
✔ จ่ายตามการใช้งานจริง

---

## **3.3.4 ความสัมพันธ์ของ IaaS, PaaS และ SaaS**

![ความสัมพันธ์ของ IaaS, PaaS และ SaaS](https://github.com/BeelzebubCode/Cyber-Tutorial/blob/main/Cloud%20Computing/Image/figure3.5.png)  
*รูปที่ 3.5 ความสัมพันธ์ของ IaaS, PaaS และ SaaS*

---

### ✅ แนวคิดภาพรวม
สถาปัตยกรรม Cloud ถูกออกแบบเป็น **ชั้น (Layer)** เพื่อแยกความรับผิดชอบของแต่ละบริการดังนี้:
- **ชั้นล่างสุด (Infrastructure Layer)** → IaaS เป็นรากฐานของระบบ
- **ชั้นกลาง (Platform Layer)** → PaaS ให้เครื่องมือและสภาพแวดล้อมสำหรับนักพัฒนา
- **ชั้นบนสุด (Application Layer)** → SaaS ให้แอปพลิเคชันสำเร็จรูปสำหรับผู้ใช้

---

### ✅ รายละเอียดแต่ละชั้น

| ชั้นบริการ | องค์ประกอบ | หน้าที่ | ตัวอย่าง |
|-----------|-----------|--------|---------|
| **Infrastructure Layer (IaaS)** | Facilities, Hardware, Virtualization, Core Connectivity & Delivery | ให้ทรัพยากร Virtual เช่น VM, Storage | AWS EC2, Google Compute Engine |
| **Platform Layer (PaaS)** | Middleware, APIs, Data Management | ให้เครื่องมือพัฒนาและ Deploy แอป | Google App Engine, Microsoft Azure |
| **Application Layer (SaaS)** | Applications, Presentation UI | ให้แอปสำเร็จรูปพร้อมใช้งาน | Gmail, Google Docs |

---

### ✅ ความสัมพันธ์ระหว่าง Layer
- **IaaS** → ฐานรากของ Cloud ที่ให้ฮาร์ดแวร์และ Virtualization  
- **PaaS** → ใช้ IaaS เป็นพื้นฐาน เพิ่ม Middleware, APIs และบริการเสริม  
- **SaaS** → ใช้ PaaS และ IaaS เพื่อให้บริการแอปพร้อมใช้งานสำหรับผู้ใช้  

**สรุป:**  
✔ หากไม่มี **IaaS** → จะไม่มี PaaS และ SaaS  
✔ หากไม่มี **PaaS** → การสร้าง SaaS จะซับซ้อนขึ้นมาก  
✔ SaaS พึ่ง PaaS และ IaaS ในการทำงานอย่างเต็มรูปแบบ

---

## **3.4 การประมวลผลแบบกลุ่มเมฆประเภทอื่น**

| Layer | Service | Description | Example |
|-------|---------|------------|---------|
| Cloud Applications | SaaS | Desktop and business apps | Gmail |
| Cloud Services | CaaS | SOA ผ่าน Web Service | Amazon Flexible Payments |
| Cloud Platform | PaaS | Hosting enterprise apps | Google App Engine |
| Cloud Storage | Storage as a Service | Storage และ Database | AWS S3 |
| Cloud Infrastructure | IaaS | Virtualization | AWS EC2 |

---

## **3.5 สรุป**
✔ SaaS = ผู้ใช้ทั่วไป  
✔ PaaS = นักพัฒนา  
✔ IaaS = องค์กรที่ต้องการควบคุมสูงสุด  

---

## **3.6 คำถามท้ายบท (พร้อมเฉลย ✅)**

### **1. IaaS คืออะไร? ยกตัวอย่าง**
✅ บริการที่ให้โครงสร้างพื้นฐาน Virtual เช่น **AWS EC2**, **Google Compute Engine**

---

### **2. PaaS คืออะไร? ทำไมเรียกว่าแพลตฟอร์มสำหรับนักพัฒนา**
✅ เพราะนักพัฒนาสามารถ Deploy แอปได้โดยไม่ต้องจัดการ Hardware

---

### **3. ยกตัวอย่าง SaaS ที่ใช้อยู่ในชีวิตประจำวัน**
✅ **Gmail, Facebook, Google Docs**

---

### **4. ความแตกต่างหลักระหว่าง SaaS, PaaS, IaaS คืออะไร**
✅  
- **SaaS** → ใช้งานซอฟต์แวร์สำเร็จรูป  
- **PaaS** → สำหรับพัฒนาและ Deploy แอป  
- **IaaS** → ให้ทรัพยากร Virtual Hardware สำหรับติดตั้ง OS และระบบเอง  

---

### **5. Hypervisor ทำหน้าที่อะไร**
✅ จัดการ Virtual Machine บน Hardware จริง

---

### **6. ทำไม Gmail ถึงจัดว่าเป็น SaaS โดยอ้างอิงกับลักษณะสำคัญของ Cloud (หัวข้อ 2.2)**

✅ **เหตุผล:**
- Gmail คือ **Software as a Service (SaaS)** เพราะ:
  - ผู้ใช้เข้าถึงผ่าน **Web Interface หรือ Mobile App** โดยไม่ต้องติดตั้งซอฟต์แวร์
  - ตรงตาม **ลักษณะสำคัญของ Cloud (NIST 2.2)**:
    1. **On-Demand Self-Service** → สมัครและใช้งานได้ทันที
    2. **Broad Network Access** → ใช้งานได้บนอุปกรณ์หลากหลายผ่านอินเทอร์เน็ต
    3. **Resource Pooling** → ใช้ทรัพยากรร่วมกันแบบ Multi-Tenant
    4. **Rapid Elasticity** → ขยายพื้นที่เก็บอีเมลได้ตามต้องการ
    5. **Measured Service** → จ่ายตามการใช้งานจริง (ฟรีหรือแบบจ่ายเพิ่ม)

**สรุป:** Gmail เป็นซอฟต์แวร์สำเร็จรูปที่รันบน Cloud ผู้ใช้ไม่ต้องจัดการระบบเอง

---

### **7. ยกตัวอย่างบริการ SaaS เพิ่มเติมอย่างน้อย 2 แหล่ง และสรุปเหตุผล**
✅ ตัวอย่าง:
- **Google Docs**  
  → ให้บริการเอกสารออนไลน์ ทำงานร่วมกันได้แบบ Real-Time โดยไม่ต้องติดตั้งโปรแกรม
- **Microsoft 365**  
  → ชุด Office ผ่าน Cloud ใช้งานได้หลายอุปกรณ์
- **Salesforce**  
  → บริการ CRM ออนไลน์ ผู้ใช้เพียงล็อกอินใช้งาน ไม่ต้องดูแลโครงสร้างพื้นฐาน

**สรุป:** ทั้งหมดนี้เป็น SaaS เพราะผู้ใช้เข้าถึงผ่านอินเทอร์เน็ตโดยไม่ต้องติดตั้งหรือดูแลเซิร์ฟเวอร์เอง

