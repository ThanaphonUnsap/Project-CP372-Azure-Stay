# 📊 Hotel Revenue Optimization: Increasing ADR by 10%

## 📌 Project Overview

This project aims to increase the **Average Daily Rate (ADR)** by **10% within 6 months** by analyzing booking data, focusing on **Booking Channels** and **Rate Codes**.

The main goal is to optimize pricing strategy by:

* Reducing excessive promotional room sales during high-demand periods
* Increasing the proportion of full-price (**Rack Rate**) bookings
* Targeting high-value customers from the most profitable channels

---

## 🎯 Objectives

* Increase ADR by 10% within 6 months
* Identify inefficiencies in current pricing and promotion strategies
* Improve revenue performance by optimizing customer segmentation and booking channels

---

## 🧠 Hypotheses

### 🔍 Question 1

**Are we losing potential full-price revenue by offering too many promotions during high-demand periods?**

> Hypothesis:
> During high-demand periods, the proportion of promotional bookings is too high, leading to a lower ADR than expected.

---

### 🔍 Question 2

**Which booking channels bring in the highest value guests?**

> Hypothesis:
> Certain booking channels (e.g., Direct Booking) generate higher ADR compared to others (e.g., OTA platforms).

---

### 🔍 Question 3

**Does the booking lead time affect the selling price?**

> Hypothesis:
> Bookings made closer to the check-in date tend to have higher ADR compared to early bookings.

---

## 🗂️ Dataset Description

The dataset includes hotel booking data with the following key attributes:

* Booking Date
* Check-in Date
* Room Rate
* Rate Code (Promo / Rack Rate)
* Booking Channel
* Customer Information

---

## ⚙️ Methodology

### 1. Data Preparation

* Clean missing or inconsistent data
* Convert date fields into proper formats
* Calculate derived metrics such as Lead Time

---

### 2. Key Metrics

#### 📌 Average Daily Rate (ADR)

ADR = Total Room Revenue / Total Rooms Sold

#### 📌 Cancellation Rate (if applicable)

Cancellation Rate = Cancelled Bookings / Total Bookings

---

### 3. Analysis Approach

#### 🔹 Hypothesis 1 Analysis

* Segment data into **High Demand vs Low Demand periods**
* Compare:

  * % Promo vs Rack Rate
  * ADR difference
* Identify revenue loss opportunities

---

#### 🔹 Hypothesis 2 Analysis

* Group data by **Booking Channel**
* Calculate ADR per channel
* Identify **High Value Channels**

---

#### 🔹 Hypothesis 3 Analysis

* Calculate **Lead Time = Check-in Date - Booking Date**
* Segment into:

  * Short Lead Time
  * Medium Lead Time
  * Long Lead Time
* Compare ADR across segments

---

## 📊 Expected Outcomes

* Identification of overuse of promotions during high-demand periods
* Clear ranking of booking channels based on revenue contribution
* Insight into pricing strategy based on booking timing

---

## 🚀 Business Recommendations

* Limit promotional rate usage during high-demand periods
* Prioritize high-performing booking channels
* Implement dynamic pricing based on lead time
* Increase focus on Rack Rate customers

---

## 🗃️ Project Structure

```
project/
│
├── data/
│   └── hotel_booking.csv
│
├── scripts/
│   ├── data_cleaning.sql
│   ├── analysis.sql
│   └── analysis.py
│
├── results/
│   ├── adr_by_channel.png
│   ├── promo_vs_rack.png
│   └── lead_time_analysis.png
│
└── README.md
```

---

## 💻 Tools & Technologies

* SQL (PostgreSQL / MySQL)
* Python (Pandas, Matplotlib)
* Excel (optional)
* GitHub for version control

---

## 🔄 Version Control (GitHub)

* Initial dataset upload
* Data cleaning process
* Hypothesis testing scripts
* Final analysis and README update

---

## 🌍 Repository Access

This repository is set to **Public** for evaluation purposes.

---

## 📌 Conclusion

This project demonstrates how data-driven analysis can improve hotel revenue by optimizing pricing strategies, reducing unnecessary promotions, and focusing on high-value customer segments.

The insights from this analysis can directly support business decisions to achieve a **10% increase in ADR within 6 months**.

---
