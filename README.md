# การเพิ่มประสิทธิภาพรายได้โรงแรม (Azure Stay)

---

## 1. Background & Pain Points

แม้ว่าโรงแรม Azure Stay จะมีอัตราการเข้าพัก (Occupancy Rate) อยู่ในระดับที่ดี แต่พบว่า Revenue Per Available Room (RevPAR) ยังคงต่ำกว่าคู่แข่งในตลาด ซึ่งสะท้อนถึงปัญหาในด้านกลยุทธ์การตั้งราคาและการบริหารช่องทางการขาย

ปัญหาหลักเกิดจากการใช้ราคาส่งเสริมการขาย (Promotion Rate) มากเกินไป โดยเฉพาะในช่วงที่มีความต้องการสูง (High Demand) แทนที่จะขายในราคาปกติ (Rack Rate) เพื่อเพิ่มรายได้สูงสุด

นอกจากนี้ โรงแรมยังขาดการใช้ข้อมูลในการตัดสินใจเชิงกลยุทธ์ เช่น การเลือกขายห้องผ่านช่องทางที่เหมาะสม หรือการตั้งราคาตามช่วงเวลาและพฤติกรรมลูกค้า ส่งผลให้เกิดการสูญเสียรายได้ (Revenue Leakage)

### ปัญหาหลัก:

* ใช้ Promotion มากเกินไปในช่วง High Demand
* สัดส่วน Rack Rate ต่ำ
* การเลือก Channel และการตั้งราคาไม่มีประสิทธิภาพ

### ผลกระทบทางธุรกิจ:

* ADR และ RevPAR ต่ำกว่าศักยภาพ
* สูญเสียโอกาสในการสร้างรายได้
* กลยุทธ์ Revenue Management ไม่มีประสิทธิภาพ

---

## 2. SMART Objective

เพิ่ม Average Daily Rate (ADR) ขึ้น 10% ภายใน 6 เดือน โดย:

* ลดสัดส่วนการขายแบบ Promotion ในช่วง High Demand
* เพิ่มสัดส่วนการขายแบบ Rack Rate
* ปรับสัดส่วนช่องทางการขาย (Channel Mix) ให้เหมาะสม

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

### fact_bookings

* booking_id
* guest_id
* channel_id
* booking_date
* checkin_date
* room_rate
* rate_code

### dim_channels

* channel_id
* channel_name
* commission_rate

### dim_guests

* guest_id
* segment

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

### Insight 1: Promotion ถูกใช้มากเกินไป

ในช่วง High Demand ยังมีการขายแบบ Promotion ในสัดส่วนสูง ทำให้ ADR ต่ำกว่าที่ควรจะเป็น

### Insight 2: Channel มีผลต่อกำไร

OTA มีจำนวน booking สูง แต่ Net ADR ต่ำ เนื่องจากค่าคอมมิชชั่นสูง ขณะที่ Direct ให้กำไรต่อ booking สูงกว่า

### Insight 3: Lead Time ส่งผลต่อราคา

ลูกค้าที่จองใกล้วันเข้าพักมีแนวโน้มจ่ายในราคาที่สูงกว่า

---

## 8. Business Recommendations

* ลดการใช้ Promotion ในช่วง High Demand
* เพิ่มสัดส่วน Rack Rate
* ส่งเสริม Direct Booking
* ใช้กลยุทธ์การตั้งราคาตาม Lead Time

---

## 9. GitHub Structure

```id="d6p7zv"
project/
│
├── data/
│   └── hotel_booking.csv
│
├── scripts/
│   ├── analysis.sql
│
├── results/
│   ├── adr_analysis.png
│   ├── channel_analysis.png
│
└── README.md
```

---

## 10. Conclusion

ผลการวิเคราะห์แสดงให้เห็นว่าปัญหาหลักเกิดจากการใช้ Promotion มากเกินไปและการเลือก Channel ที่ไม่มีประสิทธิภาพ

หากมีการปรับกลยุทธ์ด้านราคาและช่องทางการขาย จะสามารถเพิ่ม ADR และบรรลุเป้าหมายการเติบโต 10% ภายใน 6 เดือนได้

---
