# Hospital_Readmission_Analysis_Project

## 📊 Overview
This project analyzes hospital readmission rates using CMS Hospital Readmissions Reduction Program (HRRP) data. The analysis was conducted in BigQuery, and results were visualized in Power BI.

## 🛠 Tools Used
- Google BigQuery (SQL)
- Power BI
- Excel

## 📈 Key Insights
- The national average predicted readmission rate is approximately 15%.
- Certain states, such as DC and Kentucky, have higher average readmission rates.
- Top 5 facilities with the highest excess readmission ratios were identified.
- Trends over time were analyzed (though most data was for 2020).

## 🗂 Files Included
- `*.csv` files: Query outputs used in the Power BI dashboard.
- `dashboard_screenshot.png`: Visual preview.
- `Hospital_Readmission_Analysis.pdf`: Full exported report from Power BI.

## 💻 SQL Queries Used

```sql
-- Explore Your Data
SELECT
  r.`Facility Name`,
  r.`Facility ID`,
  r.`State`,
  r.`Measure Name`,
  r.`Number of Discharges`,
  r.`Excess Readmission Ratio`,
  r.`Predicted Readmission Rate`,
  r.`Expected Readmission Rate`,
  r.`Number of Readmissions`,
  r.`Start Date`,
  r.`End Date`
FROM
  `hospital-readmission-analytics.hospital_readmission.readmission_data` AS r
LIMIT 20;

-- Calculate Overall Average Predicted Readmission Rate
SELECT
  AVG(SAFE_CAST(r.`Predicted Readmission Rate` AS FLOAT64)) AS avg_predicted_readmission_rate
FROM
  `hospital-readmission-analytics.hospital_readmission.readmission_data` AS r;

-- Average Predicted Readmission Rate by Measure Name
SELECT
  r.`Measure Name`,
  ROUND(AVG(SAFE_CAST(r.`Predicted Readmission Rate` AS FLOAT64)), 2) AS avg_predicted_rate
FROM
  `hospital-readmission-analytics.hospital_readmission.readmission_data` AS r
GROUP BY
  r.`Measure Name`
ORDER BY
  avg_predicted_rate DESC;

-- Average Predicted Readmission Rate by State
SELECT
  r.State,
  ROUND(AVG(SAFE_CAST(r.`Predicted Readmission Rate` AS FLOAT64)), 2) AS avg_predicted_rate
FROM
  `hospital-readmission-analytics.hospital_readmission.readmission_data` AS r
GROUP BY
  r.State
ORDER BY
  avg_predicted_rate DESC;

-- Top 5 Facilities with Highest Excess Readmission Ratio
SELECT
  r.`Facility Name`,
  r.State,
  ROUND(SAFE_CAST(r.`Excess Readmission Ratio` AS FLOAT64), 2) AS excess_ratio
FROM
  `hospital-readmission-analytics.hospital_readmission.readmission_data` AS r
ORDER BY
  excess_ratio DESC
LIMIT 5;

-- Top 5 Facilities with Highest Predicted Readmission Rate
SELECT
  r.`Facility Name`,
  r.State,
  ROUND(SAFE_CAST(r.`Predicted Readmission Rate` AS FLOAT64), 2) AS predicted_rate
FROM
  `hospital-readmission-analytics.hospital_readmission.readmission_data` AS r
ORDER BY
  predicted_rate DESC
LIMIT 5;

-- Readmission Trends Over Time
SELECT
  EXTRACT(YEAR FROM r.`Start Date`) AS start_year,
  ROUND(AVG(SAFE_CAST(r.`Predicted Readmission Rate` AS FLOAT64)), 2) AS avg_predicted_rate
FROM
  `hospital-readmission-analytics.hospital_readmission.readmission_data` AS r
GROUP BY
  start_year
ORDER BY
  start_year;




## 🧑‍💼 Author

Zebiba Ousman
(https://www.linkedin.com/in/zebiba-ousman/)  


