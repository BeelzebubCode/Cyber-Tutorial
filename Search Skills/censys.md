# 🌐 Censys - เครื่องมือค้นหาโฮสต์และใบรับรอง SSL ทั่วโลก

> Censys คือเครื่องมือค้นหาและวิเคราะห์สินทรัพย์อินเทอร์เน็ต เช่น โฮสต์ (IP), โดเมน, พอร์ต, และใบรับรอง SSL ที่เปิดเผยต่อสาธารณะ โดยใช้ข้อมูลจากการสแกนอินเทอร์เน็ตทั้ง IPv4 และ Certificate Transparency Logs

---

## 🧠 เหมาะกับใคร
- Blue Team / SOC → ตรวจสอบว่าทรัพยากรองค์กรหลุดไปยัง public หรือไม่
- Pentester / Bug Bounty → ค้นหาโดเมนย่อย, บริการ, หรือ cert ที่องค์กรเปิดไว้โดยไม่รู้ตัว
- นักวิจัยด้านความปลอดภัย → ทำ Asset Inventory & SSL analysis

---

## 📥 วิธีใช้งานเบื้องต้น
- สมัครที่ [Censys.io](https://search.censys.io) (บัญชีฟรีมี API และ Query Limit)
- ใช้ช่องค้นหาหลัก หรือ [Advanced Search](https://search.censys.io/search) ที่ใช้ภาษา query แบบ SQL-like

---

## 🔍 ตัวอย่าง Query

| คำค้น | ความหมาย |
|--------|-----------|
| `services.service_name: "http"` | โฮสต์ที่รัน HTTP Service อยู่ |
| `services.banner: "Apache 2.4.1"` | เครื่องที่เปิด Apache เวอร์ชัน 2.4.1 |
| `location.country: "Thailand"` | อุปกรณ์ที่อยู่ในประเทศไทย |
| `services.port: 445` | โฮสต์ที่เปิด SMB port 445 |
| `services.software.uniform_resource_identifier: "nginx"` | บริการที่ใช้ nginx |

---

## 🔐 ตรวจสอบ SSL Certificate
```text
services.tls.certificates.leaf_data.subject.common_name: "example.com"
services.tls.certificates.validation.nss.valid: true
```
- ตรวจหาโดเมนที่ออกใบ cert ผิด หรือหมดอายุ
- ค้นหา subdomain ที่ฝังอยู่ใน cert

---

## 🔧 API และ CLI
- มี API ใช้งานผ่าน Python ได้ (ใช้สำหรับ automation หรือ threat hunting)
- CLI: `pip install censys`
```bash
censys search "services.port: 21 AND services.banner: 'vsftpd'"
censys config <API_ID> <SECRET>
```

---

## 💡 เทคนิคเพิ่มเติม
- ใช้ร่วมกับ recon tool เช่น Amass, Subfinder, crt.sh เพื่อ enrich data
- ใช้ Censys Certificate search เพื่อดูว่ามี domain ไหนออกใบ cert โดย CA แปลก ๆ

---

## ⚠️ คำเตือน
> ข้อมูลใน Censys เป็นข้อมูล public แต่การนำไปใช้ในทางที่ผิด เช่น scanning หรือ exploitation โดยไม่ได้รับอนุญาต ถือว่าผิดจริยธรรม

<sub>ที่มา: Censys.io และเอกสาร API อย่างเป็นทางการ</sub>
