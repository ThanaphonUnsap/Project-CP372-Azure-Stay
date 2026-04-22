
# 🏨 Azure Stay Hotel การเพิ่มประสิทธิภาพรายได้โรงแรม 🏨

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
