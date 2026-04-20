#  การเพิ่มประสิทธิภาพรายได้โรงแรม (ADR Optimization - Azure Stay)

##  Background & Pain Points

แม้ว่าโรงแรม Azure Stay จะมีอัตราการเข้าพัก (Occupancy Rate) อยู่ในระดับที่ดี แต่พบว่า **Revenue Per Available Room (RevPAR)** ยังต่ำกว่าคู่แข่งในตลาด ซึ่งสะท้อนถึงปัญหาด้าน **กลยุทธ์การตั้งราคา (Pricing Strategy)** และ **การบริหารช่องทางการขาย (Channel Management)**

ปัญหาหลักเกิดจากการใช้ **ราคาส่งเสริมการขาย (Promotional Rate)** มากเกินไป โดยเฉพาะในช่วงที่มีความต้องการสูง (High Demand) ส่งผลให้โรงแรมไม่สามารถสร้างรายได้สูงสุดจากลูกค้าที่มีศักยภาพในการจ่ายในราคาเต็ม (**Rack Rate**)

นอกจากนี้ โรงแรมยังขาดการใช้ข้อมูลในการวิเคราะห์เพื่อเลือก:

* ขาย “ห้องแบบไหน”
* ผ่าน “ช่องทางใด”
* ให้กับ “ลูกค้ากลุ่มไหน”
* ใน “ช่วงเวลาใด”

จึงเกิดปัญหา **Revenue Leakage** หรือการสูญเสียรายได้ที่ควรจะได้รับ แม้ว่าจะมี Demand อยู่แล้วก็ตาม

###  Pain Points หลัก:

* ใช้ Promo มากเกินไปในช่วง High Demand
* สัดส่วน Rack Rate ต่ำเกินไป
* Channel และ Pricing Strategy ไม่มีประสิทธิภาพ

###  ผลกระทบทางธุรกิจ:

* ADR และ RevPAR ต่ำกว่าคู่แข่ง
* สูญเสียโอกาสในการขายในราคาสูง
* ไม่สามารถ maximize รายได้จาก demand ที่มีอยู่

---

##  SMART Objectives

* **Specific**: วิเคราะห์ Booking Channel และ Rate Code เพื่อเพิ่มประสิทธิภาพการตั้งราคา
* **Measurable**: เพิ่ม ADR อย่างน้อย 10%
* **Achievable**: ใช้ข้อมูล booking ที่มีอยู่แล้ว
* **Relevant**: แก้ปัญหา Revenue Leakage และ Pricing Inefficiency
* **Time-bound**: ภายใน 6 เดือน

###  Goal:

เพิ่ม **Average Daily Rate (ADR)** ขึ้น **10% ภายใน 6 เดือน** โดย:

* ลดสัดส่วนการขายแบบ Promotion ในช่วง High Demand
* เพิ่มสัดส่วนลูกค้า Rack Rate
* ปรับ Channel Mix ให้เหมาะสม

---

## Hypotheses & Method

###  Hypothesis 1

ในช่วง High Demand มีการใช้ Promotion มากเกินไป ทำให้ ADR ต่ำกว่าที่ควรจะเป็น

---

###  Hypothesis 2

ช่องทางการจองบางประเภท (เช่น Direct Booking) ให้ ADR สูงกว่าช่องทางอื่น

---

###  Hypothesis 3

ลูกค้าที่จองใกล้วันเข้าพัก (Short Lead Time) มีแนวโน้มจ่ายราคาสูงกว่า

---

##  Measures (ตัวชี้วัด)

###  1. ADR (Average Daily Rate)

ADR = รายได้รวม / จำนวนห้องที่ขายได้

---

###  2. Net Revenue

Net Revenue = รายได้รวม - ค่าคอมมิชชั่น

---

###  3. Net ADR

Net ADR = (รายได้รวม - ค่าคอมมิชชั่น) / จำนวนห้องที่ขายได้

---

###  4. Promotion Ratio

Promotion Ratio = จำนวนการจองแบบ Promo / จำนวนการจองทั้งหมด

---

##  Dimensions (มิติข้อมูล)

###  1. Booking Channel

* Direct
* OTA (Agoda, Booking, Expedia)
* Travel Agent

---

###  2. Rate Code

* Promo
* Rack Rate

---

###  3. Demand Level (Derived)

```sql id="4z0h5q"
CASE 
  WHEN booking_count_per_day > threshold THEN 'High Demand'
  ELSE 'Low Demand'
END
```

---

###  4. Lead Time (Derived)

Lead Time = Check-in Date - Booking Date

---

##  Methodology

1. เตรียมข้อมูล (Data Cleaning)
2. สร้าง Derived Fields (Demand Level, Lead Time)
3. คำนวณ Metrics (ADR, Net ADR, Promotion Ratio)
4. วิเคราะห์ตาม Hypothesis
5. สรุป Insight และ Recommendation

---

##  Expected Insights

* พบการใช้ Promotion สูงเกินไปในช่วง High Demand
* Channel บางประเภทให้รายได้สูงกว่า
* Lead Time มีผลต่อพฤติกรรมราคา

---

##  Business Recommendations

* จำกัดการใช้ Promotion ในช่วง High Demand
* เพิ่มสัดส่วน Rack Rate
* เน้นช่องทางที่ให้ ADR สูง (เช่น Direct Booking)
* ใช้ Dynamic Pricing ตาม Lead Time

---

##  AI Data Generation

###  Prompt ที่ใช้สร้างข้อมูล

> สร้างชุดข้อมูลการจองโรงแรม โดยมี schema ดังนี้:

* fact_bookings (booking_id, guest_id, channel_id, booking_date, checkin_date, room_rate, rate_code)
* dim_channels (channel_id, channel_name, commission_rate)
* dim_guests (guest_id, segment)

เงื่อนไข:

* OTA มีจำนวน booking สูงและ commission สูง
* Direct มี commission = 0
* มีทั้ง Promo และ Rack Rate
* มีช่วง High Demand และ Low Demand

---

##  Data Schema

###  fact_bookings

* booking_id
* guest_id
* channel_id
* booking_date
* checkin_date
* room_rate
* rate_code

---

### dim_channels

* channel_id
* channel_name
* commission_rate

---

###  dim_guests

* guest_id
* segment

---

##  Project Structure

```id="c6q9xb"
project/
│
├── data/
│   └── hotel_booking.csv
│
├── scripts/
│   ├── data_cleaning.sql
│   ├── analysis.sql
│
├── results/
│   ├── adr_analysis.png
│   ├── channel_analysis.png
│   └── promo_analysis.png
│
└── README.md
```

---

##  GitHub Documentation

* README อธิบายครบ
* มี dataset (AI-generated)
* มี SQL / Python scripts
* โครงสร้างไฟล์ชัดเจน
* มี commit แสดงพัฒนา
* ตั้งเป็น Public Repository

---

##  Conclusion

แม้โรงแรมจะมี Demand สูง แต่การใช้ Promotion มากเกินไปและการเลือก Channel ที่ไม่เหมาะสม ทำให้สูญเสียรายได้ที่ควรจะได้รับ

การปรับกลยุทธ์ด้าน Pricing และ Channel จะช่วยเพิ่ม ADR และทำให้โรงแรมสามารถบรรลุเป้าหมาย **เพิ่มรายได้ 10% ภายใน 6 เดือน** ได้

---

##  Future Improvements

* ใช้ Machine Learning เพื่อทำนาย Demand
* พัฒนา Dynamic Pricing แบบอัตโนมัติ
* สร้าง Dashboard สำหรับติดตามผลแบบ Real-time

---
