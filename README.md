# hms-cowork-skills

Claude Code plugin สำหรับทีม Cowork HumanSoft — ถามข้อมูล ACP เป็นภาษาไทยได้เลย ไม่ต้องรู้ API หรือ parameter ใดๆ

## Skills ที่มี

| Skill | ใช้สำหรับ |
|-------|----------|
| `/analytics` | Funnel รายเดือน, Cohort Conversion, Key Metrics (Financial/Conversion/Retention/Marketing), Success Call Performance พร้อม drilldown |
| `/crm` | ค้นหาลูกค้า, ข้อมูลพื้นฐาน, Follow-up, Incident, Ticket, Payment, Stage |
| `/report` | Domain Register ใหม่, Lead Report, Domain หมดอายุ, Follow-up Summary |
| `/cheatsheet` | เปิดคู่มือ HTML พร้อม example prompts ทั้งหมด |

> ไม่จำเป็นต้องพิมพ์ `/skill` — ถามตรงๆ ก็ได้ เช่น _"ดู funnel เดือนนี้"_ Claude จะเลือก skill ที่เหมาะสมให้อัตโนมัติ

---

## ติดตั้ง

ต้องมี [Claude Code](https://claude.ai/code) ติดตั้งแล้ว จากนั้นรันใน terminal:

```bash
claude plugin marketplace add https://github.com/HumanSoftTH/hms-cowork-skills
claude plugin install hms-cowork-skills
```

Restart Claude แล้วใช้งานได้เลย

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
lead report เดือนนี้ แยก source
follow-up สัปดาห์นี้สรุปเป็นยังไง
```

---

## Requirements

- Claude Code CLI
- MCP servers ที่ต้องการ: `acp-analytics`, `acp-crm`  
  (ติดตั้งแยกต่างหาก — ติดต่อทีม Dev)
