# 🗂️ Linux Directory Structure

> Linux ใช้ระบบไฟล์แบบต้นไม้ (hierarchical) โดยเริ่มจากรากคือ `/` แล้วแตกออกเป็นไดเรกทอรีย่อย

---

## 🧭 สารบัญ
- [โครงสร้างพื้นฐาน](#โครงสร้างพื้นฐาน)
- [แสดงโครงสร้างไดเรกทอรีด้วย tree](#แสดงโครงสร้างไดเรกทอรีด้วย-tree)
- [จุดน่าสังเกต](#จุดน่าสังเกต)
- [ตัวอย่างคำสั่ง](#ตัวอย่างคำสั่ง)

---

## 🌳 โครงสร้างพื้นฐาน

| ไดเรกทอรี | คำอธิบาย |
|-----------|-----------|
| `/` | Root directory — จุดเริ่มต้นของระบบไฟล์ทั้งหมด |
| `/bin` | Binary system command (คำสั่งพื้นฐาน เช่น `ls`, `cp`, `rm`) |
| `/boot` | ไฟล์บูต เช่น kernel, grub |
| `/dev` | อุปกรณ์ในระบบ (device files) เช่น `/dev/sda` |
| `/etc` | ไฟล์ config ทั่วไปของระบบ (text-based config) |
| `/home` | โฟลเดอร์ของผู้ใช้แต่ละคน เช่น `/home/pong` |
| `/lib`, `/lib64` | ไลบรารีที่ใช้โดย binary ใน `/bin` และ `/sbin` |
| `/media`, `/mnt` | จุด mount สำหรับ USB/CD/อุปกรณ์ภายนอก |
| `/opt` | แอปพลิเคชันเสริมที่ติดตั้งภายนอก package manager |
| `/proc` | ข้อมูล kernel และ process (virtual FS) |
| `/root` | โฟลเดอร์ของ root user เท่านั้น |
| `/run` | ไฟล์ runtime ที่เปลี่ยนตามการบูต |
| `/sbin` | คำสั่งระบบที่ใช้สำหรับ root เช่น `shutdown`, `fsck` |
| `/srv` | ข้อมูลของ service ต่าง ๆ เช่น FTP, HTTP |
| `/sys` | ข้อมูลเกี่ยวกับ kernel/device (เหมือน `/proc`) |
| `/tmp` | ไฟล์ชั่วคราว ลบเมื่อรีบูต |
| `/usr` | โปรแกรมและไลบรารีสำหรับผู้ใช้ทั่วไป (แบ่งออกเป็น `/usr/bin`, `/usr/lib`, `/usr/share`) |
| `/var` | ไฟล์ที่เปลี่ยนแปลงบ่อย เช่น log, mail, spool |

---

## 🌲 แสดงโครงสร้างไดเรกทอรีด้วย `tree`

ติดตั้งคำสั่ง `tree` เพื่อดูโครงสร้างไดเรกทอรีแบบภาพรวม:

```bash
sudo apt install tree
```

ตัวอย่างการแสดงโครงสร้างระดับบนสุด:

```bash
tree / -L 2
```

ตัวอย่างผลลัพธ์:
```
/  
├── bin  
├── boot  
├── dev  
├── etc  
├── home  
│   └── pong  
├── lib  
├── lib64  
├── media  
├── mnt  
├── opt  
├── proc  
├── root  
├── run  
├── sbin  
├── srv  
├── sys  
├── tmp  
├── usr  
│   ├── bin  
│   ├── lib  
│   └── share  
└── var  
```

---

## 🔍 จุดน่าสังเกต

- `/etc/passwd` เก็บข้อมูล user ทั้งระบบ
- `/etc/shadow` เก็บรหัสผ่านแบบ hash
- `/var/log/` รวม log ระบบ เช่น `auth.log`, `syslog`
- `/proc/cpuinfo`, `/proc/meminfo` — ดูข้อมูล CPU/RAM

---

## 🧪 ตัวอย่างคำสั่ง

```bash
ls /
ls /etc
cat /etc/os-release
cat /proc/cpuinfo
tree / -L 2
```

> 📘 เพิ่มเติม: ใช้ `man hier` เพื่อดูคำอธิบายโครงสร้างทั้งหมด
