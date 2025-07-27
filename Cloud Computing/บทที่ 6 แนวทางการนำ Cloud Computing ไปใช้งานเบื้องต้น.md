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

## **6.1.1 Microsoft Azure คืออะไร**

Microsoft Azure เป็นแพลตฟอร์ม **Cloud Computing** ของ Microsoft ที่ให้บริการหลากหลายรูปแบบตามความต้องการของผู้ใช้งาน โดยแบ่งออกเป็น:

---

### ✅ **ประเภทของบริการใน Microsoft Azure**

| ประเภทบริการ | ความหมาย | ตัวอย่าง |
|-------------|-----------|----------|
| **IaaS (Infrastructure as a Service)** | ให้บริการทรัพยากรคอมพิวเตอร์เสมือน เช่น VM, Storage, Network | Azure Virtual Machines, Azure Disk Storage |
| **PaaS (Platform as a Service)** | ให้บริการแพลตฟอร์มสำหรับนักพัฒนา โดยไม่ต้องดูแลโครงสร้างพื้นฐาน | Azure App Service, Azure Functions |
| **SaaS (Software as a Service)** | ให้บริการซอฟต์แวร์สำเร็จรูปบนคลาวด์ | Microsoft 365, Dynamics 365 |

---

### ✅ **จุดเด่นของ Microsoft Azure**
- รองรับการทำงาน **ข้ามแพลตฟอร์ม** (Windows, Linux, Ubuntu)
- ระบบคิดค่าใช้จ่าย **ตามการใช้งานจริง** (Pay-as-you-go)
- **Auto-Scaling** รองรับโหลดสูง
- มีเครื่องมือสำหรับนักพัฒนา เช่น **Azure SDK**, **Azure CLI**, **REST API**

---

### ✅ **ตัวอย่างบริการหลักของ Azure**

| หมวดบริการ | รายละเอียด |
|-----------|-----------|
| **Compute** | Azure Virtual Machines สำหรับรัน OS เช่น Windows Server, Ubuntu |
| **App Hosting** | Azure App Service สำหรับโฮสต์เว็บแอปและ API |
| **Storage** | Azure Blob Storage, Table Storage สำหรับจัดเก็บข้อมูล |
| **Database** | Azure SQL Database และ Data Warehouse |
| **AI/ML** | Azure Machine Learning สำหรับสร้างโมเดล AI |

---

### ✅ **วิธีการเข้าถึงและจัดการ Microsoft Azure**

| วิธีการ | รายละเอียด |
|--------|-----------|
| **Azure Portal** | ผ่านเว็บเบราว์เซอร์ |
| **Azure CLI** | คำสั่ง Command Line |
| **Azure SDK / API** | สำหรับนักพัฒนาเชื่อมระบบ |

---

### 🔑 **สรุป**
Microsoft Azure เป็น **Cloud Platform ครบวงจร** ที่เหมาะสำหรับ:
- **องค์กร** → ลดต้นทุนและย้ายระบบขึ้นคลาวด์
- **นักพัฒนา** → ใช้ PaaS สำหรับทำแอป
- **งาน AI และ Big Data** → ต้องการทรัพยากรคอมพิวเตอร์ประสิทธิภาพสูง

<sub>[⬆ กลับไปสารบัญ](#สารบัญ)</sub>

