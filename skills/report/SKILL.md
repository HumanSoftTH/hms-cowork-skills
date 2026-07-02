---
name: report
description: "ดึง report สำเร็จรูปจาก ACP: Commission รายเดือน, Domain Register ใหม่, Lead report, Domain หมดอายุ, Follow-up summary"
---

# /report — ACP Reports Guide

ใช้เมื่อต้องการดู report สำเร็จรูป ไม่ว่าจะถามตรงๆ หรือพิมพ์ `/report`

## Step 1: ถามว่าต้องการ report อะไร

ถ้าผู้ใช้ยังไม่ระบุ ให้แสดงเมนู:

```
ต้องการ report อะไรครับ?

1. Commission — สรุป commission รายเดือนแยก role
2. Domain Register — รายชื่อ domain ที่สมัครใหม่ในช่วงเวลา
3. Lead Report — สรุป lead แยก source และ status
4. Domain หมดอายุ — รายชื่อ domain ที่หมด / ใกล้หมดอายุ
5. Follow-up Summary — สรุปการ follow-up ในช่วงเวลา
```

## Step 2: รวบรวม parameters

### เมนู 1: Commission Report

ถาม: "ต้องการดู commission เดือนไหนครับ? (เช่น พ.ค. 2026)"
ถาม: "ต้องการดูทุก role หรือเฉพาะ role? (sale / trainer / sale_co / all)"

แปลงเดือนไทย → YYYY-MM
แปลง role: ทุก role=all, ขาย=sale, เทรนเนอร์=trainer, sale_co=sale_co

เรียก tool: **getCommissionReport**

แสดงผล:
```
💰 Commission Report — พ.ค. 2026

| ชื่อ | Role | จำนวน | Commission |
|------|------|--------|------------|
...

ยอดรวม: ฿XXX,XXX
```

### เมนู 2: Domain Register

ถาม: "ต้องการดู domain ที่สมัครในช่วงไหนครับ? (เช่น 1–30 มิ.ย. 2026)"

แปลงวันที่ → YYYY-MM-DD

เรียก tool: **getDomainRegisterReport**

แสดงผล: รายชื่อ domain ใหม่ พร้อมวันสมัคร, แพ็กเกจ, ช่องทาง
สรุป: จำนวนทั้งหมด, แยก package top 3, แยก channel top 3

### เมนู 3: Lead Report

ถาม: "ต้องการดู lead ในช่วงไหนครับ? (เช่น มิ.ย. 2026)"

แปลงเป็น date_from, date_to (YYYY-MM-DD)

เรียก tool: **getLeadReport**

แสดงผล: จำนวน lead แยก source, สรุป conversion rate, top channels

### เมนู 4: Domain หมดอายุ

ถาม: "ต้องการดู domain ที่หมดอายุในช่วงไหน? (เช่น เดือน ก.ค.–ส.ค. 2026 หรือดูทั้งหมด)"

ถ้าไม่ระบุช่วง → ไม่ส่ง date_from/date_to (ดูทั้งหมด)

เรียก tool: **getExpiredCompanyReport**

แสดงผล:
```
⚠️ Domain หมดอายุ / ใกล้หมดอายุ

| บริษัท | Domain | วันหมดอายุ | แพ็กเกจ | ผู้ดูแล |
...

รวม X รายการ — หมดแล้ว Y / ใกล้หมด (30 วัน) Z
```

### เมนู 5: Follow-up Summary

ถาม: "ต้องการดู follow-up ที่นัดในช่วงไหน? (เช่น สัปดาห์นี้ / เดือนนี้)"

แปลงเป็น followup_from, followup_to (YYYY-MM-DD)
- สัปดาห์นี้: จันทร์–อาทิตย์ของสัปดาห์ปัจจุบัน
- เดือนนี้: 01–สิ้นเดือนของเดือนปัจจุบัน

เรียก tool: **getFollowUpReport**

แสดงผล: จำนวน follow-up แยก agent, แยก topic, แยก outcome
สรุป: ทำไปแล้ว X / ยังไม่ได้ทำ Y / นัดใหม่ Z

## Step 3: เสนอ next step

หลังแสดง report เสนอ:
- "ต้องการ filter เพิ่มเติมไหมครับ? เช่น เฉพาะ agent คนนึง หรือ channel ใดช่อง"
- "ต้องการ export เป็น list รายละเอียดไหมครับ?"
- "ต้องการ drilldown ดูรายละเอียดของรายการใดไหมครับ? (ใช้ /analytics)"

## หมายเหตุ

- ข้อมูลดึงจาก ACP production real-time
- Follow-up report กรองตามวันที่ **นัด** follow-up ไม่ใช่วันที่สร้าง
- ถ้าไม่ระบุ date range อาจ timeout ให้ระบุช่วงไม่เกิน 3 เดือนต่อครั้ง
