
# การเพิ่มประสิทธิภาพรายได้โรงแรม (Azure Stay)

<img width="1920" height="1080" alt="Project Canvas pptx" src="https://github.com/user-attachments/assets/6067512e-00ba-4672-a4e1-63f8137b8606" />
---

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
| M (Measurable) | เพิ่ม Average Daily Rate (ADR) อย่างน้อย 10% และลดสัดส่วนการใช้ Promotion ลงอย่างน้อย 15% ในช่วง High Demand                                                                                         |
| A (Achievable) | ใช้ข้อมูลการจองที่มีอยู่มาวิเคราะห์พฤติกรรมการตั้งราคา (Pricing) และ Channel Performance จากนั้นนำผลลัพธ์ไปปรับกลยุทธ์ เช่น จำกัด Promo และเพิ่ม Rack Rate                                           |
| R (Relevant)   | สอดคล้องกับเป้าหมายของ Azure Stay ในการเพิ่มรายได้ (Revenue) และลดการสูญเสียรายได้ (Revenue Leakage) จากการตั้งราคาที่ไม่มีประสิทธิภาพ                                                               |
| T (Time-bound) | ดำเนินการวิเคราะห์และปรับกลยุทธ์ให้เห็นผลภายใน 6 เดือน                                                                                                                                               |


---

## 3. Data Analytics Project Canvas

### Business Problem

ADR และ RevPAR ต่ำ เนื่องจากการตั้งราคาและเลือก Channel ไม่มีประสิทธิภาพ

### Business Goal

เพิ่ม ADR 10% ภายใน 6 เดือน

### Key Metrics

* ADR = รายได้รวม / จำนวนห้องที่ขายได้
* Net ADR = (รายได้รวม - ค่าคอมมิชชั่น) / จำนวนห้องที่ขายได้
* Net Revenue = รายได้รวม - ค่าคอมมิชชั่น
* Promotion Ratio = จำนวนการจองแบบ Promo / จำนวนการจองทั้งหมด

### Dimensions

* Booking Channel
* Rate Code (Promo / Rack)
* Demand Level (High / Low)
* Lead Time

### Hypotheses

1. มีการใช้ Promotion มากเกินไปในช่วง High Demand
2. ช่องทาง Direct ให้ ADR สูงกว่า OTA
3. ลูกค้าที่จองใกล้วันเข้าพักมีแนวโน้มจ่ายแพงกว่า

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

* จำกัดการใช้ Promotion ในช่วง Peak
* เพิ่ม Direct Booking
* ใช้ Dynamic Pricing

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
---
###  fact_booking_finish

ตารางหลักสำหรับเก็บข้อมูลการจอง (Transaction Table)

| Column Name  | Description                         |
| ------------ | ----------------------------------- |
| booking_id   | รหัสการจอง                          |
| guest_id     | รหัสลูกค้า (FK)                     |
| channel_id   | รหัสช่องทางการจอง (FK)              |
| booking_date | วันที่ทำการจอง                      |
| checkin_date | วันที่เข้าพัก                       |
| room_rate    | ราคาห้องต่อคืน                      |
| rate_code    | ประเภทเรทราคา (Promo / Rack)        |
| room_nights  | จำนวนคืนที่เข้าพัก                  |
| revenue      | รายได้รวม (room_rate × room_nights) |

---

###  dim_channels

ตารางข้อมูลช่องทางการจอง

| Column Name     | Description                                   |
| --------------- | --------------------------------------------- |
| channel_id      | รหัสช่องทาง                                   |
| channel_name    | ชื่อช่องทาง (Direct, Agoda, Booking, Expedia) |
| commission_rate | อัตราค่าคอมมิชชั่น (%)                        |

---

###  dim_guests

ตารางข้อมูลลูกค้า

| Column Name | Description                           |
| ----------- | ------------------------------------- |
| guest_id    | รหัสลูกค้า                            |
| segment     | ประเภทลูกค้า (Leisure, Business, VIP) |

---

###  dim_rate_code

ตารางประเภทเรทราคา

| Column Name | Description           |
| ----------- | --------------------- |
| rate_code   | รหัสเรทราคา           |
| rate_type   | ประเภท (Promo / Rack) |

---

###  dim_date

ตารางวันที่ (ใช้สำหรับวิเคราะห์เวลา)

| Column Name | Description        |
| ----------- | ------------------ |
| date        | วันที่             |
| month       | เดือน              |
| year        | ปี                 |
| day_of_week | วันในสัปดาห์       |
| is_weekend  | เป็นวันหยุดหรือไม่ |

---

###  dim_demand_level (Derived)

ตารางที่สร้างจากเงื่อนไข (Derived Dimension)

| Column Name  | Description                  |
| ------------ | ---------------------------- |
| demand_level | ระดับ Demand (High / Low)    |
| logic        | คำนวณจากจำนวน booking ต่อวัน |

ตัวอย่างเงื่อนไข:

```sql id="xq4n7m"
CASE 
  WHEN booking_count_per_day > threshold THEN 'High Demand'
  ELSE 'Low Demand'
END
```

---

##  Relationships (ความสัมพันธ์)

* fact_booking_finish.guest_id → dim_guests.guest_id
* fact_booking_finish.channel_id → dim_channels.channel_id
* fact_booking_finish.rate_code → dim_rate_code.rate_code
* fact_booking_finish.checkin_date → dim_date.date

---

### หมายเหตุ

* ตาราง fact_booking_finish เป็นศูนย์กลางของการวิเคราะห์ (Star Schema)
* ตาราง dim_* ใช้สำหรับอธิบายมิติข้อมูล (Dimensions)
* demand_level เป็น Derived Field ที่สร้างจาก logic ทางธุรกิจ

---


---

## 6. SQL Analysis

### 6.1 วิเคราะห์ ADR ตาม Rate และ Demand

```sql id="0e6b4d"
SELECT 
    demand_level,
    rate_code,
    COUNT(*) AS total_bookings,
    AVG(room_rate) AS adr,
    SUM(room_rate) AS revenue
FROM fact_bookings
GROUP BY demand_level, rate_code
ORDER BY demand_level;
```

---

### 6.2 วิเคราะห์ Channel และ Net Revenue

```sql id="6trn5u"
SELECT 
    c.channel_name,
    COUNT(*) AS bookings,
    SUM(f.room_rate) AS revenue,
    SUM(f.room_rate * c.commission_rate) AS commission,
    SUM(f.room_rate - (f.room_rate * c.commission_rate)) AS net_revenue,
    AVG(f.room_rate) AS adr,
    AVG(f.room_rate - (f.room_rate * c.commission_rate)) AS net_adr
FROM fact_bookings f
JOIN dim_channels c ON f.channel_id = c.channel_id
GROUP BY c.channel_name
ORDER BY net_revenue DESC;
```

---

### 6.3 วิเคราะห์ Lead Time

```sql id="0k0p4f"
SELECT 
    CASE 
        WHEN DATEDIFF(checkin_date, booking_date) <= 3 THEN 'Short'
        WHEN DATEDIFF(checkin_date, booking_date) <= 14 THEN 'Medium'
        ELSE 'Long'
    END AS lead_time_group,
    COUNT(*) AS bookings,
    AVG(room_rate) AS adr
FROM fact_bookings
GROUP BY lead_time_group
ORDER BY adr DESC;
```

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


