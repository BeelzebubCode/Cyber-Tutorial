# บทที่ 6: แนวทางการทำการประมวลผลแบบกลุ่มเมฆไปใช้งานเบื้องต้น

## **สารบัญ**
- [6.1 Microsoft Azure](#61-microsoft-azure)
  - [6.1.1 Microsoft Azure คืออะไร](#611-microsoft-azure-คืออะไร)
  - [6.1.2 ประเด็นเรื่องความปลอดภัยของ Microsoft Azure](#612-ประเด็นเรื่องความปลอดภัยของ-microsoft-azure)
- [6.2 Mobile Cloud Computing](#62-mobile-cloud-computing)
- [6.3 การประยุกต์การประมวลผลแบบกลุ่มเมฆที่ให้บริการแบบ PaaS](#63-การประยุกต์การประมวลผลแบบกลุ่มเมฆที่ให้บริการแบบ-paas)
- [6.4 การประมวลผลแบบกลุ่มเมฆกับ Internet of Things (IoT)](#64-การประมวลผลแบบกลุ่มเมฆกับ-internet-of-things-iot)
- [6.5 การสร้างการประมวลผลแบบกลุ่มเมฆ](#65-การสร้างการประมวลผลแบบกลุ่มเมฆ)
- [6.6 การประมวลผลแบบกลุ่มเมฆและการเรียนรู้ของเครื่อง (Machine Learning)](#66-การประมวลผลแบบกลุ่มเมฆและการเรียนรู้ของเครื่อง-machine-learning)
- [6.7 คำถามท้ายบท](#67-คำถามท้ายบท)

---

# **6.1 Microsoft Azure**
## **6.1.1 Microsoft Azure คืออะไร**

### 🔍 **Microsoft Azure คืออะไร**
Microsoft Azure เป็น **แพลตฟอร์มประมวลผลแบบกลุ่มเมฆ (Cloud Platform)** จาก Microsoft ที่ให้บริการครบวงจร:
- **IaaS (Infrastructure as a Service)** → สร้าง Virtual Machines (VM), Storage, Network
- **PaaS (Platform as a Service)** → พัฒนาและโฮสต์แอปพลิเคชัน โดยไม่ต้องดูแลโครงสร้างพื้นฐาน
- **SaaS (Software as a Service)** → ใช้งานแอปสำเร็จรูป เช่น Microsoft 365

---

### ✅ **จุดเด่นของ Azure**
- **Pay-as-you-go** → คิดค่าบริการตามการใช้งานจริง
- **ระบบ Subscription** → สมัครแล้วเลือกใช้บริการที่ต้องการ
- **ยืดหยุ่น (Flexible)** → เพิ่ม/ลดขนาด VM หรือสร้าง VM ใหม่ได้ทันที
- **เข้าถึงได้ทุกที่ผ่านอินเทอร์เน็ต**
- **รองรับหลาย OS** → เช่น **Windows Server**, **Ubuntu**, **Linux**

---

### ✅ **โครงสร้างและเทคโนโลยี**
- ใช้เทคโนโลยี **Virtualization** ผ่าน **Azure Hypervisor**
- เครื่องมือสำหรับควบคุม:
  - **Azure Portal** (เว็บเบราว์เซอร์)
  - **Azure CLI**
  - **API / SDK** สำหรับนักพัฒนา
- มี **ระบบสำรองข้อมูลอัตโนมัติ** และ **Access Control** เพิ่มความปลอดภัย

---

### ✅ **บริการหลักของ Microsoft Azure**
1. **Compute:**  
   - สร้างและรัน **VM** สำหรับงานทั่วไป
   - รัน **Application Server** หรือ **Media Streaming**
2. **Storage:**  
   - จัดเก็บไฟล์ใน **Blob Storage** ที่ปลอดภัย
3. **Database:**  
   - ใช้ **Azure SQL Database** หรือ **Data Warehouse**
4. **AI/ML Services:**  
   - ทำ **Big Data Analytics** และสร้างโมเดล Machine Learning

---

### ✅ **ตัวอย่างการใช้งานจริง**
- นักวิจัยใช้ **VM บน Azure** เพื่อรันงานคำนวณหนัก
- นักพัฒนา Deploy แอปด้วย **Azure App Service** (PaaS)
- องค์กรใช้ **Azure Storage + SQL Database** เพื่อจัดการ Big Data

---

### 🔑 **สรุปสั้นๆ**
**Azure = Cloud Platform แบบครบวงจร**  
เหมาะสำหรับ:
- **องค์กร** → ลดต้นทุน Hardware
- **นักพัฒนา** → ใช้ PaaS ทำแอป
- **งาน AI / Big Data** → ต้องการประสิทธิภาพสูงและปลอดภัย

---
