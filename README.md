
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

## 5. Data Schema

โปรเจคนี้ใช้โครงสร้างแบบ Star Schema โดยมีตาราง fact เป็นศูนย์กลาง และ dimension tables สำหรับการวิเคราะห์เชิงธุรกิจ

---

###  fact_booking_finish (hotel_processed_data)

ตารางหลักสำหรับเก็บข้อมูลการจองและใช้ในการวิเคราะห์ทั้งหมด

| Column Name  | Description                         |
| ------------ | ----------------------------------- |
| booking_id   | รหัสการจอง                          |
| booking_date | วันที่ทำการจอง                      |
| checkin_date | วันที่เข้าพัก                       |
| channel_id   | รหัสช่องทางการจอง (FK)              |
| rate_code    | ประเภทเรทราคา (FK)                  |
| room_type_id | ประเภทห้อง (FK)                     |
| room_nights  | จำนวนคืนที่เข้าพัก                  |
| room_rate    | ราคาห้องต่อคืน                      |
| revenue      | รายได้รวม (room_rate × room_nights) |

---

###  dim_channels

ข้อมูลช่องทางการจอง

| Column Name     | Description                                  |
| --------------- | -------------------------------------------- |
| channel_id      | รหัสช่องทาง                                  |
| channel_name    | ชื่อช่องทาง (Direct Web, Booking.com, Agoda) |
| channel_type    | ประเภท (Direct / OTA)                        |
| commission_rate | อัตราค่าคอมมิชชั่น                           |

---

###  dim_rate_codes

ข้อมูลประเภทเรทราคา

| Column Name | Description              |
| ----------- | ------------------------ |
| rate_code   | รหัสเรทราคา              |
| rate_type   | ประเภท (Rack / Non-Rack) |
| description | รายละเอียดราคา           |

---

###  dim_room_types

ข้อมูลประเภทห้องพัก

| Column Name  | Description    |
| ------------ | -------------- |
| room_type_id | รหัสประเภทห้อง |
| room_type    | ชื่อประเภทห้อง |

---

###  dim_room_inventory

ข้อมูลจำนวนห้องที่มีในระบบ

| Column Name  | Description      |
| ------------ | ---------------- |
| room_type_id | รหัสประเภทห้อง   |
| total_rooms  | จำนวนห้องทั้งหมด |

---

###  dim_calendar

ข้อมูลวันที่สำหรับการวิเคราะห์เวลา

| Column Name | Description              |
| ----------- | ------------------------ |
| date        | วันที่                   |
| day_of_week | วันในสัปดาห์             |
| month       | เดือน                    |
| year        | ปี                       |
| is_weekend  | ระบุวันหยุด (True/False) |

---

### 🔹 dim_hotel_derived_features (Derived Table)

ตารางนี้เป็นข้อมูลที่สร้างขึ้นจากการคำนวณ (Derived Features) เพื่อใช้ในการวิเคราะห์เชิงธุรกิจ

| Column Name  | Description                             |
| ------------ | --------------------------------------- |
| booking_id   | รหัสการจอง                              |
| lead_time    | จำนวนวันล่วงหน้า                        |
| lead_bin     | กลุ่มการจอง (Short / Medium / Long)     |
| demand_level | ระดับความต้องการ (High / Normal)        |
| net_ADR      | รายได้เฉลี่ยต่อห้องหลังหักค่าคอมมิชชั่น |
| day_of_week  | วันในสัปดาห์                            |
| is_weekend   | ระบุวันหยุด (True/False)                |

---

## 🔹 Derived Logic

### Lead Time

```sql id="3g9c8z"
DATEDIFF(checkin_date, booking_date)
```

---

### Lead Bin

```sql id="9k3p1x"
CASE 
  WHEN lead_time <= 3 THEN 'Short'
  WHEN lead_time <= 14 THEN 'Medium'
  ELSE 'Long'
END
```

---

### Demand Level

```sql id="4w7xzd"
CASE 
  WHEN day_of_week IN ('Friday', 'Saturday') THEN 'High Demand'
  ELSE 'Normal'
END
```

---

### Net ADR

```sql id="k2d9qw"
(room_rate - (room_rate * commission_rate))
```

---

### Weekend Flag

```sql id="p8v2mt"
CASE 
  WHEN day_of_week IN ('Saturday', 'Sunday') THEN TRUE
  ELSE FALSE
END
```


---

##  Relationships

* fact_booking_finish.channel_id → dim_channels.channel_id
* fact_booking_finish.rate_code → dim_rate_codes.rate_code
* fact_booking_finish.room_type_id → dim_room_types.room_type_id
* fact_booking_finish.checkin_date → dim_calendar.date
* fact_booking_finish.booking_id → dim_hotel_derived_features.booking_id

---

##  Data Usage in Analysis

ข้อมูลจาก schema นี้ถูกนำไปใช้ในการวิเคราะห์และสร้างกราฟในไฟล์ .ipynb ได้แก่:

* Promotion Usage by Demand Level
* ADR Comparison (Rack vs Non-Rack)
* Channel Performance (Gross ADR vs Net ADR)
* Lead Time vs Pricing
* Channel Profitability Analysis

---

##  หมายเหตุ

* Schema นี้ออกแบบให้สอดคล้องกับ dataset จริงในโฟลเดอร์ data
* Derived fields ถูกสร้างขึ้นเพื่อใช้ในการวิเคราะห์เชิงธุรกิจ
* โครงสร้างนี้ช่วยให้สามารถวิเคราะห์ Revenue, Pricing และ Channel Performance ได้อย่างครบถ้วน

---



---



## 7. Insights

### 1. Inefficient Use of Promotions During Peak Demand

ในช่วง Peak Demand (วันศุกร์–เสาร์) โรงแรมยังคงมีการขายห้องพักในรูปแบบ Promotion ประมาณ 30%
ทั้งที่มีความต้องการสูง ส่งผลให้สูญเสียโอกาสในการขายในราคาเต็ม (Rack Rate) และทำให้รายได้ไม่ถูก maximize ตามศักยภาพ

---

### 2. High Volume Does Not Equal High Profitability

ช่องทาง Booking.com สามารถสร้างจำนวนการจองได้สูงที่สุด
แต่เมื่อหักค่าคอมมิชชั่นประมาณ 15% แล้ว พบว่า Net ADR ต่ำกว่าช่องทาง Direct Web อย่างมีนัยสำคัญ
สะท้อนให้เห็นว่าช่องทางที่มียอดจองสูง ไม่ได้หมายความว่าจะสร้างกำไรสูงเสมอไป

---

### 3. Pricing Does Not Reflect Actual Demand

ลูกค้าที่จองล่วงหน้า 14 วันขึ้นไป มักได้รับราคาที่ต่ำกว่าลูกค้าที่จองใกล้วันเข้าพัก
แสดงให้เห็นว่าการตั้งราคายังไม่สอดคล้องกับระดับความต้องการ (Demand) ที่แท้จริง
ซึ่งเป็นโอกาสในการนำกลยุทธ์ Dynamic Pricing มาใช้เพื่อเพิ่มรายได้

---


## 8. Business Recommendations

* ลดการใช้ Promotion ในช่วง High Demand
* เพิ่มสัดส่วน Rack Rate
* ส่งเสริม Direct Booking
* ใช้กลยุทธ์การตั้งราคาตาม Lead Time

---
