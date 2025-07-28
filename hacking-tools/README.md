# 🧨 Hacking Tools Collection

> รวมเครื่องมือยอดนิยมที่ใช้ในโลกของการแฮ็ก ทั้งในด้าน Red Team, CTF, Bug Bounty, OSCP และการเรียนรู้ความปลอดภัยไซเบอร์เชิงรุก

---

## 🧭 หมวดหมู่เครื่องมือ

| หมวดหมู่ | คำอธิบาย |
|-----------|----------|
| [`recon/`](./recon/) | รวบรวมเครื่องมือรวบรวมข้อมูล (Reconnaissance) จากเป้าหมาย เช่น subdomain, email, IP |
| [`web-scanning/`](./web-scanning/) | สแกนและวิเคราะห์เว็บ เช่น directory brute-force, CMS scan, เว็บซ่อน |
| [`network-scanning/`](./network-scanning/) | สแกนเครือข่าย พอร์ต และ service ต่าง ๆ เช่น nmap, masscan |
| [`exploitation/`](./exploitation/) | ยิง exploit หรือ payload เข้าเป้าหมาย |
| [`privilege-escalation/`](./privilege-escalation/) | ยกระดับสิทธิ์จาก user ธรรมดาไปสู่ root/admin |
| [`password-attacks/`](./password-attacks/) | Brute-force, crack password hash, wordlist |
| [`wireless-attacks/`](./wireless-attacks/) | เจาะระบบไร้สาย เช่น Wi-Fi, WPA2 |
| [`social-engineering/`](./social-engineering/) | หลอกล่อเหยื่อ เช่น phishing, spoofing |
| [`reverse-shells/`](./reverse-shells/) | รวมคำสั่งและเทคนิค reverse shell |
| [`persistence/`](./persistence/) | เทคนิคฝังตัวให้กลับมาได้แม้เหยื่อ reboot |
| [`post-exploitation/`](./post-exploitation/) | ขโมยข้อมูล, credentials, ยึดระบบภายใน |
| [`malware-analysis/`](./malware-analysis/) | วิเคราะห์ไฟล์ต้องสงสัย, reverse malware |
| [`steganography/`](./steganography/) | ซ่อนข้อมูลในรูป, เสียง, ไฟล์ ฯลฯ |
| [`osint/`](./osint/) | เครื่องมือ OSINT เช่น ดึงข้อมูลจากเว็บสาธารณะ |
| [`forensic-tools/`](./forensic-tools/) | กู้ไฟล์, วิเคราะห์ memory, ตรวจ log |

---

## 🚨 หมายเหตุ

- เครื่องมือเหล่านี้ส่วนใหญ่ใช้ในทาง **ethical hacking** เท่านั้น
- ห้ามใช้เพื่อโจมตีหรือเจาะระบบที่คุณ **ไม่มีสิทธิ์อย่างชัดเจน**
- ใช้ใน Lab, CTF, หรือกับเครื่องของตัวเองเพื่อการเรียนรู้

---

## 📦 วิธีใช้ Repo นี้

1. เข้าแต่ละโฟลเดอร์ตามหมวด
2. เปิดไฟล์ `.md` เช่น `gobuster.md`, `nmap.md` เพื่อดูวิธีใช้งาน
3. ปรับคำสั่งให้เข้ากับเป้าหมายจริง
4. ใช้ร่วมกับ TryHackMe / HackTheBox เพื่อฝึกจริง

---

> 🎯 ศาสตร์แห่งการแฮ็ก = ความรู้ + จริยธรรม  
> จงเรียนรู้เพื่อป้องกันและเข้าใจ ไม่ใช่เพื่อทำลาย 🛡️
