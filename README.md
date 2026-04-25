# 🏨 Azure Stay Hotel การเพิ่มประสิทธิภาพรายได้โรงแรม

## Problem 2: ต้นทุนการกระจายสินค้าสูง (High Distribution Costs)

> **กลุ่ม:** 150, 239, 569
> 
> **รายวิชา:** CP372 Data Analytics and Business Intelligence
> 
> **ชุดข้อมูล:** ข้อมูลสังเคราะห์โดย AI

---

# 0. Data Analytics Project Canvas

<img width="1920" height="1080" alt="Project Canvas pptx" src="https://github.com/user-attachments/assets/6067512e-00ba-4672-a4e1-63f8137b8606" />

---

# Section 1: Strategic Alignment

## 1. Background & Pain Points

แม้ว่าโรงแรม Azure Stay จะมีอัตราการเข้าพัก (Occupancy Rate) อยู่ในระดับที่ดี แต่พบว่า Revenue Per Available Room (RevPAR) ยังคงต่ำกว่าคู่แข่งในตลาด ซึ่งสะท้อนถึงปัญหาในด้านกลยุทธ์การตั้งราคาและการบริหารช่องทางการขาย

ปัญหาหลักเกิดจากการใช้ราคา (Promotion Rate) มากเกินไป โดยเฉพาะในช่วงที่มีความต้องการสูง (High Demand) แทนที่จะขายในราคาปกติ (Rack Rate) เพื่อเพิ่มรายได้สูงสุด

นอกจากนี้ โรงแรมยังขาดการใช้ข้อมูลในการตัดสินใจเชิงกลยุทธ์ เช่น การเลือกขายห้องผ่านช่องทางที่เหมาะสม หรือการตั้งราคาตามช่วงเวลาและพฤติกรรมลูกค้า ส่งผลให้เกิดการสูญเสียรายได้ (Revenue Leakage)

### ผลกระทบทางธุรกิจ:

* ADR และ RevPAR ต่ำกว่าศักยภาพ
* สูญเสียโอกาสในการสร้างรายได้
* กลยุทธ์ Revenue Management ไม่มีประสิทธิภาพ

---

## 2. วัตถุประสงค์แบบ SMART

| ตัวอักษร       | วัตถุประสงค์                                                                                                                                                                                         |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| S (Specific)   | วิเคราะห์และระบุปัจจัยที่ส่งผลต่อการลดลงของ ADR โดยใช้ข้อมูล Booking Channel และ Rate Code เพื่อค้นหาสัดส่วนการใช้ Promotion ที่ไม่เหมาะสม โดยเฉพาะในช่วง High Demand และปรับ Channel Mix ให้เหมาะสม |
| M (Measurable) | ลด Non-Rack Rate จาก 78.9% เหลือไม่เกิน 65% ในช่วง Weekend และเพิ่ม ADR โดยรวมอย่างน้อย 10% โดยมี Commission Loss Rate เป็นตัวชี้วัดเพิ่มเติม                                       |
| A (Achievable) | ใช้ข้อมูลการจองที่มีอยู่มาวิเคราะห์พฤติกรรมการตั้งราคา (Pricing) และ Channel Performance จากนั้นนำผลลัพธ์ไปปรับกลยุทธ์ เช่น จำกัด Promo และเพิ่ม Rack Rate                                           |
| R (Relevant)   | สอดคล้องกับเป้าหมายของ Azure Stay ในการเพิ่มรายได้ (Revenue) และลดการสูญเสียรายได้ (Revenue Leakage) จากการตั้งราคาที่ไม่มีประสิทธิภาพ                                                               |
| T (Time-bound) | ดำเนินการวิเคราะห์และปรับกลยุทธ์ให้เห็นผลภายใน 6 เดือน                                                                                                                                               |

---

# Section 2: Analytical Design

## 3. Hypothesis & Method

### Business Problem

ADR และ RevPAR ต่ำ เนื่องจากการตั้งราคาและเลือก Channel ไม่มีประสิทธิภาพ

### Business Goal

เพิ่ม ADR 10% ภายใน 6 เดือน

### Key Metrics
```
* ADR = Total Room Revenue / Number of Rooms Sold
* Net ADR = Total Room Revenue x (1 - Commission Rate) / Number of Rooms Sold
* Net Revenue = Total Room Revenue x (1 - Commission Rate)
* RevPAR = Total Room Revenue / Total Room Available
* gross_revenue = total_room_revenue

!!!! ถ้าเป็น Rate ต้องนำค่ามา หาร 100
```

### Dimensions

* Booking Channel
* Rate Code (Promo / Rack)
* Demand Level (High / Low)
* Lead Time

### Hypotheses

1. ขาย Promo มากเกินไปแม้ในวัน High Demand การจองช่วงวันหยุดสุดสัปดาห์ (Fri–Sat) ยังเป็น Non-Rack Rate ถึง 31.2% ทั้งที่เป็นช่วงที่ความต้องการสูง ส่งผลให้ ADR ต่ำกว่า Rack Rate เฉลี่ย 12.3%
2. สัดส่วน Rack Rate ต่ำกว่าที่ควร การจองทั้งหมดมีเพียง 21.1% ที่เป็น Rack Rate ขณะที่ Non-Rack Rate คิดเป็น 78.9% แสดงว่าโรงแรมปล่อยส่วนลดมากเกินความจำเป็น
3. OTA ดึงรายได้หายไป 8.7% OTA คิดเป็น 33.3% ของการจองทั้งหมด แต่เสียค่า Commission เฉลี่ย 17.6% ต่อการจอง ทำให้สูญเสียรายได้รวมถึง 8.7% ของ Gross Revenue ทั้งหมด
4. ไม่ได้ประโยชน์จากลูกค้าจองกระชั้นลูกค้าที่จองล่วงหน้าน้อยกว่า 7 วัน ให้ ADR ต่ำกว่าลูกค้าที่จองล่วงหน้ามากกว่า 10% ทั้งที่ควรตั้งราคาสูงขึ้นเมื่อห้องเหลือน้อย

### Expected Outcome

* ลดการใช้ Promotion ในช่วง High Demand
* เพิ่ม ADR และ Net Revenue
* ปรับกลยุทธ์ Channel ให้มีประสิทธิภาพ

### Business Actions

* จำกัดการใช้ Promotion ในช่วง Peak โดยเปิดขายเฉพาะ Rack Rate, Non-Refundable และ Corporate Negotiated ในวันศุกร์–เสาร์
* Shift OTA Booking → Direct Channel ด้วย Best Rate Guarantee และการลงทุนใน Google Hotel Ads เพื่อลด Commission Loss
* ตั้ง Last-Minute Rate Rule เมื่อ Booking Window ≤7 วัน ให้ปิด Discount Rate อัตโนมัติ และขึ้นราคา Rack Rate 5–10%

---

## 4. Diagram (Data to Decision Flow)

```id="s3c7yp"
Raw Data
   ↓
Data Cleaning
   ↓
Feature Engineering (Lead Time, Demand Level)
   ↓
Analysis (ADR, Channel, Promo)
   ↓
Insights
   ↓
Business Actions
   ↓
Revenue Improvement
```

---

## Section 3: Data Execution & EDA

### 5. AI Data Quality

โปรเจคนี้ใช้โครงสร้างแบบ Star Schema โดยมีความ fact เป็นศูนย์กลาง และ dimension tables สำหรับการวิเคราะห์ข้อมูลโรงแรม

### 5.1 Entity Relationship Overview

ระบบฐานข้อมูลประกอบด้วย:
- **Fact Table**: `fact_bookings` - ข้อมูลการจองหลัก
- **Dimension Tables**: ข้อมูลอ้างอิงสำหรับการวิเคราะห์

### 5.2 Fact Table

#### fact_bookings
ตารางข้อมูลหลักที่เก็บข้อมูลการจองแต่ละรายการ (5,322 แถว)

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| booking_id | Nominal (String) | รหัสการจองที่ไม่ซ้ำกัน (Primary Key) | RES-00001 |
| booking_date | Interval (Date) | วันที่ทำการจอง | 2025-08-22 |
| check_in_date | Interval (Date) | วันที่เช็คอิน | 2025-09-01 |
| check_out_date | Interval (Date) | วันที่เช็คเอาท์ | 2025-09-09 |
| room_type_id | Nominal (String) | Foreign key → dim_room_types | RT_DLX_KG |
| channel_id | Nominal (String) | Foreign key → dim_channels | CH_WEB |
| rate_code_id | Nominal (String) | Foreign key → dim_rate_codes | RC_RACK |
| segment_id | Nominal (String) | ประเภทลูกค้า | Business |
| number_of_rooms | Ratio (Continuous) | จำนวนห้องที่จอง | 1.0 |
| adults_count | Ratio (Continuous) | จำนวนผู้ใหญ่ | 2.0 |
| children_count | Ratio (Continuous) | จำนวนเด็ก | 1.0 |
| LOS_nights | Ratio (Continuous) | จำนวนคืนที่พัก (Length of Stay) | 8.0 |
| BLT_days | Ratio (Continuous) | จำนวนวันล่วงหน้าที่จอง (Book Lead Time) | 10.0 |
| status | Nominal (String) | สถานะการจอง | Confirmed |

### 5.3 Dimension Tables

#### dim_calendar
ตารางข้อมูลวันที่สำหรับการวิเคราะห์ตามเวลา

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| date_key | Interval (Date) | วันที่ (Primary Key) | 2025-01-12 |
| day_name | Nominal (String) | ชื่อวัน | Sunday |
| month_name | Nominal (String) | ชื่อเดือน | January |
| quarter | Nominal (String) | ไตรมาส | Q1 |
| is_weekend | Nominal (Boolean) | วันหยุดสุดสัปดาห์ | true |
| is_holiday | Nominal (Boolean) | วันหยุดนักขัตฤกษ์ | false |
| season | Nominal (String) | ฤดูกาล | Low |

#### dim_channels
ตารางข้อมูลช่องทางการจอง

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| channel_id | Nominal (String) | รหัสช่องทาง (Primary Key) | CH_WEB |
| channel_name | Nominal (String) | ชื่อช่องทาง | Direct Website |
| channel_type | Nominal (String) | ประเภทช่องทาง | Direct, OTA |
| commission_rate | Ratio (Continuous) | อัตราค่าคอมมิชชั่น | 0.0 |

#### dim_rate_codes
ตารางข้อมูลรหัสอัตรา

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| rate_code_id | Nominal (String) | รหัสอัตรา (Primary Key) | RC_RACK |
| rate_name | Nominal (String) | ชื่ออัตรา | Rack Rate |
| discount_factor | Ratio (Continuous) | ตัวคูณส่วนลด | 1.0 |
| is_rack | Nominal (Boolean) | เป็นอัตราพื้นฐานหรือไม่ | true |
| is_commissionable | Nominal (Boolean) | มีค่าคอมมิชชั่นหรือไม่ | true |
| description | Nominal (String) | รายละเอียดอัตรา | Standard flexible rate |

#### dim_room_types
ตารางข้อมูลประเภทห้อง

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| room_type_id | Nominal (String) | รหัสประเภทห้อง (Primary Key) | RT_DLX_KG |
| room_type_name | Nominal (String) | ชื่อประเภทห้อง | Deluxe King |
| base_rate_thb | Ratio (Continuous) | ราคาพื้นฐานต่อคืน (THB) | 170.0 |
| bed_type | Nominal (String) | ประเภทเตียง | King |
| max_occupancy | Ratio (Continuous) | จำนวนผู้เข้าพักสูงสุด | 3.0 |
| floor_range | Nominal (String) | ช่วงชั้นห้อง | 4–6 |

#### dim_room_inventory
ตารางข้อมูลสถานะห้องพัก

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| date | Interval (Date) | วันที่ (Primary Key) | 2025-01-12 |
| total_capacity | Ratio (Continuous) | จำนวนห้องทั้งหมด | 120.0 |
| rooms_currently_using | Ratio (Continuous) | ห้องที่ใช้งานอยู่ | 0.0 |
| rooms_out_of_order | Ratio (Continuous) | ห้องที่ปิดซ่อม | 6.0 |
| rooms_available_for_sale | Ratio (Continuous) | ห้องที่พร้อมขาย | 114.0 |

#### dim_segments
ตารางข้อมูลประเภทลูกค้า

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| segment_id | Nominal (String) | รหัสประเภทลูกค้า (Primary Key) | Business |
| segment_name | Nominal (String) | ชื่อประเภทลูกค้า | Business Traveller |
| segment_category | Nominal (String) | หมวดหมู่ลูกค้า | Corporate |
| is_contracted | Nominal (Boolean) | มีสัญญาหรือไม่ | true |
| price_sensitivity | Nominal (String) | ความไวต่อราคา | High |
| volume_potential | Nominal (String) | ศักยภาพปริมาณการจอง | Low |
| typical_channel | Nominal (String) | ช่องทางการจองทั่วไป | Corporate rate / Direct |

### 5.4 Data Quality Assessment

#### Missing Values Analysis
- **fact_bookings**: มีค่าว่างในคอลัมน์ `segment_id` บางแถว
- **dim_calendar**: ข้อมูลครบถ้วน ไม่มีค่าว่าง
- **dimension tables**: ข้อมูลครบถ้วนในทุกตาราง

#### Data Integrity Checks
- **Primary Keys**: ทุก Primary Key มีค่าไม่ซ้ำกัน
- **Foreign Keys**: ความสัมพันธ์ระหว่างตารางถูกต้อง
- **Date Ranges**: ข้อมูลวันที่อยู่ในช่วง 2025-2026 ตามที่คาดหวัง

### 5.5 Business Rules Validation
- **Check-in/Check-out Logic**: วันเช็คเอาท์มาหลังวันเช็คอิน
- **Booking Lead Time**: คำนวณจากวันจองถึงวันเช็คอิน
- **Length of Stay**: คำนวณจากวันเช็คอินถึงวันเช็คเอาท์
- **Room Capacity**: จำนวนผู้เข้าพักไม่เกิน max_occupancy ของประเภทห้อง

 ### 5.6 Prompt ที่ใช้สร้างชุดข้อมูล
สร้างชุดข้อมูลการจองโรงแรมที่สมจริงสำหรับโรงแรม 4 ดาวชื่อ "Azure Stay"  
ข้อมูลต้องเป็นไปตาม schema นี้อย่างเคร่งครัด  และต้องสอดคล้องกันภายใน (Foreign Key ต้องตรงกัน การคำนวณทางการเงินต้องถูกต้อง) 5300 Rows:

**TABLE 1: fact_bookings:** 

booking_id (PK): RES-00001, RES-00002, ...

booking_date: วันที่จอง (2025)

check_in_date: วันที่ check-in (2025)

check_out_date: วันที่ check-out (2025)

room_type_id (FK): RT_DLX_KG, RT_STD_QN, RT_SUIT, RT_OCEAN, RT_ACC

channel_id (FK): CH_WEB, CH_EXP, CH_BKG, CH_WALK, CH_GDS, CH_CORP

rate_code_id (FK): RC_RACK, RC_AAA, RC_NRF, RC_SEAS, RC_CORP

segment_id: Business, Leisure, Wholesale, Transient, Group

number_of_rooms: 1, 2, 3

adults_count: 1–5

children_count: 0–4

LOS_nights: 2–14 คืน

BLT_days: 4–49 วัน (จองล่วงหน้า)

status: Confirmed, Checked-Out, Cancelled, No-Show

**TABLE 2: dim_calendar:** 

date_key (PK): วันที่ทุกวัน(ปี 2025)

day_name: Monday–Sunday

month_name: January–December

quarter: Q1, Q2, Q3, Q4

is_weekend: True / False

is_holiday: False (ทั้งหมด)

season: Low, Shoulder, High

**TABLE 3: dim_channels**

channel_id (PK): CH_BKG, CH_CORP, CH_EXP, CH_GDS, CH_WALK, CH_WEB

channel_name: Booking.com, Corporate Agent, Expedia, GDS, Walk-in, Website

channel_type: OTA, Wholesale, GDS, Direct

commission_rate: 0% – 18%

**TABLE 4: dim_rate_codes**

rate_code_id (PK): RC_RACK, RC_AAA, RC_NRF, RC_SEAS, RC_CORP

rate_name: Rack Rate, AAA Discount, Non-Refundable, Seasonal Promo, Corporate Negotiated

discount_factor: 0.80 – 1.00

is_rack: True / False

is_commissionable: True (ทั้งหมด)

description: คำอธิบาย rate แต่ละประเภท

**TABLE 5: dim_room_inventory**

date (PK): วันที่รายวัน

total_capacity: 120 (คงที่ทุกวัน)

rooms_currently_using: 0 (คงที่)

rooms_out_of_order: 0–6 ห้อง

rooms_available_for_sale: 114–120 ห้อง

**TABLE 6: dim_room_types**

room_type_id (PK): RT_ACC, RT_DLX_KG, RT_OCEAN, RT_STD_QN, RT_SUIT

room_type_name: Accessible, Deluxe King, Ocean View, Standard Queen, Suite

base_rate_thb: 120 – 260 (หน่วย: พัน THB)

bed_type: Queen, King

max_occupancy: 2–6 คน

floor_range: ชั้น 1, 1–3, 4–6, 5–7, 7

**TABLE 7: dim_segments**

segment_id (PK): Business, Leisure, Wholesale, Transient, Group

segment_name: Business Traveller, Leisure Traveller, Wholesale/Agent, Transient, Group Booking

segment_category: Corporate, Retail, Wholesale, Group

is_contracted: True / False

price_sensitivity: Low, Medium, High

volume_potential: Low, Medium, High

typical_channel: OTA, Direct, GDS, Walk-in, Corporate Agent

---

## 6. EDA & Visualizations
### Data Overview
"Rows (จำนวนแถว) : 5,318
Columns (จำนวนคอลัมน์) : 34"
### Data Quality Check

| # | Step | Description | Column Stats |
|---|------|-------------|--------------|
| 1 | **Changed Type** | ปรับประเภทข้อมูลของแต่ละคอลัมน์ให้ถูกต้อง — ตัวเลข (Number), ข้อความ (Text) หรือวันที่ (Date) เพื่อให้ระบบคำนวณได้ถูกต้อง | **`total_room_revenue`** <br>Mean = 555.47 \| Max = 1,998.36 \| Min = -1,836.00 \| Median = 504.00 |
| 2 | **Removed `guest_id`** | ลบคอลัมน์ไอดีของลูกค้าออก เนื่องจากไม่ได้ใช้ในการวิเคราะห์ และเพื่อปกป้องความเป็นส่วนตัวของข้อมูล | **`base_price`** <br>Mean = 188.57 \| Max = 260.00 \| Min = 120.00 \| Median = 170.00 |
| 3 | **Replaced null → 0 (`children_count`)** | เปลี่ยนค่าว่าง (null) ในคอลัมน์จำนวนเด็กเป็น 0 เพื่อป้องกัน error เวลานำไปคำนวณ | **`commission_rate`** <br>Mean = 0.09 \| Max = 0.18 \| Min = 0.00 \| Median = 0.10 |
| 4 | **Removed negative LOS** | กรองแถวที่มีค่า Length of Stay ติดลบออก เนื่องจากเป็นข้อมูลที่ผิดพลาด (วัน check-out มาก่อนวัน check-in) ส่งผลให้ค่า Min ของ `total_room_revenue`, `ADR`, `net_revenue`, `net_ADR` ที่ติดลบหายไปหลัง clean | **`BLT_days`** <br>Mean = 21.45 \| Max = 53.00 \| Min = 0.00 \| Median = 21.00 |
| 5 | **Changed `total_room_revenue`** | ปรับ format คอลัมน์รายได้ห้องพักให้เป็นทศนิยม 2 ตำแหน่ง และคำนวณใหม่จากสูตร `base_price × discount_factor × LOS_nights × number_of_rooms` | **`LOS_nights`** <br>Mean = 3.45 \| Max = 14.00 \| Min = 1.00 \| Median = 3.00 |
| 6 | **Sorted rows by `booking_id`** | จัดเรียงข้อมูลจาก `RES-00001` ขึ้นไปตามลำดับ เพื่อให้อ่านและตรวจสอบข้อมูลได้ง่าย | **`discount_factor`** <br>Mean = 0.89 \| Max = 1.00 \| Min = 0.80 \| Median = 0.88 |
| 7 | **Trimmed / Cleaned Text** | ลบช่องว่างส่วนเกินที่หัว/ท้ายข้อความ และลบ non-printable characters เพื่อป้องกันปัญหาเวลาใช้ filter หรือ VLOOKUP | **`number_of_rooms`** <br>Mean = 1.20 \| Max = 3.00 \| Min = 1.00 \| Median = 1.00 |
| 8 | **Removed `guest_id`** (confirmed) | ยืนยันการลบคอลัมน์ `guest_id` ออกจาก dataset สุดท้าย | **`adults_count`** <br>Mean = 1.80 \| Max = 6.00 \| Min = 1.00 \| Median = 2.00 |
| 9 | **Feature Engineering** | สร้างคอลัมน์ใหม่ 3 ตัว: `day_of_week` / `is_weekend` (True ถ้า Sat–Sun) / `is_rack` (True ถ้า `rate_code_id = RC_RACK`) | **`children_count`** <br>Mean = 0.51 \| Max = 6.00 \| Min = 0.00 \| Median = 0.00 |
| 10 | **KPI Calculation** | คำนวณ KPI หลัก: `ADR = total_room_revenue / LOS_nights / number_of_rooms` และ `net_ADR = net_revenue / LOS_nights / number_of_rooms` | **`ADR`** <br>Mean = 160.00 \| Max = 622.20 \| Min = -306.00 \| Median = 168.00 <br><br> **`net_revenue`** <br>Mean = 502.96 \| Max = 1,638.66 \| Min = -1,836.00 \| Median = 456.10 <br><br> **`net_ADR`** <br>Mean = 144.91 \| Max = 558.27 \| Min = -306.00 \| Median = 149.60 |
| 11 | **Lead Time Binning** | จัดกลุ่ม `BLT_days` เป็น 4 ช่วง (`lead_bin`) เพื่อวิเคราะห์พฤติกรรมการจอง: `0–7 days` / `8–14 days` / `15–30 days` / `31+ days` | **`BLT_days`** <br>Mean = 21.45 \| Max = 53.00 \| Min = 0.00 \| Median = 21.00 |

> **Note:** ค่า Min ที่ติดลบใน `total_room_revenue`, `ADR`, `net_revenue` และ `net_ADR` คือค่าก่อนทำ Step 4 (Removed negative LOS) — หลังจาก clean แล้วค่าเหล่านี้จะถูกกำจัดออก
---
## CELL 1 : Hypotheses 1 — ขาย Promo มากเกินไปในวัน High Demand
**กราฟ** : Grouped Bar Chart — สัดส่วน Rack Rate vs Non-Rack Rate แยกตามวันในสัปดาห์ (เรียงจาก Mon → Sun)
• เปรียบเทียบสัดส่วนการขาย Rack Rate และ Non-Rack Rate
    ในแต่ละวันของสัปดาห์
• วันศุกร์–เสาร์ (High Demand) ควรมีสัดส่วน Rack Rate สูงกว่าวันอื่น
    แต่ถ้ายังมี Non-Rack Rate ~30% แสดงว่าโรงแรมปล่อยโปรโมชัน
    มากเกินความจำเป็นในช่วงที่ควรเก็บราคาเต็ม

**เหตุผลที่เลือก** : Grouped Bar เหมาะสมที่สุดสำหรับการเปรียบเทียบ 2 หมวดหมู่
  (Rack vs Non-Rack) ข้าม 7 วัน — ทำให้เห็นความแตกต่างของแต่ละวัน
  ได้ในมุมมองเดียว สื่อสารได้ชัดเจนกว่า Pie Chart หรือ Stacked Bar
<img width="1187" height="586" alt="image" src="https://github.com/user-attachments/assets/c04c13aa-6b58-48a5-adf0-a01d8aa819ae" />

## CELL 2 : Hypotheses 2 — สัดส่วน Rack Rate โดยรวมต่ำกว่าที่ควร
**กราฟ :** Donut Chart (Pie with hole) + Bar Chart เปรียบ ADR
• Donut Chart แสดงสัดส่วน Rack Rate ต่อ Non-Rack Rate ทั้งหมด
    — หาก Non-Rack Rate เกิน 60% แสดงว่าโรงแรมปล่อยส่วนลดมากเกินไป
• Bar Chart เปรียบ ADR เฉลี่ยของแต่ละ Rate Type
    — ช่วยประเมินมูลค่าที่สูญเสียต่อห้องต่อคืน

**เหตุผลที่เลือก :** Donut เหมาะสำหรับแสดง Part-to-Whole (ส่วนแบ่งตลาด)
  ดีกว่า Pie ธรรมดาเพราะอ่านสัดส่วนได้ง่ายกว่า
  จับคู่กับ Bar Chart เพื่อเชื่อม "ปริมาณ" กับ "มูลค่า" ในสไลด์เดียว
<img width="1223" height="590" alt="image" src="https://github.com/user-attachments/assets/33bc8c4e-9e9e-435f-9dc5-3740605ca03d" />

## CELL 3 : Hypotheses 3 — OTA ดึงรายได้หายไป 8.7%
**กราฟ :** Grouped Bar Chart — Gross ADR vs Net ADR แต่ละ Channel พร้อมเส้นแสดง Commission Rate
• แสดง ADR ก่อน (Gross) และหลัง (Net) หัก Commission ทุก Channel
• ช่อง OTA เช่น Booking.com / Expedia มักมี Gross ADR สูง
    แต่พอหัก Commission ~15-18% แล้ว Net ADR อาจต่ำกว่า Direct
• ช่วยให้เห็น "ต้นทุนที่ซ่อนอยู่" ของแต่ละ Distribution Channel

**เหตุผลที่เลือก :** Grouped Bar + Secondary Axis (Commission %) ทำให้เห็นทั้ง "มูลค่า"
  และ "ต้นทุน" ในกราฟเดียว — เหมาะสำหรับ Revenue Manager ที่ต้อง
  เปรียบเทียบ Channel Profitability อย่างรวดเร็ว
<img width="1186" height="588" alt="image" src="https://github.com/user-attachments/assets/b3d80c52-cd1c-4b71-a0d4-1de1aaa0c400" />

## CELL 4 : Hypotheses 4 — ไม่ได้ประโยชน์จากลูกค้าจองกระชั้น
**กราฟ:** Bar Chart + Scatter Plot — ADR เฉลี่ยตามกลุ่ม Lead Time + แสดง Volume ประกอบ
• ลูกค้าที่จองล่วงหน้า 0–7 วัน (Last-Minute) ควรจ่ายราคาสูงกว่า เพราะห้องเหลือน้อยและ Demand สูง — แต่ถ้า ADR กลับต่ำกว่า แสดงว่าระบบ Pricing ยังไม่ได้ใช้ Last-Minute Premium • Volume ของแต่ละ Segment 
ช่วยบอกว่า Last-Minute มีสัดส่วนมากน้อยแค่ไหน

**เหตุผลที่เลือก :** Bar เรียงตาม Lead Time ทำให้เห็น Pricing Curve — ควรเห็นว่า ยิ่งจองกระชั้นยิ่งราคาสูง (Inverse Relationship ตาม Revenue Management Theory) 
ถ้า Curve ไม่เป็นแบบนั้น คือ Pricing Opportunity ที่ยังไม่ได้ใช้
<img width="989" height="590" alt="image" src="https://github.com/user-attachments/assets/288c7369-6b6a-4384-9f5a-b7b1ed49983c" />


---

# Section 4: Insights & Impact

## 7. Findings (Insights)

### 1. Peak Demand ยังถูกกัดด้วย Non-Rack Rate
วันหยุดขายโปรโมชันเยอะเกินไป Weekend ที่ควรขายราคาเต็ม แต่ยังมีส่วนลดอยู่ถึง 79.6% ทั้งที่ต่างจากวันธรรมดา (78.6%) แทบไม่เลย ถ้าปิดโปรโมชันวันศุกร์–เสาร์ได้ ADR จะเพิ่มขึ้นทันที ฿23/คืน

### 2. Non-Rack Rate มากเกินไป
ราคาเต็มขายได้แค่ 21% ของทั้งโรงแรม ไม่ใช่แค่วันหยุด แต่ทุกวันทั้งโรงแรมปล่อยส่วนลดมากเกินไป Rack Rate มีแค่ 21.1% Non-Rack คิดเป็น 78.9% ของยอดจองทั้งหมด

### 3. OTA สร้าง Volume แต่ Net ADR ต่ำกว่า Direct ฿28/คืน OTA คิดเป็น 33.8%
OTA ช่วยให้มีคนจองเยอะ แต่รายได้จริงหายไป ฿175,424/ปี OTA คิดเป็น 33.8% ของการจองทั้งหมด แต่พอหัก Commission 17.5% ออกแล้ว ได้เงินจริงต่ำกว่าจอง Direct ถึง ฿24/คืน

### 4. Last-Minute Pricing
ยิ่งจองกระชั้น ยิ่งได้ราคาถูก — ซึ่งผิดปกติคนจองกระชั้นภายใน 7 วัน ได้ ADR ฿152.8 ถูกกว่าคนจองล่วงหน้า 31+ วัน (฿165.5) อยู่ ฿12.7/คืน ทั้งที่ห้องเหลือน้อย ราคาควรแพงขึ้น ไม่ใช่ถูกลง

---

## 8. Recommendations

### 1.ปิด Promo วัน Fri–Sat
ปิดโปรโมชันวันศุกร์–เสาร์ เปิดขายเฉพาะราคาเต็ม (Rack) และ Non-Refundable แทน ไม่ต้องลด AAA หรือ Seasonal Promo ในวันที่คนอยากจองอยู่แล้ว ได้คืน ADR ฿23/คืน ทันทีและ Non-Rack ลดจาก 78.9% --> 72.0%

### 2.ดัน Direct ลด OTA
ดึงลูกค้าให้จอง Direct แทน OTA ทำ Best Rate Guarantee + ลงทุน Google Hotel Ads เพื่อให้คนจองตรงกับโรงแรมแทนการจองผ่าน OTA เป้าหมาย Shift 10% ภายใน 6 เดือน ประหยัด Commission ได้ ฿17,500+/ปี

### 3.Last-Minute Premium Pricing
ตั้งราคาให้แพงขึ้นเมื่อห้องเหลือน้อย ปิดส่วนลดอัตโนมัติเมื่อ Booking Window ≤7 วัน หรือ Occupancy เกิน 70% เป้าหมายดัน ADR Last-minute จาก ฿152.8 → ฿170+ คือให้แพงกว่าคนจองล่วงหน้าด้วยซ้ำ

### 4.Cancellation Policy แบบ Tiered
ปรับ Cancellation Policy ตามระยะเวลาที่จอง จองไกล 30+ วัน → Non-Refundable แต่ลด 5% เป็นแรงจูงใจ จอง 8–14 วัน → Refund ได้ 50% ถ้า Cancel ก่อน 72 ชม.
จองกระชั้น ≤7 วัน → Non-Refundable 100% เสมอ เป้าหมายเพิ่ม Non-Refundable จาก 19.7% → 35% เพื่อให้รายได้มั่นคงขึ้น

---


# 9. โครงสร้างโปรเจกต์

```


Project-CP372-Azure-Stay/
│
├── data/
│   ├── dim_calendar.xlsx
│   ├── dim_channels.xlsx
│   ├── dim_hotel_derived_features.csv
│   └── dim_rate_codes.xlsx
│   └── dim_room_inventory.xlsx
│   └── dim_room_types.xlsx
│   └── hotel_Finished_dataset.xlsx
│   └── hotel_processed_data.csv.xlsx
│  
├── notebook/
│   └── Project_Azure_Stay.ipynb
│
├── images/
│   ├── Bar Chart + Scatter Plot.png
│   ├── Bubble Chart.png
│   ├── Donut Chart.png
│   ├── Grouped Bar Chart 2.png
│   ├── Grouped Bar Chart.png
│
│
└── README.md
```

---

# 10. วิธีรันโปรแกรม

## Google Colab (แนะนำ)
1. ไปที่ https://github.com/ThanaphonUnsap/Project-CP372-Azure-Stay/tree/main
2. และไปที่ data/Hotel_Azure_Stay_Dataset_Finished ดาวน์โหลด
3. ไปที่ notebook/Project_Azure_Stay_Finished ดาวน์โหลด
4. เข้า Colab แล้วทำการลากไฟล์ใส่เข้าไปใน
5. รันทุกเซลล์ตามลำดับ
