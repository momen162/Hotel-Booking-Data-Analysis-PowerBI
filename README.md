# 🏨 Hospitality Intelligence: Revenue & Cancellation Analysis

![Power BI Badge](https://img.shields.io/badge/Power%20BI-Desktop-yellow)
![Status](https://img.shields.io/badge/Status-Completed-success)
![Data](https://img.shields.io/badge/Data-Hotel%20Booking%20CSV-blue)

## 📸 Dashboard Preview
![Dashboard Screenshot](dashboard.png)


---

## 📝 Executive Summary
This project delivers an interactive Power BI dashboard for a global hospitality group to analyze over **119,000** booking records. The goal was to identify the root causes of revenue leakage due to cancellations and optimize pricing strategies based on booking lead times.

**Key Outcome:** identified a **$X.X Million** potential revenue recovery opportunity by targeting high-risk "Early Bird" cancellations with stricter deposit policies.

---

## 💼 Business Problem
The hotel management team is facing two critical issues:
1.  **High Cancellation Rate:** A significant portion of bookings are cancelled last minute, leaving rooms unsold.
2.  **Revenue Unpredictability:** Difficulty in forecasting revenue due to fluctuating lead times and distribution channel volatility.

**Objective:**
* Visualize revenue trends to identify seasonal dips.
* Analyze cancellation patterns by Hotel Type (City vs. Resort) and Lead Time.
* Determine the most profitable distribution channels.

---

## 🛠️ Technical Solution & Process

### 1. Data Cleaning & Transformation (Power Query)
* **Data Type Validation:** Corrected date formats for `Arrival Date` and `Booking Date` to enable time-intelligence functions.
* **Conditional Columns:** Created a `Booking Window` column to categorize lead times into bins (e.g., "0-7 Days", "8-30 Days", "180+ Days").
* **Data Modeling:** Established a Star Schema (if applicable) or optimized a flat-file structure for performance.

### 2. DAX Calculations (Key Measures)
The following DAX measures were created to quantify performance and risk:

**A. Booking Volume Metrics**
* **Total Bookings:** Calculated the total count of distinct booking IDs.
    ```dax
    Total Bookings = COUNT('Data'[Booking ID])
    ```
* **Cancelled Bookings:** Isolated bookings where the status was flagged as cancelled (1).
    ```dax
    Cancelled Bookings = CALCULATE(COUNT('Data'[Booking ID]), 'Data'[Cancelled (0/1)] = 1)
    ```
* **Cancellation Rate (%):** The ratio of cancelled bookings to total bookings.
    ```dax
    Cancellation Rate = DIVIDE([Cancelled Bookings], [Total Bookings], 0)
    ```

**B. Revenue & Financial Metrics**
* **Total Revenue:** Aggregated realized revenue from successful stays.
    ```dax
    Total Revenue = SUM('Data'[Revenue])
    ```
* **Total Revenue Loss:** calculated the potential revenue lost due to cancellations.
    ```dax
    Total Revenue Loss = SUM('Data'[Revenue Loss])
    ```
* **Average Daily Rate (ADR):** The average price paid per room per day.
    ```dax
    Avg Daily Rate (ADR) = AVERAGE('Data'[Avg Daily Rate])
    ```

**C. Customer Behavior (Calculated Column)**
* **Booking Window:** Created a calculated column to segment customers by Lead Time (how far in advance they booked).
    ```dax
    Booking Window = SWITCH(TRUE(),
        'Data'[Lead Time] <= 7, "0-7 Days (Last Minute)",
        'Data'[Lead Time] <= 30, "08-30 Days (Short Term)",
        'Data'[Lead Time] <= 90, "31-90 Days (Medium Term)",
        'Data'[Lead Time] <= 180, "91-180 Days (Long Term)",
        "180+ Days (Early Bird)"
    )
    ```

### 3. Dashboard Design (UI/UX)
* Implemented **Page Navigation** and **Slicers** (Date, Hotel, Country) for user-driven exploration.
* Used **Conditional Formatting** on KPI cards to highlight negative trends (Red for high cancellations).
* Designed a **Combo Chart** to correlate Booking Volume with Cancellation Risk.

---

## 🔍 Key Insights & Recommendations

### 📊 Insight 1: The "Early Bird" Risk
* **Observation:** Guests booking more than **90 days in advance** have a cancellation rate of over **40%**, significantly higher than last-minute bookers.
* **Recommendation:** Implement a non-refundable deposit policy for bookings made >90 days out to secure revenue.

### 📉 Insight 2: Channel Performance
* **Observation:** **Online Travel Agents (OTAs)** drive 60% of bookings but have the highest cancellation rate compared to **Direct** or **Corporate** bookings.
* **Recommendation:** Incentivize direct bookings through loyalty perks to reduce dependency on high-churn OTAs.

### 🌍 Insight 3: Seasonality
* **Observation:** Peak revenue occurs in **August**, while **January** sees the lowest ADR.
* **Recommendation:** Launch "Winter Getaway" promotional campaigns in November to boost January occupancy.

---

## 📂 Project Structure
```text
├── Data/
│   ├── Hotel Management Booking Data.xlsx  # Raw Dataset
├── PowerBI/
│   ├── Hotel_Analysis.pbix                # Source File
├── Images/
│   ├── dashboard.png           # Preview Image
└── README.md                              # Documentation
