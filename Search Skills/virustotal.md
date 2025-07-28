# 🦠 VirusTotal - วิเคราะห์มัลแวร์และ URL ที่น่าสงสัย

> [VirusTotal](https://www.virustotal.com/) เป็นบริการฟรีที่ใช้ตรวจสอบไฟล์, URL, โดเมน และแฮชของไฟล์ว่าเป็นมัลแวร์หรือไม่ โดยใช้ engine จากแอนตี้ไวรัสมากกว่า 60 ตัว พร้อมข้อมูลเชิงลึกจาก community

🌐 เว็บไซต์: <a href="https://www.virustotal.com/" target="_blank">https://www.virustotal.com/</a>

---

## 🧠 เหมาะกับใคร
- นักวิเคราะห์มัลแวร์ / Blue Team → ตรวจสอบ IOC (Indicator of Compromise)
- Pentester → วิเคราะห์ไฟล์ต้องสงสัยที่โหลดจากเป้าหมาย
- นักเรียน / OSINT → ตรวจสอบความเสี่ยงของไฟล์หรือโดเมนก่อนคลิก

---

## 🔍 สิ่งที่สามารถตรวจสอบได้

| ประเภทข้อมูล | ตัวอย่าง |
|---------------|-----------|
| **ไฟล์** | `.exe`, `.zip`, `.pdf`, `.docx` เป็นต้น |
| **URL** | `http://example.com/malware.exe` |
| **Hash** | `SHA256`, `MD5`, `SHA1` ของไฟล์ |
| **โดเมน** | `example.com` |
| **IP Address** | `8.8.8.8` |

---

## 📥 วิธีใช้งาน
1. เข้าเว็บ <a href="https://www.virustotal.com/" target="_blank">VirusTotal</a>
2. เลือก tab: File, URL, Domain, IP ตามต้องการ
3. อัปโหลดไฟล์ / วาง URL หรือ hash → กด Submit

---

## 🔬 การตีความผลลัพธ์
- **Detection ratio**: เช่น `3/62` → ตรวจพบ 3 จาก 62 engine
- **แท็บ Behavior / Relations**: แสดงการเชื่อมต่อของมัลแวร์ เช่น ไปยัง IP ไหน, สร้าง process อะไร
- **Community Insight**: ความคิดเห็นจากผู้ใช้ เช่น การยืนยันว่าไฟล์อันตรายจริง

---

## 🔧 VirusTotal API
- สามารถใช้ API เพื่อตรวจสอบ hash หรืออัปโหลดไฟล์แบบอัตโนมัติได้
- สมัครบัญชีเพื่อขอ API Key ได้ฟรี (แบบจำกัด rate)

ตัวอย่าง Python script:
```python
import requests

url = 'https://www.virustotal.com/api/v3/files/<FILE_ID>'
headers = {
    'x-apikey': 'YOUR_API_KEY'
}
response = requests.get(url, headers=headers)
print(response.json())
```

---

## 💡 เทคนิคเพิ่มเติม
- เช็ก hash ก่อนอัปโหลดจริง เพื่อไม่แพร่มัลแวร์
- สแกน URL ที่ได้จาก phishing email
- ตรวจสอบ `.zip`, `.js`, `.docm` หรือไฟล์แนบที่คนส่งมาทางแปลก ๆ

---

## ⚠️ คำเตือน
> VirusTotal คือเครื่องมือสำหรับตรวจสอบ ไม่ควรใช้แทนแอนตี้ไวรัสหลักในระบบคุณ และห้ามอัปโหลดข้อมูลลับ/ภายในองค์กร

<sub>ที่มา: virustotal.com, API doc, และการใช้งานจริง</sub>
