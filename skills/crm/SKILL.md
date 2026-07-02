---
name: crm
description: "ค้นหาและดูข้อมูลลูกค้าจาก ACP CRM: ค้นหาลูกค้า, ดูข้อมูลพื้นฐาน, follow-up, incident, ticket, payment status, customer stage"
---

# /crm — ACP CRM Guide

ใช้เมื่อต้องการค้นหาหรือดูข้อมูลลูกค้า ไม่ว่าจะถามตรงๆ หรือพิมพ์ `/crm`

## Step 1: ถามว่าต้องการอะไร

ถ้าผู้ใช้ยังไม่ระบุ ให้แสดงเมนู:

```
ต้องการข้อมูลลูกค้าแบบไหนครับ?

1. ค้นหาลูกค้า — หาจากชื่อบริษัท, ชื่อ domain, หรือชื่อคน
2. ข้อมูลพื้นฐานลูกค้า — แพ็กเกจ, วันหมดอายุ, ช่องทาง, ขนาดบริษัท
3. Follow-up ล่าสุด — ดูบันทึก follow-up และนัดหมาย
4. Incident / ปัญหา — ดูรายการ incident ที่เกิดขึ้น
5. Ticket — ค้นหาหรือดูรายละเอียด ticket
6. Payment Status — ดูสถานะการชำระเงิน
7. Customer Stage — ดู stage ปัจจุบันใน pipeline
```

## Step 2: รวบรวมข้อมูลที่ต้องการ

### เมนู 1: ค้นหาลูกค้า

ถาม: "ต้องการค้นหาจากอะไรครับ? (ชื่อบริษัท, domain, หรือชื่อคน)"

เรียก tool: **searchCustomers**
- parameter: `query` = คำค้นหา

แสดงผล: รายการบริษัทที่พบ พร้อม domain และ package
ถ้าพบหลายรายการ ให้ผู้ใช้เลือก แล้วถามต่อว่า "ต้องการดูข้อมูลอะไรเพิ่มเติมครับ?"

### เมนู 2: ข้อมูลพื้นฐาน

ถาม: "ชื่อบริษัทหรือ domain ที่ต้องการดูครับ?"

ถ้ายังไม่รู้ customer_id → เรียก **searchCustomers** ก่อน แล้วนำ ID มาเรียก **getCustomerBasicInfo**

แสดงผล:
```
🏢 [ชื่อบริษัท] · [domain]
แพ็กเกจ: [package_name]
หมดอายุ: [วันที่]
ขนาด: [จำนวน employee] คน ([Sizing])
ช่องทาง: [channel]
ผู้ดูแล: [sale/support ชื่อ]
```

### เมนู 3: Follow-up

เรียก tool: **getCustomerFollowUps**

แสดงผล: รายการ follow-up เรียงตามวันที่ล่าสุด พร้อม topic และผลลัพธ์

### เมนู 4: Incident

เรียก tool: **getCustomerIncidents**

แสดงผล: รายการ incident พร้อมสถานะและวันที่

### เมนู 5: Ticket

ถามว่า "ค้นหา ticket หรือดู ticket เฉพาะรายการ?"
- ค้นหา → **searchTickets** (ใส่ query)
- ดูรายละเอียด → **getTicketDetails** (ใส่ ticket_id)
- ดู ticket ของลูกค้า → **getCustomerTickets**

### เมนู 6: Payment Status

เรียก tool: **getCustomerPaymentStatus**

แสดงผล: สถานะการชำระเงิน, ยอดค้างชำระ, ประวัติการชำระ

### เมนู 7: Customer Stage

เรียก tool: **getCustomerStage**

แสดงผล: stage ปัจจุบัน, วันที่เข้า stage, ประวัติการเปลี่ยน stage

## Step 3: แสดงผลและเสนอ next step

หลังแสดงข้อมูลแต่ละอย่าง เสนอสิ่งที่ทำต่อได้:
- "ต้องการดู follow-up หรือ incident ด้วยไหมครับ?"
- "ต้องการดูลูกค้ารายอื่นในผลการค้นหาไหมครับ?"
- "ต้องการ export รายชื่อลูกค้าที่ค้นพบไหมครับ?"

## หมายเหตุ

- ข้อมูลดึงจาก ACP production real-time
- การค้นหาใช้ LIKE search ดังนั้นพิมพ์แค่บางส่วนก็พอ
- ถ้าพิมพ์ชื่อภาษาไทยไม่เจอ ลองพิมพ์เป็นภาษาอังกฤษหรือ domain
