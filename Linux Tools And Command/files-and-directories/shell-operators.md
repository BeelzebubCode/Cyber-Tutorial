# ⚙️ Shell Operators in Linux

> รวมคำสั่งและตัวดำเนินการ (operators) สำหรับควบคุม flow ของคำสั่งใน shell เช่น การ pipe ข้อมูล หรือรันคำสั่งแบบมีเงื่อนไข

---

## 🔗 คำสั่งพื้นฐาน

| Operator | ความหมาย | ตัวอย่าง |
|----------|-----------|-----------|
| `|` (pipe) | ส่ง output ของคำสั่งหนึ่งไปยังอีกคำสั่ง | `cat file.txt \| grep "admin"` |
| `&&` | รันคำสั่งถัดไป *เมื่อคำสั่งก่อนหน้าสำเร็จ (exit code = 0)* | `mkdir test && cd test` |
| `&` | รันคำสั่งใน background | `ping google.com &` |

---

## 📂 ตัวอย่างการใช้งานจริง

```bash
cat users.txt | grep admin

mkdir workspace && cd workspace

ping google.com &
echo "Ping started in background"
```

---

## 💡 เคล็ดลับ

- ใช้ pipe ร่วมกับ `grep`, `sort`, `uniq`, `cut` ได้
- ใช้ `&&` เชื่อมหลายคำสั่งที่ต้องการให้ทำต่อเมื่อไม่มี error
- ใช้ `jobs` ดูคำสั่งที่รันอยู่เบื้องหลัง และ `fg` เพื่อดึงกลับมาด้านหน้า

---

> 🧠 Operator เหล่านี้ช่วยให้เราสามารถเขียนคำสั่งที่ยืดหยุ่น และควบคุมลำดับการทำงานได้แบบมืออาชีพ
