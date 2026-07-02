---
name: analytics
description: "วิเคราะห์ข้อมูล ACP Administrator: Funnel รายเดือน, Cohort conversion, Key Metrics (Financial/Conversion/Retention/Marketing), Success Call Performance พร้อม drilldown รายละเอียด"
---

# /analytics — ACP Analytics Guide

ใช้เมื่อผู้ใช้ต้องการดูข้อมูลเชิงวิเคราะห์จาก ACP Administrator ไม่ว่าจะถามตรงๆ หรือพิมพ์ `/analytics`

## Step 1: ถามว่าต้องการดูอะไร

ถ้าผู้ใช้ยังไม่ระบุ ให้แสดงเมนู:

```
ต้องการดูข้อมูลอะไรครับ?

1. Funnel รายเดือน — ดู pipeline ตั้งแต่ Register → Paid/Dropoff แยกกลุ่มตัวแทน
2. Cohort Conversion — วิเคราะห์ conversion แต่ละ cohort เดือน (M0/M1/M2...)
3. Key Metrics รายปี — Financial / Conversion / Retention / Marketing
4. Success Call Performance — ผลงาน call team รายสัปดาห์
5. Drilldown รายละเอียด — ขอดูรายการ domain เบื้องหลังตัวเลข
```

## Step 2: รวบรวม parameters

### เมนู 1: Funnel รายเดือน

ถามตามลำดับ:
- "ต้องการดูช่วงเดือนไหนครับ? (เช่น ม.ค.–มิ.ย. 2026)"
- "กรองกลุ่มตัวแทนไหม? ทั้งหมด / เฉพาะ HumanSoft / เฉพาะ Dealer (default: ทั้งหมด)"
- "กรอง Sizing ไหม? S (1–75 คน) / M (76–200 คน) / L (201+ คน) / ทั้งหมด (default: ทั้งหมด)"
- "ฐาน % เริ่มจากจุดไหน? Register / Prospect / Success Call / Setup / Training (default: Register)"

แปลงชื่อเดือนไทย → YYYY-MM ก่อนส่ง API:
- ม.ค.=01, ก.พ.=02, มี.ค.=03, เม.ย.=04, พ.ค.=05, มิ.ย.=06
- ก.ค.=07, ส.ค.=08, ก.ย.=09, ต.ค.=10, พ.ย.=11, ธ.ค.=12

แปลง agent_group: ทั้งหมด=all, HumanSoft=humansoft, Dealer=dealer
แปลง sizing: ทั้งหมด=all_size, S=size_s, M=size_m, L=size_l
แปลง pct_base: Register=register, Prospect=prospect, Success Call=success_call, Setup=setup, Training=training

เรียก tool: **getFunnelReport**

### เมนู 2: Cohort Conversion

ถามตามลำดับ:
- "ต้องการดู cohort ประเภทไหน?"
  ```
  กลุ่ม New Customer:
    1. สมัครแล้วจ่ายเงิน (register → paid)
    2. สมัครแล้ว demo/training (register → meeting)
    3. demo/training แล้วจ่ายเงิน (meeting → paid)
  
  กลุ่ม Onboarding (First Year):
    4. Setup → Success Payroll
    5. Setup → First Payroll
    6. Training → Success Payroll
    7. First Payroll → Success Payroll
  ```
- "ต้องการดูเป็น จำนวน domain หรือ % conversion? (default: จำนวน)"
- "ช่วงเดือนไหน? (เช่น ม.ค.–มิ.ย. 2026)"
- "กรอง Sizing ไหม? (default: ทั้งหมด)"

แปลง data_type:
- 1=register_to_paid, 2=register_to_meeting, 3=meeting_to_paid
- 4=setup_to_success_payroll, 5=setup_to_first_payroll, 6=training_to_success_payroll, 7=first_payroll_to_success_payroll
แปลง display_type: จำนวน=domain_count, %=conversion_rate

เรียก tool: **getCohortReport**

### เมนู 3: Key Metrics รายปี

ถามตามลำดับ:
- "ต้องการดู Key Metrics ด้านไหน?"
  ```
  1. Financial — MRR, ARR, revenue, cash flow, domain active
  2. Conversion — funnel rate, rejection stage, squad performance
  3. Retention — churn, drop-off, ticket, renewal
  4. Marketing — leads by source, paid customer breakdown
  ```
- "ปีไหน? (เช่น 2026)"
- "กรอง Sizing ไหม? (default: ทั้งหมด)"
- "ต้องการดูทุก topic หรือเฉพาะบางอย่าง? (ถามเฉพาะ analytics team)"

แปลง action ตาม metric ที่เลือก:
- 1 → tool **getFinancialKeyMetrics**
- 2 → tool **getConversionKeyMetrics**
- 3 → tool **getRetentionKeyMetrics**
- 4 → tool **getMarketingKeyMetrics**

### เมนู 4: Success Call Performance

ถามตามลำดับ:
- "ต้องการดูช่วงสัปดาห์ไหน? (เช่น สัปดาห์ที่ 1–13 ปี 2026)"
- "กรอง Sizing ไหม? (default: ทั้งหมด)"

แปลงสัปดาห์: สัปดาห์ที่ N ปี YYYY → YYYY-WNN (เช่น สัปดาห์ที่ 1 ปี 2026 → 2026-W01)
คำนวณสัปดาห์ปัจจุบันจากวันที่วันนี้ (วันที่ 1 ม.ค. = สัปดาห์ที่ 1 ไปเรื่อยๆ ทุก 7 วัน)

เรียก tool: **getSuccessCallPerformance**

### เมนู 5: Drilldown รายละเอียด

ถาม:
- "กำลัง drilldown จากรายงานอะไร? (Funnel / Cohort / Financial / Conversion / Retention)"
- "เดือนหรือสัปดาห์ที่ต้องการ drilldown?"
- รับ drill_key และ drill_type จากผู้ใช้ หรืออธิบายให้ผู้ใช้เลือก

เรียก tool ที่ตรงกัน:
- Funnel → **getFunnelDrilldown** (cohort_month, stage_code)
- Cohort → **getCohortDrilldown** (cohort_month, column_key)
- Financial → **getFinancialKeyMetricsDrilldown**
- Conversion → **getConversionKeyMetricsDrilldown**
- Retention → **getRetentionKeyMetricsDrilldown**
- Success Call → **getSuccessCallPerformanceDrilldown**

## Step 3: แสดงผล

### สำหรับ Management (ตอบสั้น ชัด มีตัวเลขเด่น)
```
📊 Funnel Report — ม.ค.–มิ.ย. 2026 (HumanSoft · All Size · ฐาน Register)

เดือนที่ดีที่สุด: เม.ย. 2026 — Register 245 domain, Paid 38 ราย (15.5%)
เดือนที่ต่ำสุด: ม.ค. 2026 — Register 180 domain, Paid 22 ราย (12.2%)
เฉลี่ย 6 เดือน: Paid Rate 13.8%

[ตารางสรุป]
| เดือน | Register | Paid | Paid Rate |
...

💡 ข้อสังเกต: Q2 มี conversion สูงกว่า Q1 ประมาณ 2.5% น่าติดตาม Success Call ในช่วงนั้น
```

### สำหรับ Analytics Team (ตารางเต็ม + แนะนำ drilldown)
- แสดงข้อมูลครบทุก stage / column
- แนะนำ: "ถ้าต้องการดูรายชื่อ domain เบื้องหลังตัวเลขไหน บอกเดือนและ stage ได้เลยครับ"
- เสนอ export: "ต้องการ list รายชื่อ domain สำหรับ Excel ไหมครับ?"

## หมายเหตุ

- ข้อมูลทั้งหมดดึงจาก ACP production real-time
- Sizing S = 1–75 คน (รวม domain ที่ไม่ระบุขนาด), M = 76–200 คน, L = 201+ คน
- S + M + L = All Size เสมอ
- ปีงบประมาณไทย: ปี 2569 = ปี 2026
