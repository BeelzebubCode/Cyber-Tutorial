# 🔍 Google Search Dork Cheat Sheet for Cyber Security

## 📌 ความสำคัญของทักษะการค้นหา

- อินเทอร์เน็ตเต็มไปด้วยข้อมูล — ใครค้นได้แม่น = ได้เปรียบ
- Hacker, Pentester, Blue Team, Analyst ทุกคนต้องใช้
- เป็นเครื่องมือ OSINT ขั้นพื้นฐานที่สำคัญมาก

---

## 🧠 Google Dork Operators ที่ควรรู้

| Operator | ความหมาย | ตัวอย่าง | อธิบาย |
|----------|-----------|----------|--------|
| `"..."` | ค้นหาคำหรือวลีเป๊ะ ๆ | `"confidential document"` | ค้นหาประโยคตรงตัว ไม่ตัดคำ |
| `site:` | จำกัดการค้นหาในเว็บไซต์ | `site:github.com payload.py` | ค้นเฉพาะในเว็บที่กำหนด |
| `-` | ตัดคำที่ไม่ต้องการออก | `linux tutorial -ubuntu` | เอาผลลัพธ์ที่ไม่มีคำที่ไม่ต้องการ |
| `filetype:` | ค้นหาเฉพาะไฟล์ | `filetype:pdf exploit development` | หาไฟล์ pdf, doc, xls, ppt ฯลฯ |
| `intitle:` | คำในหัวข้อเว็บ | `intitle:"index of /admin"` | ค้นหาชื่อเว็บที่ขึ้นด้วยข้อความนี้ |
| `inurl:` | คำใน URL | `inurl:login site:gov.uk` | หา URL ที่มีคำว่า login |
| `cache:` | เรียกหน้าเว็บจากแคช | `cache:tryhackme.com` | ดูหน้าเว็บล่าสุดที่ Google เก็บไว้ |
| `related:` | หาเว็บคล้ายกัน | `related:cnn.com` | หาข่าวหรือเว็บคล้าย CNN |

---

## 🧨 ตัวอย่างการใช้งานจริง

### 🔐 หาหน้า admin
```
inurl:admin login
intitle:"admin panel"
site:example.com inurl:admin
```

### 📁 หาไฟล์หลุดหรือ config สำคัญ
```
intitle:"index of" ".env"
intitle:"index of" ".git"
filetype:log password
filetype:env DB_PASSWORD
```

### 📥 หาไฟล์ความรู้/เอกสารลับ
```
filetype:pdf site:nist.gov cybersecurity
filetype:xls intext:@example.com
filetype:ppt "ethical hacking"
```

### 🌍 หา email/ชื่อพนักงานบริษัท
```
site:linkedin.com "@company.com"
intext:"email" AND "@target.com"
```

---

## 💡 เคล็ดลับ

- ผสม Dork หลายตัวเพื่อความแม่นยำ เช่น:
```
site:example.com filetype:pdf "internal use only"
```

- ใช้เครื่องหมาย `OR` ได้ เช่น:
```
"admin login" OR "administrator panel"
```

---

## 🛡️ ข้อควรระวัง

> ใช้เพื่อ **การศึกษา / การตรวจสอบระบบที่คุณมีสิทธิ์เข้าถึงเท่านั้น**  
> การใช้ Dork หาไฟล์ที่ไม่ได้รับอนุญาตเข้าถึง = ผิดกฎหมายในหลายประเทศ

---

## 📚 แหล่งเรียนรู้เพิ่มเติม

- 🔗 https://www.exploit-db.com/google-hacking-database
- 🧠 TryHackMe: Room `Search Skills`
- 🛠 OSINT Framework: https://osintframework.com/

---

> ✨ ทักษะการค้นหา คือดาบของนัก Cyber  
> ใครฟันไว-แม่น = ได้เปรียบในสนามรบไซเบอร์เสมอ ⚔️
