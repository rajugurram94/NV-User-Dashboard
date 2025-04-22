# NV-User-Dashboard

 1. README.md (Full Documentation)
markdown
Copy
Edit
# 🚀 NVIDIA - User Performance Dashboard (Power BI)

This project presents a real-time **User Performance Dashboard** built in **Power BI**, designed to monitor and evaluate annotation workforce efficiency and quality. It visualizes metrics like scorecards, active time, productivity, LA quality, and efficiency across users in a given timeframe.

---

## 📊 Dashboard Overview

### KPIs
- **Associates:** 23
- **Productivity:** 86%
- **LA Quality:** 85.17%
- **Avg. Active Time:** 6.46 hours
- **LA Efficiency:** 92.31%
- **Score Card:** 126.17%

### User-Level Metrics
- Score Card (Derived KPI)
- LA Efficiency
- Productivity
- LA Quality
- Average Active Time (in hours)

### Filters/Slicers
- Date Range (e.g., 01-Aug-2024 to 28-Aug-2024)
- User Name
- Attendance
- User Role
- Active Status
- Label Class
- Projects
- Work Orders

---

## 🧠 Dataset

- **Source:** Internal employee annotation data
- **Tables Used:**
  - `UserPerformanceData` (Fact Table)
  - `Calendar` (Date Dimension)
  - `UserDetails` (Dimension)
  - `ProjectDetails` (Dimension)

---

## 🧮 DAX Measures

### `Productivity`
```dax
Productivity = 
DIVIDE(SUM(UserPerformanceData[Total Completed Tasks]), SUM(UserPerformanceData[Total Assigned Tasks]), 0)
LA Quality
dax
Copy
Edit
LA Quality = 
AVERAGE(UserPerformanceData[Quality Score])
LA Efficiency
dax
Copy
Edit
LA Efficiency = 
DIVIDE(SUM(UserPerformanceData[Valid LA Outputs]), SUM(UserPerformanceData[Total LA Attempts]), 0)
Average Active Time
dax
Copy
Edit
AVG Active Time = 
AVERAGE(UserPerformanceData[Active Hours])
Score Card
dax
Copy
Edit
Score Card = 
CALCULATE(
    AVERAGEX(
        VALUES(UserPerformanceData[User ID]),
        DIVIDE(
            UserPerformanceData[Productivity %] +
            UserPerformanceData[LA Quality %] +
            UserPerformanceData[LA Efficiency %],
        3)
    )
)
🧹 Power Query Transformations
Removed null rows/columns

Added calculated columns:

Month, Week, Day

Full Name = First Name & Last Name

Converted time columns from text to Time/Decimal

Merged UserDetails and ProjectDetails via UserID & ProjectID

Applied filter for dates within range: 01-Aug-2024 to 28-Aug-2024

📈 Visuals Used
Card Visuals for KPI metrics

Matrix Table for user-level breakdown

Line Chart for trend analysis per KPI

Slicers for interactive filtering

Custom Theme: NVIDIA-branded color palette (black, green, white)

✅ Use Case
This dashboard supports:

Performance appraisal

Quality assurance review

Training needs analysis

Task allocation strategies

🚀 How to Use
Clone/download this repo

Open the .pbix file in Power BI Desktop

Refresh the data connections

Use slicers to analyze performance by date, project, or role

📁 Folder Structure
arduino
Copy
Edit
📁 NVIDIA-Performance-Dashboard
│
├── 📄 README.md
├── 📁 DAX-Formulas
│   └── measures.txt
├── 📁 PowerQuery-Steps
│   └── transform_steps.txt
├── 📁 Visuals
│   └── screenshots.png
├── 📁 Dataset (if shareable)
│   └── user_data_sample.csv
├── 📁 Deployment-Guide (optional)
│   └── setup.md
🔐 Confidentiality Notice
This dashboard is built on dummy/test data representative of internal operational structures. Ensure production deployment complies with your organization's data sharing policies.

pgsql
Copy
Edit

---

## 📂 2. `DAX-Formulas/measures.txt`

```dax
-- Productivity %
Productivity = DIVIDE(SUM(UserPerformanceData[Total Completed Tasks]), SUM(UserPerformanceData[Total Assigned Tasks]), 0)

-- LA Quality %
LA Quality = AVERAGE(UserPerformanceData[Quality Score])

-- LA Efficiency %
LA Efficiency = DIVIDE(SUM(UserPerformanceData[Valid LA Outputs]), SUM(UserPerformanceData[Total LA Attempts]), 0)

-- Average Active Time
AVG Active Time = AVERAGE(UserPerformanceData[Active Hours])

-- Score Card Metric
Score Card = 
CALCULATE(
    AVERAGEX(
        VALUES(UserPerformanceData[User ID]),
        DIVIDE(
            UserPerformanceData[Productivity %] +
            UserPerformanceData[LA Quality %] +
            UserPerformanceData[LA Efficiency %],
        3)
    )
)
🧼 3. PowerQuery-Steps/transform_steps.txt
text
Copy
Edit
1. Connected to Excel file: "UserPerformanceData.xlsx"
2. Removed empty rows and columns
3. Changed data types:
   - Date -> Date
   - Active Hours -> Decimal Number
4. Merged with 'UserDetails' on User ID
5. Merged with 'ProjectDetails' on Project ID
6. Created calculated columns:
   - Full Name = [First Name] & " " & [Last Name]
   - Status = if Attendance = "Yes" then "Active" else "Inactive"
7. Filtered rows for reporting period: 01-Aug-2024 to 28-Aug-2024
8. Loaded clean table into data model
