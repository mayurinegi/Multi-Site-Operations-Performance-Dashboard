# 📊 DAX Measures – Multi-Site Operations Performance Dashboard

## 🧠 Key Concept

Columns are evaluated in **row context** and stored in the model, whereas measures are evaluated in **filter context** and computed dynamically.

---

## 🎯 STEP 1: Base Measures (Foundation)

These measures provide the foundation for all calculations and ensure consistency across visuals.

### 💰 Total Cost
Total Cost = SUM(Operations[Cost])

### 💸 Total Budget
Total Budget = SUM(Operations[Budget])

### ⏱️ Total Cleaning Hours
Total Cleaning Hours = SUM(Operations[Cleaning Hours])

### ⏱️ Total Planned Hours
Total Planned Hours = SUM(Operations[Planned Hours])

### 🚨 Total Complaints
Total Complaints = SUM(Operations[Complaints])

💬 Insight:
"I start by creating base measures to ensure all calculations are reusable and consistent across visuals."

---

## 🎯 STEP 2: Variance Measures

### 💸 Cost Variance
Cost Variance = [Total Cost] - [Total Budget]

**Business Meaning:**
- Positive → Overspending ❌  
- Negative → Under budget ✅  

---

### ⏱️ Hours Variance
Hours Variance = [Total Cleaning Hours] - [Total Planned Hours]

⚠️ Note:
This does NOT automatically indicate good or bad performance.

💬 Insight:
"I use variance measures to compare actual vs target performance, but interpret them in context rather than in isolation."

---

## 🎯 STEP 3: Performance Logic (Calculated Column)

### 🔴 Performance Status
Performance Status =
IF(
    Operations[Cost] > Operations[Budget]
    || Operations[Complaints] > 3,
    "Underperforming",
    "Good"
)

**Logic Explanation:**
- Cost > Budget → Financial issue  
- High Complaints → Service quality issue  
- Avoids incorrect assumptions based on hours alone  

💬 Insight:
"I avoid using staffing variance alone to define performance. Instead, I combine financial and service quality indicators."

---

## 🎯 STEP 4: Underperforming Sites (KPI Measure)

### 🔢 Underperforming Sites
Underperforming Sites =
CALCULATE(
    DISTINCTCOUNT(Operations[Site]),
    Operations[Performance Status] = "Underperforming"
)

---

### 🧠 How It Works

Example:

| Site | Performance Status |
|------|------------------|
| A    | Underperforming  |
| A    | Underperforming  |
| B    | Good             |
| C    | Underperforming  |

After filtering:

| Site |
|------|
| A    |
| A    |
| C    |

DISTINCTCOUNT = **2 (A, C)**

---

### 🧠 Explanation

This measure uses **CALCULATE** to modify the filter context by selecting only rows where Performance Status is "Underperforming".  
Then **DISTINCTCOUNT** counts unique sites within that filtered context.  

Since measures operate in filter context, the result updates dynamically with slicers and visuals.

💬 Insight:
"This measure helps managers quickly identify how many sites require attention."

---

## 🎯 STEP 5: Complaint Level (Optional)

### 🚨 Complaint Level
Complaint Level =
IF(
    Operations[Complaints] > 3,
    "High",
    "Normal"
)

---

## 🎯 STEP 6: Advanced Metric

### 📊 Budget Utilization %
Budget Utilization % =
DIVIDE([Total Cost], [Total Budget], 0)

💬 Insight:
"I use percentage-based metrics to normalize performance across sites of different sizes."

---

## 🧠 Final Logic Summary

| Metric | Purpose |
|-------|--------|
| Total Cost | Overall spend |
| Cost Variance | Financial performance |
| Hours Variance | Operational insight |
| Complaints | Service quality |
| Performance Status | Combined evaluation |

---

## 🚀 Key Takeaway

This model demonstrates how combining financial, operational, and service metrics using DAX enables meaningful insights and better decision-making.
