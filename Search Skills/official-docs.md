# 📚 การใช้งานเอกสารอย่างเป็นทางการ (Official Documentation)

> หนึ่งในทักษะที่สำคัญที่สุดของผู้เรียนสายเทคนิค คือการค้นหา อ่าน และอ้างอิงจากเอกสารอย่างเป็นทางการ เพราะข้อมูลเหล่านี้มาจากแหล่งต้นฉบับที่ถูกต้องและอัปเดตเสมอ

---

## 📖 Linux Man Pages

Linux และระบบที่คล้าย Unix มีระบบเอกสารภายในที่เรียกว่า **man page** ใช้ดูคำอธิบาย คำสั่ง ตัวเลือก ตัวอย่างการใช้ ฯลฯ

```bash
man ip      # เปิดหน้าคู่มือคำสั่ง ip
man ls      # หน้าคู่มือของคำสั่ง ls
```

- กด `q` เพื่อออกจากหน้า man
- ค้นหาผ่านเว็บ: `man ip site:man7.org` หรือ `man curl` บน Google

🔗 <a href="https://man7.org/linux/man-pages/" target="_blank">https://man7.org/linux/man-pages/</a>

---

## 🪟 Microsoft Windows Documentation

Windows มีเอกสารอย่างเป็นทางการสำหรับคำสั่งต่าง ๆ เช่น `ipconfig`, `netstat`, `powershell` ฯลฯ บนเว็บ Microsoft Docs

🔍 ตัวอย่าง:
- `ipconfig site:learn.microsoft.com`
- `powershell Get-Process`

🔗 <a href="https://learn.microsoft.com/en-us/windows-server/" target="_blank">https://learn.microsoft.com</a>

---

## 🌐 เอกสารผลิตภัณฑ์ (Product Docs)

ซอฟต์แวร์หรือบริการที่ดีควรมีเอกสารประกอบอย่างเป็นทางการ เช่น:

| ชื่อ | ลิงก์ |
|------|-------|
| Snort | <a href="https://docs.snort.org/" target="_blank">docs.snort.org</a> |
| Apache HTTPD | <a href="https://httpd.apache.org/docs/" target="_blank">httpd.apache.org/docs</a> |
| PHP | <a href="https://www.php.net/manual/en/" target="_blank">php.net/manual</a> |
| Node.js | <a href="https://nodejs.org/en/docs" target="_blank">nodejs.org/en/docs</a> |

---

## 💡 เทคนิคการค้นหาเอกสาร

- ใช้ Google ด้วยคำว่า:
```bash
<คำสั่งหรือชื่อโปรแกรม> site:officialsite.com
```

- หรือใช้ `intitle:"manual"` หรือ `inurl:docs` เพื่อเจาะจงผลลัพธ์

---

## 🧠 สรุป

| จุดเด่น | เหตุผลที่ควรใช้ |
|---------|------------------|
| ✅ ความถูกต้อง | มาจากผู้พัฒนาหรือองค์กรต้นฉบับ |
| 🆕 ความทันสมัย | อัปเดตตามเวอร์ชันล่าสุด |
| 📌 ความละเอียด | มี syntax, parameters, flags, และตัวอย่างครบ |

---

> ✨ ยิ่งคุณอ่านเอกสารเป็นเร็วเท่าไร ยิ่งเป็นอิสระมากขึ้นจากการพึ่งพาคำถามคนอื่น ⚡

<sub>สร้างเพื่อส่งเสริมทักษะการค้นหาและเข้าใจเอกสารทางเทคนิค</sub>
