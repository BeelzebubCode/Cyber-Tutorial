# 🌐 Shodan - เครื่องมือค้นหาอุปกรณ์บนอินเทอร์เน็ต

> Shodan คือเครื่องมือค้นหาสำหรับอุปกรณ์ที่เชื่อมต่ออินเทอร์เน็ต เช่น Server, Router, Webcam, IoT, SCADA ซึ่งต่างจาก Google ที่เน้นข้อมูลจากเว็บไซต์

---

## 🧠 เหมาะกับใคร
- Pentester / Red Team → หาช่องโหว่ระบบที่เปิด public
- Blue Team / SOC → ตรวจสอบอุปกรณ์องค์กรตัวเองว่าหลุดสู่ public หรือไม่
- OSINT / Bug Bounty → หาเป้าหมายที่น่าสนใจแบบไม่ต้องสแกนเอง

---

## 🔍 ตัวอย่างการค้นหา (Shodan Dorks)

| คำค้น | ความหมาย |
|--------|-----------|
| `apache 2.4.1` | ค้นหาอุปกรณ์ที่เปิดเผย banner Apache เวอร์ชันนี้ |
| `port:22 country:TH` | ค้นหา SSH ที่อยู่ในประเทศไทย |
| `os:"Windows 7"` | ค้นหาเครื่องที่ยังใช้ Windows 7 |
| `org:"TOT"` | อุปกรณ์ที่ใช้จากองค์กร TOT |
| `product:"GoAhead"` | กล้อง/Router ที่ใช้ embedded web server GoAhead |

---

## 🧪 ตัวอย่างขั้นสูง
```sh
port:21 Anonymous user logged in
http.title:"WebcamXP"
ssl:"expired" country:US
has_screenshot:true city:"Bangkok"
```

---

## 🔐 ใช้งานจริง
```bash
# ใช้งานผ่าน CLI ต้องติดตั้ง shodan-cli ก่อน
shodan init <API_KEY>
shodan search "product:Apache country:TH" --limit 10
```

หรือเข้าเว็บ: <a href="https://www.shodan.io" target="_blank">Shodan.io 🌐</a>

---

## 💡 เทคนิคเพิ่มเติม
- สมัครบัญชีฟรี จะค้นหาได้ 50 results / search
- API Key ใช้ได้ทั้ง CLI และ script (Python, Bash)
- ใช้ร่วมกับเครื่องมืออื่น เช่น Nmap, Metasploit เพื่อสแกนเป้าหมายต่อ

---

## ⚠️ คำเตือน
> ห้ามโจมตีหรือสแกนอุปกรณ์ที่คุณไม่มีสิทธิ์อย่างชัดเจน ใช้เพื่อการศึกษาเท่านั้น

<sub>ข้อมูลจาก Shodan.io และประสบการณ์การใช้งานจริง</sub>
