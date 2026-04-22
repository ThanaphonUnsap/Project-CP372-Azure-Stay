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

ปัญหาหลักเกิดจากการใช้ราคาส่งเสริมการขาย (Promotion Rate) มากเกินไป โดยเฉพาะในช่วงที่มีความต้องการสูง (High Demand) แทนที่จะขายในราคาปกติ (Rack Rate) เพื่อเพิ่มรายได้สูงสุด

นอกจากนี้ โรงแรมยังขาดการใช้ข้อมูลในการตัดสินใจเชิงกลยุทธ์ เช่น การเลือกขายห้องผ่านช่องทางที่เหมาะสม หรือการตั้งราคาตามช่วงเวลาและพฤติกรรมลูกค้า ส่งผลให้เกิดการสูญเสียรายได้ (Revenue Leakage)

### ปัญหาหลัก:

Pain Point 1: ขาย Promo มากเกินไปแม้ในวัน High Demand
การจองช่วงวันหยุดสุดสัปดาห์ (Fri–Sat) ยังเป็น Non-Rack Rate ถึง 31.2% ทั้งที่เป็นช่วงที่ความต้องการสูง ส่งผลให้ ADR ต่ำกว่า Rack Rate เฉลี่ย 12.3%

Pain Point 2: สัดส่วน Rack Rate ต่ำกว่าที่ควร
การจองทั้งหมดมีเพียง 38.1% ที่เป็น Rack Rate ขณะที่ Non-Rack Rate คิดเป็น 61.9% แสดงว่าโรงแรมปล่อยส่วนลดมากเกินความจำเป็น

Pain Point 3: OTA ดึงรายได้หายไป 8.7%
OTA คิดเป็น 33.3% ของการจองทั้งหมด แต่เสียค่า Commission เฉลี่ย 17.6% ต่อการจอง ทำให้สูญเสียรายได้รวมถึง 8.7% ของ Gross Revenue ทั้งหมด

Pain Point 4: ไม่ได้ประโยชน์จากลูกค้าจองกระชั้น
ลูกค้าที่จองล่วงหน้าน้อยกว่า 7 วัน ให้ ADR ต่ำกว่าลูกค้าที่จองล่วงหน้ามากกว่า 10% ทั้งที่ควรตั้งราคาสูงขึ้นเมื่อห้องเหลือน้อย

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

* ADR = รายได้รวม / จำนวนห้องที่ขายได้
* Net ADR = (รายได้รวม - ค่าคอมมิชชั่น) / จำนวนห้องที่ขายได้
* Net Revenue = รายได้รวม - ค่าคอมมิชชั่น
* Promotion Ratio = จำนวนการจองแบบ Promo / จำนวนการจองทั้งหมด
* Commission Loss Rate = ค่า Commission รวม / Gross Revenue รวม

### Dimensions

* Booking Channel
* Rate Code (Promo / Rack)
* Demand Level (High / Low)
* Lead Time

### Hypotheses

1. มีการใช้ Promotion มากเกินไปในช่วง High Demand
2. ช่องทาง Direct ให้ ADR สูงกว่า OTA
3. ลูกค้าที่จองใกล้วันเข้าพัก (Last-Minute) ควรจ่ายแพงกว่า แต่ปัจจุบันยังไม่เป็นเช่นนั้น เนื่องจากขาด Dynamic Pricing Rule ที่เหมาะสม

### Analytical Approach

* แบ่งข้อมูลตาม Demand Level
* เปรียบเทียบ Promo กับ Rack Rate
* วิเคราะห์ ADR ตาม Channel
* วิเคราะห์ ADR ตาม Lead Time

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

# Section 3: Data Execution & EDA

## 5. AI Data Quality 

โปรเจคนี้ใช้โครงสร้างแบบ Star Schema โดยมีตาราง fact เป็นศูนย์กลาง และ dimension tables สำหรับการวิเคราะห์เชิงธุรกิจ

---

## 5.1 Entity Relationship Overview

```
dim_calendar ──────────────────────────────────────────────────┐
dim_channels ─────────────────────────────────────────────┐    │
dim_rate_codes ───────────────────────────────────────┐    │    │
dim_room_types ───────────────────────────────────┐    │    │    │
                                                  ▼    ▼    ▼    ▼
                                           ┌──────────────────────────┐
                                           │      fact_booking         │
                                           └──────────────────────────┘
                                                        │
                                                        ▼
                                           dim_hotel_derived_features
                                           dim_room_inventory (date-level)
```

---

## 5.2 Fact Table

### `fact_booking`

The central fact table containing one row per booking record (5,318 rows).

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| `booking_id` | Nominal (String) | Unique booking identifier **(Primary Key)** | `RES-00001` |
| `booking_date` | Interval (Date) | Date the booking was created | `2025-08-22` |
| `check_in_date` | Interval (Date) | Scheduled check-in date | `2025-09-01` |
| `check_out_date` | Interval (Date) | Scheduled check-out date | `2025-09-09` |
| `room_type_id` | Nominal (String) | Foreign key → `dim_room_types` | `RT_DLX_KG` |
| `room_type_name` | Nominal (String) | Human-readable room type name | `Deluxe King` |
| `base_price` | Ratio (Continuous) | Base nightly room price in USD before discount | `170` |
| `commission_rate` | Ratio (Continuous) | Channel commission rate (0–1) | `0.18` |
| `rate_code_id` | Nominal (String) | Foreign key → `dim_rate_codes` | `RC_RACK` |
| `rate_name` | Nominal (String) | Human-readable rate plan name | `Rack Rate` |
| `discount_factor` | Ratio (Continuous) | Discount multiplier applied to base price (0–1) | `1.0` |
| `channel_id` | Nominal (String) | Foreign key → `dim_channels` | `CH_WEB` |
| `channel_name` | Nominal (String) | Human-readable channel name | `Direct Website` |
| `channel_type` | Nominal (String) | Channel category | `Direct`, `OTA`, `GDS`, `Wholesale` |
| `segment_id` | Nominal (String) | Guest market segment | `Business`, `Leisure`, `Wholesale`, `Transient`, `Group` |
| `status` | Nominal (String) | Current booking status | `Confirmed`, `Checked-Out`, `Cancelled`, `No-Show` |
| `total_room_revenue` | Ratio (Continuous) | Total revenue for the booking in USD | `832.00` |
| `number_of_rooms` | Ratio (Discrete) | Number of rooms in the booking | `1`, `2`, `3` |
| `adults_count` | Ratio (Discrete) | Number of adult guests | `1`–`5` |
| `children_count` | Ratio (Discrete) | Number of child guests | `0`–`4` |
| `LOS_nights` | Ratio (Discrete) | Length of stay in nights | `2`–`14` |
| `BLT_days` | Ratio (Discrete) | Booking lead time in days (days between booking and check-in) | `0`–`49+` |
| `day_of_week` | Nominal (String) | Day of the week for check-in date | `Monday`–`Sunday` |
| `is_weekend` | Binary (Boolean) | Whether check-in falls on a weekend (Sat–Sun) | `True`, `False` |
| `is_rack` | Binary (Boolean) | Whether booking is at rack rate (`RC_RACK`) | `True`, `False` |
| `ADR` | Ratio (Continuous) | Average Daily Rate = `total_room_revenue / LOS_nights / number_of_rooms` | `208.00` |
| `net_revenue` | Ratio (Continuous) | Revenue after deducting channel commission | `682.24` |
| `net_ADR` | Ratio (Continuous) | Net ADR after commission | `170.56` |
| `lead_bin` | Ordinal (String) | Bucketed booking lead time | `0–7 days`, `8–14 days`, `15–30 days`, `31+ days` |

---

## 5.3 Dimension Tables

### `dim_room_types`

Lookup table for hotel room categories (5 rows).

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| `room_type_id` | Nominal (String) | Unique room type identifier **(Primary Key)** | `RT_DLX_KG` |
| `room_type_name` | Nominal (String) | Human-readable room type name | `Deluxe King` |
| `base_rate_usd` | Ratio (Continuous) | Base nightly rate in USD | `170` |

**Available room types:**

| `room_type_id` | `room_type_name` | `base_rate_usd` |
|----------------|------------------|-----------------|
| `RT_STD_QN` | Standard Queen | $120 |
| `RT_DLX_KG` | Deluxe King | $170 |
| `RT_SUIT` | Suite | $260 |
| `RT_OCEAN` | Ocean View | $210 |
| `RT_ACC` | Accessible | $130 |

---

### `dim_rate_codes`

Lookup table for rate plans and pricing strategies (5 rows).

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| `rate_code_id` | Nominal (String) | Unique rate code identifier **(Primary Key)** | `RC_RACK` |
| `rate_name` | Nominal (String) | Human-readable rate plan name | `Rack Rate` |
| `discount_factor` | Ratio (Continuous) | Price multiplier applied to base rate (0–1) | `0.85` |
| `is_commissionable` | Binary (Boolean) | Whether a channel commission is applied | `1` = Yes, `0` = No |
| `description` | Nominal (String) | Brief description of the rate plan | `Corporate negotiated rate.` |

**Available rate codes:**

| `rate_code_id` | `rate_name` | `discount_factor` | `is_commissionable` |
|----------------|-------------|-------------------|----------------------|
| `RC_RACK` | Rack Rate | 1.00 | Yes |
| `RC_AAA` | AAA Discount | 0.90 | Yes |
| `RC_CORP` | Corporate Negotiated | 0.85 | No |
| `RC_NRF` | Non-Refundable | 0.80 | Yes |
| `RC_SEAS` | Seasonal Promo | 0.88 | Yes |

---

### `dim_channels`
Lookup table for booking distribution channels (6 rows).

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| `channel_id` | Nominal (String) | Unique channel identifier **(Primary Key)** | `CH_WEB` |
| `channel_name` | Nominal (String) | Human-readable channel name | `Direct Website` |
| `channel_type` | Nominal (String) | Broad channel category | `Direct`, `OTA`, `GDS`, `Wholesale` |
| `commission_rate` | Ratio (Continuous) | Commission rate charged by this channel (0–1) | `0.18` |

**Available channels:**

| `channel_id` | `channel_name` | `channel_type` | `commission_rate` |
|--------------|----------------|----------------|-------------------|
| `CH_WEB` | Direct Website | Direct | 0.00 |
| `CH_WALK` | Walk-in | Direct | 0.00 |
| `CH_EXP` | Expedia | OTA | 0.18 |
| `CH_BKG` | Booking.com | OTA | 0.17 |
| `CH_GDS` | GDS | GDS | 0.12 |
| `CH_CORP` | Corporate Agent | Wholesale | 0.10 |

---

### `dim_calendar`

Date dimension covering the full analysis period (61 rows: Sep 1 – Oct 31, 2025).

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| `date_key` | Interval (Date) | Calendar date **(Primary Key)** | `2025-09-15` |
| `day_name` | Nominal (String) | Name of the day of the week | `Monday` |
| `is_weekend` | Binary (Boolean) | Whether the date is a Saturday or Sunday | `True`, `False` |
| `is_holiday` | Binary (Boolean) | Whether the date is a public holiday | `True`, `False` |
| `season` | Ordinal (String) | Demand season classification | `High`, `Shoulder` |

---

### `dim_room_inventory`

Daily room inventory table at the hotel level (61 rows: Sep 1 – Oct 31, 2025).

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| `date` | Interval (Date) | Calendar date **(Primary Key)** | `2025-09-01` |
| `total_capacity` | Ratio (Discrete) | Total number of rooms in the hotel | `120` |
| `rooms_out_of_order` | Ratio (Discrete) | Rooms that are closed for maintenance or unavailable for sale | `0`–`12` |
| `rooms_available_for_sale` | Ratio (Discrete) | Rooms ready to be sold = `total_capacity − rooms_out_of_order` | `116` |

---

## 5.4 Derived Feature Table

### `dim_hotel_derived_features`

Pre-computed feature table derived from `fact_booking`, used for analysis and modeling (5,318 rows — one row per booking).

| Column | Data Type | Description | Example |
|--------|-----------|-------------|---------|
| `booking_id` | Nominal (String) | Foreign key → `fact_booking` **(Primary Key)** | `RES-00001` |
| `day_of_week` | Nominal (String) | Day of the week for check-in date | `Monday` |
| `is_weekend` | Binary (Boolean) | Whether check-in falls on a weekend | `True`, `False` |
| `is_rack` | Binary (Boolean) | Whether the booking uses rack rate (`RC_RACK`) | `True`, `False` |
| `ADR` | Ratio (Continuous) | Average Daily Rate in USD | `208.00` |
| `net_revenue` | Ratio (Continuous) | Revenue after deducting channel commission | `682.24` |
| `net_ADR` | Ratio (Continuous) | Net ADR after commission deduction | `170.56` |
| `lead_bin` | Ordinal (String) | Bucketed booking lead time | `0–7 days`, `8–14 days`, `15–30 days`, `31+ days` |

---

## 5.5 Key Relationships

```
fact_booking.room_type_id   → dim_room_types.room_type_id
fact_booking.rate_code_id   → dim_rate_codes.rate_code_id
fact_booking.channel_id     → dim_channels.channel_id
fact_booking.check_in_date  → dim_calendar.date_key
fact_booking.check_in_date  → dim_room_inventory.date
fact_booking.booking_id     → dim_hotel_derived_features.booking_id
```

---

## 5.6 Derived / Calculated Columns

The following columns in `fact_booking` and `dim_hotel_derived_features` are **computed columns**, not raw source fields:

| Column | Formula |
|--------|---------|
| `total_room_revenue` | `base_price × discount_factor × LOS_nights × number_of_rooms` |
| `ADR` | `total_room_revenue / LOS_nights / number_of_rooms` |
| `net_revenue` | `total_room_revenue × (1 − commission_rate)` |
| `net_ADR` | `net_revenue / LOS_nights / number_of_rooms` |
| `is_rack` | `True` if `rate_code_id == 'RC_RACK'` else `False` |
| `rooms_available_for_sale` | `total_capacity − rooms_out_of_order` |
| `BLT_days` | `check_in_date − booking_date` (in days) |
| `lead_bin` | Binned from `BLT_days`: 0–7 / 8–14 / 15–30 / 31+ |

---

## 6. EDA & Visualizations
### Data Overview
"Rows: 5318 แถว
Columns: 29 คอลัมน์"
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

# Section 4: Insights & Impact

## 7. Findings (Insights)

### 1. Peak Demand ยังถูกกัดด้วย Non-Rack Rate
วันศุกร์–เสาร์มี Non-Rack Rate ~31% ซึ่งไม่ต่างจาก Weekday อย่างมีนัยสำคัญ แสดงว่า Rate Restriction Policy สำหรับ Peak Period ยังขาดอยู่ การปิดโปรโมชันในช่วงดังกล่าวสามารถเพิ่ม ADR ได้ทันทีโดยไม่กระทบ Occupancy

### 2. Non-Rack Rate คิดเป็น 61.9% ของยอดจอง — Revenue Dilution ระดับ Portfolio
ปัญหาไม่ใช่แค่วันหยุดสุดสัปดาห์ แต่เป็น Structural Issue ทั้ง Portfolio ควร Review Rate Availability ในทุก Segment และตั้ง Minimum Rack Rate Contribution เป้าหมายไว้ที่ ≥50%่

### 3. OTA สร้าง Volume แต่ Net ADR ต่ำกว่า Direct เฉลี่ย ~15–18%
Booking.com และ Expedia นำส่งยอดจองสูงสุด แต่ Net ADR หลังหัก Commission ต่ำกว่า Direct Web — ควรลงทุนใน Direct Channel (Loyalty Program, Website Rate Parity) เพื่อ Shift 10–15% ของ OTA Booking มาเป็น Direct

### 4. Last-Minute Pricing ไม่สะท้อน Scarcity — ADR ต่ำกว่าที่ควร
ลูกค้าที่จองใน 7 วันสุดท้ายได้ ADR ต่ำกว่ากลุ่มจองล่วงหน้า 8–14 วัน ซึ่งตรงข้ามกับ Dynamic Pricing Principle ควรตั้ง Last-Minute Rate Rule ที่ปรับราคาขึ้นอัตโนมัติเมื่อ Occupancy เกิน 70% และ Booking Window น้อยกว่า 7 วัน

### 5. Direct Web = High Value Channel ที่ยังถูก Underinvest
Bubble Chart จะแสดงว่า Direct Web มี Net ADR สูงที่สุดและ Commission เป็น 0% แต่ Volume ยังต่ำกว่า OTA — นี่คือโอกาสสำคัญที่สุดในการเพิ่ม Net RevPAR

---

## 8. Recommendations

### 1.ปิด Promo วัน Fri–Sat
Weekend มี Non-Rack Rate 79.6% ไม่ต่างจาก Weekday (78.6%) เลย → ตั้ง Rate Restriction ปิด Seasonal Promo & AAA Discount ทุกวันศุกร์–เสาร์ เปิดขายเฉพาะ Rack / Non-Refundable / Corporate แทน
ADR Gap Rack vs Non-Rack = ฿23/คืน — Convert ได้ 30% เพิ่ม Revenue ได้ทันที

### 2.ดัน Direct ลด OTA
OTA คิดเป็น 33.8% ของจองทั้งหมด เสีย Commission ฿279,239 (9.5% ของ Gross Revenue) ทั้งที่ Direct Website & Walk-in ให้ Net ADR สูงกว่า OTA อยู่ ฿20–28/คืน → ทำ Best Rate Guarantee + ลงทุน Google Hotel Ads
เป้าหมาย: Shift OTA → Direct 10% ภายใน 6 เดือน ประหยัด Commission ได้ ฿27,000+/ปี

### 3.Last-Minute Premium Pricing
จอง 0–7 วัน ได้ ADR ฿152.8 ต่ำกว่าจอง 31+ วัน (฿165.5) อยู่ ฿12.7 — ผิดหลัก Dynamic Pricing → ตั้ง Rule ปิด Discount Rate อัตโนมัติเมื่อ Booking Window ≤7 วัน และ Auto-close เมื่อ Occupancy เกิน 70%

### 4.Cancellation Policy แบบ Tiered
Non-Refundable มีแค่ 19.7% ทั้งที่ให้รายได้มั่นคงที่สุด → ปรับ Policy ตาม Lead Time:

30+ วัน → Non-Refundable ลด 5% (จูงใจ Lock-in)
8–14 วัน → Refund 50% ถ้า Cancel ก่อน 72 ชม.
≤7 วัน → Non-Refundable 100% เสมอ


### 5.ขยาย Corporate Channel
Corporate Agent Commission แค่ 10% และ Net ADR ฿146 ดีกว่า OTA ชัดเจน → ทำ Corporate Rate Agreement กับบริษัทในพื้นที่ แลกกับ Volume Commitment รายเดือน
เป้าหมาย: Corporate Net Revenue Share 15.8% → 20%

---
