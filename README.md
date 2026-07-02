# hms-cowork-skills

Claude Code plugin สำหรับทีม Cowork HumanSoft — ถามข้อมูล ACP เป็นภาษาไทยได้เลย ไม่ต้องรู้ API หรือ parameter ใดๆ

## Skills ที่มี

| Skill | ใช้สำหรับ |
|-------|----------|
| `/analytics` | Funnel รายเดือน, Cohort Conversion, Key Metrics (Financial/Conversion/Retention/Marketing), Success Call Performance พร้อม drilldown |
| `/crm` | ค้นหาลูกค้า, ข้อมูลพื้นฐาน, Follow-up, Incident, Ticket, Payment, Stage |
| `/report` | Domain Register ใหม่, Lead Report, Domain หมดอายุ, Follow-up Summary |
| `/cheatsheet` | เปิดคู่มือ HTML พร้อม example prompts ทั้งหมด |
| `/acp-config` | ตั้งค่า ACP username/password สำหรับเชื่อมต่อข้อมูล |

> ไม่จำเป็นต้องพิมพ์ `/skill` — ถามตรงๆ ก็ได้ เช่น _"ดู funnel เดือนนี้"_ Claude จะเลือก skill ที่เหมาะสมให้อัตโนมัติ

---

## ติดตั้ง

ต้องมี [Claude Code](https://claude.ai/code) และ Node.js ≥ 18

```bash
claude plugin marketplace add https://github.com/HumanSoftTH/hms-cowork-skills
claude plugin install hms-cowork-skills
```

จากนั้น restart Claude แล้วตั้งค่า credentials:

```
/acp-config
```

Claude จะถาม ACP username และ password ให้กรอก — ทำครั้งเดียวก็พร้อมใช้

### ติดตั้งพร้อม credentials ในคำสั่งเดียว

```bash
claude plugin install hms-cowork-skills \
  --config ACP_USERNAME=your.email@humansoft.co.th \
  --config ACP_PASSWORD=yourpassword
```

### อัปเดต

```bash
claude plugin update hms-cowork-skills
```

---

## ตัวอย่างการใช้งาน

### /analytics

```
ดู funnel ม.ค.–มิ.ย. 2026 เฉพาะ HumanSoft สรุปให้หน่อย
cohort สมัครแล้วจ่ายเงิน Q1 2026 เป็น % conversion
Financial key metrics ปี 2026 สรุปตัวเลขสำคัญ
ขอดูรายชื่อ domain เบื้องหลัง funnel เดือน เม.ย. stage Paid
```

### /crm

```
ค้นหาลูกค้าชื่อ ABC Technology
ดู follow-up ล่าสุดของลูกค้า example.co.th
ticket เปิดค้างของลูกค้า XYZ มีอะไรบ้าง
สถานะการชำระเงินของ ABC ตอนนี้เป็นยังไง
```

### /report

```
lead report เดือน มิ.ย. 2026 แยก source
domain ที่จะหมดอายุใน ก.ค.–ส.ค. 2026
domain ที่สมัครใหม่เดือนนี้
follow-up สัปดาห์นี้สรุปเป็นยังไง
```

### /acp-config

```
/acp-config
```

---

## Requirements

- Claude Code CLI (ไม่รองรับ Claude cowork web)
- Node.js ≥ 18
- ACP username + password (บัญชีเดียวกับที่ใช้ login ACP ปกติ)

---

## Changelog

### v0.5.0
- แก้ `/cheatsheet` — หา install path จาก `installed_plugins.json` แทน hardcode path เพื่อให้เปิดไฟล์ได้ถูกต้องทุก OS
- แก้ `/cheatsheet` — Mac/Linux ใช้ `webbrowser.open()` แทน `open` เพื่อรองรับ Linux ด้วย
- เพิ่ม `/acp-config` ใน in-chat summary ของ `/cheatsheet`

### v0.4.0
- แก้ `/acp-config` — ถาม username/password ในข้อความแรกทันที ไม่รอให้ user พิมพ์เอง

### v0.3.0
- เพิ่ม skill `/acp-config` สำหรับตั้งค่า credentials แบบ guided

### v0.2.0
- เพิ่ม MCP connectors (`acp-analytics`, `acp-crm`) bundled ในตัว ไม่ต้องติดตั้งแยก
- เพิ่ม `userConfig` — ใส่ ACP username/password ตอน install ได้เลย

### v0.1.0
- เปิดตัว 4 skills: `/analytics`, `/crm`, `/report`, `/cheatsheet`
