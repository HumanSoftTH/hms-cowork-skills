---
name: acp-config
description: "ตรวจสอบและตั้งค่า ACP credentials (username/password) สำหรับ MCP servers acp-analytics และ acp-crm"
---

# /acp-config — ACP Credential Setup

## Step 1: เช็คสถานะ credentials ปัจจุบัน

รันคำสั่งนี้เพื่อดูว่ามี credentials อยู่แล้วหรือยัง:

**Windows (PowerShell):**
```powershell
$s = Get-Content "$env:USERPROFILE\.claude\settings.json" | ConvertFrom-Json
$cfg = $s.pluginConfigs.'hms-cowork-skills@hms-cowork-skills'.options
if ($cfg -and $cfg.ACP_USERNAME) { "CONFIGURED:$($cfg.ACP_USERNAME)" } else { "NOT_CONFIGURED" }
```

**Mac/Linux:**
```bash
python3 -c "
import json,os
s=json.load(open(os.path.expanduser('~/.claude/settings.json')))
cfg=s.get('pluginConfigs',{}).get('hms-cowork-skills@hms-cowork-skills',{}).get('options',{})
print('CONFIGURED:'+cfg['ACP_USERNAME'] if cfg.get('ACP_USERNAME') else 'NOT_CONFIGURED')
"
```

## Step 2: ตามผลลัพธ์

### ถ้าได้ NOT_CONFIGURED

ถามผู้ใช้ทันทีในข้อความเดียวว่า:

```
ยังไม่มี ACP credentials — กรุณากรอก:

**ACP Username** (email ที่ใช้ login ACP):
**ACP Password**:
```

รอผู้ใช้ตอบ แล้วไปทำ Step 3

### ถ้าได้ CONFIGURED:<username>

แสดง:
```
✓ ตั้งค่าแล้ว: <username>

ต้องการเปลี่ยน credentials ไหม?
```

- ถ้าไม่ → บอก "พร้อมใช้งานแล้ว" แล้วจบ
- ถ้าใช่ → ถาม username และ password ใหม่ แล้วไปทำ Step 3

## Step 3: บันทึก credentials

เมื่อได้ username + password จากผู้ใช้แล้ว รัน:

```
claude plugin install hms-cowork-skills@hms-cowork-skills --config ACP_USERNAME=<username> --config ACP_PASSWORD=<password>
```

## Step 4: ทดสอบ

หลังบันทึกแล้ว ทดสอบโดยเรียก tool **getLeadReport** พร้อม date_from/date_to ของเดือนปัจจุบัน

- สำเร็จ → "✓ เชื่อมต่อ ACP สำเร็จ พร้อมใช้งานแล้ว"
- error "Missing required environment variable" → "กรุณา restart Claude แล้วลองใหม่"
- error login / 401 → "Username หรือ Password ไม่ถูกต้อง กรุณาตรวจสอบ"
