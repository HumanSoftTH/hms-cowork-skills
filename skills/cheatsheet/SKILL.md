---
name: cheatsheet
description: "เปิดคู่มือ ACP Cowork Skills: แสดงรายการ skills ทั้งหมด, example prompts, และวิธีใช้งาน"
---

# /cheatsheet — ACP Cowork Skills Guide

เปิดคู่มือ HTML ที่อธิบาย skills ทั้งหมดพร้อม example prompts

## Step 1: หา install path แล้วเปิดไฟล์คู่มือ

**Windows:**
```powershell
$pluginsJson = Get-Content "$env:USERPROFILE\.claude\plugins\installed_plugins.json" | ConvertFrom-Json
$installPath = $pluginsJson.plugins.'hms-cowork-skills@hms-cowork-skills'[0].installPath
$htmlPath = Join-Path $installPath "docs\cheatsheet.html"
if (Test-Path $htmlPath) { Start-Process $htmlPath } else { Write-Output "NOT_FOUND:$htmlPath" }
```

**Mac/Linux:**
```bash
python3 -c "
import json,os,webbrowser
d=json.load(open(os.path.expanduser('~/.claude/plugins/installed_plugins.json')))
path=d['plugins']['hms-cowork-skills@hms-cowork-skills'][0]['installPath']
html=os.path.join(path,'docs','cheatsheet.html')
if os.path.exists(html): webbrowser.open('file://'+html)
else: print('NOT_FOUND:'+html)
"
```

ถ้าได้ NOT_FOUND ให้บอกผู้ใช้:
"ไม่พบไฟล์คู่มือ กรุณารัน `claude plugin update hms-cowork-skills` แล้วลองใหม่"

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
   ตัวอย่าง: "domain ที่สมัครใหม่เดือนนี้"
             "domain ที่จะหมดอายุใน 30 วัน"
             "lead report เดือน มิ.ย. 2026"
             "follow-up สัปดาห์นี้สรุปเป็นยังไง"

🔹 /cheatsheet — เปิดคู่มือนี้

🔹 /acp-config — ตั้งค่า ACP username/password

💡 เคล็ดลับ: พิมพ์คำถามธรรมชาติได้เลย ไม่ต้องพิมพ์ /skill ก็ได้
   เช่น "ดู funnel เดือน ม.ค.–มิ.ย." Claude จะรู้ว่าต้องใช้ /analytics
```
