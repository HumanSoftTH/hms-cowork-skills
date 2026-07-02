---
name: acp-config
description: "ตรวจสอบและตั้งค่า ACP credentials (username/password) สำหรับ MCP servers acp-analytics และ acp-crm"
---

# /acp-config — ACP Credential Setup

ใช้เมื่อต้องการตรวจสอบหรือตั้งค่า ACP username/password

## Step 1: ตรวจสอบสถานะปัจจุบัน

อ่านไฟล์ settings เพื่อเช็คว่ามี credentials อยู่แล้วหรือไม่

**Windows:**
```powershell
$s = Get-Content "$env:USERPROFILE\.claude\settings.json" | ConvertFrom-Json
$cfg = $s.pluginConfigs.'hms-cowork-skills@hms-cowork-skills'.options
if ($cfg) {
  Write-Output "USERNAME: $($cfg.ACP_USERNAME)"
  Write-Output "PASSWORD: $('*' * $cfg.ACP_PASSWORD.Length)"
} else {
  Write-Output "NOT_CONFIGURED"
}
```

**Mac/Linux:**
```bash
python3 -c "
import json, os
s = json.load(open(os.path.expanduser('~/.claude/settings.json')))
cfg = s.get('pluginConfigs', {}).get('hms-cowork-skills@hms-cowork-skills', {}).get('options', {})
if cfg:
    print('USERNAME:', cfg.get('ACP_USERNAME', '-'))
    print('PASSWORD:', '*' * len(cfg.get('ACP_PASSWORD', '')))
else:
    print('NOT_CONFIGURED')
"
```

## Step 2: แสดงผลและถามผู้ใช้

### กรณี NOT_CONFIGURED

แสดง:
```
ยังไม่ได้ตั้งค่า ACP credentials

กรุณากรอก:
```

แล้วถามผู้ใช้ว่า:
- "ACP Username (email ที่ใช้ login ACP):"
- "ACP Password:"

### กรณี มี credentials อยู่แล้ว

แสดง:
```
✓ ACP credentials ตั้งค่าแล้ว
  Username: <username>
  Password: ••••••••

ต้องการอัปเดตไหม? (ใช่/ไม่)
```

ถ้าไม่ต้องการอัปเดต → จบ skill พร้อมบอกว่า "พร้อมใช้งานแล้ว ลองพิมพ์ 'ดู funnel เดือนนี้' ได้เลย"

## Step 3: บันทึก credentials

เมื่อได้ username และ password จากผู้ใช้แล้ว ให้รัน:

**Windows:**
```powershell
claude plugin install hms-cowork-skills@hms-cowork-skills --config ACP_USERNAME=<username> --config ACP_PASSWORD=<password>
```

**Mac/Linux:**
```bash
claude plugin install hms-cowork-skills@hms-cowork-skills --config ACP_USERNAME=<username> --config ACP_PASSWORD=<password>
```

หมายเหตุ: ถ้า plugin ติดตั้งแล้ว คำสั่งนี้จะ update credentials เท่านั้น ไม่ reinstall

## Step 4: ทดสอบการเชื่อมต่อ

หลังบันทึกแล้ว ให้ทดสอบโดยเรียก tool จริง:

เรียก tool **getLeadReport** พร้อม date_from/date_to ของเดือนปัจจุบัน

- ถ้าสำเร็จ → แสดง "✓ เชื่อมต่อ ACP สำเร็จ พร้อมใช้งานแล้ว"
- ถ้า error "Missing required environment variable" → แจ้งว่า "กรุณา restart Claude แล้วลองใหม่"
- ถ้า error login failed / 401 → แจ้งว่า "Username หรือ Password ไม่ถูกต้อง กรุณาตรวจสอบ"

## หมายเหตุ

- Credentials เก็บใน `~/.claude/settings.json` บนเครื่องของแต่ละคน ไม่ได้ upload ไปที่ไหน
- ต้อง restart Claude หลังตั้งค่าครั้งแรก เพื่อให้ MCP server โหลด credentials ใหม่
- ถ้าเปลี่ยน password ACP ให้รัน `/acp-config` อีกครั้งเพื่ออัปเดต
