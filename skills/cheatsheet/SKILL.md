---
name: cheatsheet
description: "เปิดคู่มือ ACP Cowork Skills: แสดงรายการ skills ทั้งหมด, example prompts, และวิธีใช้งาน"
---

# /cheatsheet — ACP Cowork Skills Guide

เปิดคู่มือ HTML ที่อธิบาย skills ทั้งหมดพร้อม example prompts

## Step 1: เปิดไฟล์คู่มือ

**Windows:**
```powershell
Start-Process "$env:USERPROFILE\.claude\plugins\hms-cowork-skills\docs\cheatsheet.html"
```

**Mac/Linux:**
```bash
open ~/.claude/plugins/hms-cowork-skills/docs/cheatsheet.html
```

ถ้าหาไฟล์ไม่พบ ให้บอกผู้ใช้ว่า:
"ไม่พบไฟล์คู่มือ กรุณาตรวจสอบว่า plugin hms-cowork-skills ถูก install แล้ว"

## Step 2: แสดงสรุปใน chat

หลังเปิด browser แล้ว ให้แสดงสรุปใน chat ด้วย:

```
📖 ACP Cowork Skills — Quick Reference

🔹 /analytics — วิเคราะห์ข้อมูล Administrator
   ตัวอย่าง: "ดู funnel เดือน ม.ค.–มิ.ย. 2026"
             "Cohort Q1 2026 conversion rate เป็นยังไง"
             "Financial metrics ปี 2026"
             "Success call performance สัปดาห์ที่ 1–26"

🔹 /crm — ค้นหาและดูข้อมูลลูกค้า
   ตัวอย่าง: "ค้นหาลูกค้าชื่อ ABC"
             "ดูข้อมูลบริษัท humansoft.co.th"
             "follow-up ของลูกค้า XYZ ล่าสุด"
             "ticket เปิดค้างของลูกค้า ABC"

🔹 /report — ดู report สำเร็จรูป
   ตัวอย่าง: "commission เดือน มิ.ย. 2026"
             "domain ที่สมัครใหม่เดือนนี้"
             "domain ที่จะหมดอายุใน 30 วัน"
             "follow-up สัปดาห์นี้สรุปเป็นยังไง"

🔹 /cheatsheet — เปิดคู่มือนี้

💡 เคล็ดลับ: พิมพ์คำถามธรรมชาติได้เลย ไม่ต้องพิมพ์ /skill ก็ได้
   เช่น "ดู funnel เดือน ม.ค.–มิ.ย." Claude จะรู้ว่าต้องใช้ /analytics
```
