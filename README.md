# UNION Operator for Data Consolidation
Combining and Enhancing Data with SQL UNION

## 📌 Overview
This project demonstrates how to use the **UNION operator** in SQL to merge and consolidate data from multiple tables. We focus on filling missing unemployment rate values in the `united_nations` database by substituting them with **regional averages** when country-specific data is unavailable.

## ⚠️ Important Note
This notebook requires a **local MySQL connection** and will **not** run on Google Colab. Ensure your environment has:

- MySQL Workbench installed

- The `united_nations` database set up

---
## 🎯 Learning Objectives
By the end of this training, you will:
✅ Understand the **UNION operator** and its role in combining query results.
✅ Apply **UNION** to merge data from different tables with similar structures.
✅ Enhance data completeness by **replacing missing values** with regional estimates.

---
## 📊 Problem Statement
We need to generate a **summary of unemployment rates** per country, but some entries are missing. To address this, we:

  1. First try to fetch country-specific unemployment rates from Economic_Indicators.
  
  2. If data is missing, substitute it with regional averages (see Table 1 below).

**Table 1: Regional Unemployment Rates**

| Region                            |	Pct_regional_unemployment
|---------------------------------------------------------------:|
| Central and Southern Asia         |	                     19.59 |
| Eastern and South-Eastern Asia    |	                     22.64 |
| Europe and Northern America       |	                     24.43 |
| Latin America and the Caribbean   |	                     24.23 |
| Northern Africa and Western Asia  |	                     17.84 |
| Oceania                           |	                      4.98 |
| Sub-Saharan Africa                |	                     33.65 |

---
## 🔍 SQL Solution
**Step 1: Fetch Country-Specific Data**
```
SELECT 
    gl.Country_name,
    gl.Region,
    ei.Pct_unemployment AS Unemployment_Rate,
    'Country Data' AS Data_Source
FROM 
    Geographic_Location gl
LEFT JOIN 
    Economic_Indicators ei ON gl.Country_name = ei.Country_name
WHERE 
    ei.Pct_unemployment IS NOT NULL;
```
**Step 2: Substitute Missing Data with Regional Averages**
```
SELECT 
    gl.Country_name,
    gl.Region,
    CASE 
        WHEN gl.Region = 'Central and Southern Asia' THEN 19.59
        WHEN gl.Region = 'Eastern and South-Eastern Asia' THEN 22.64
        WHEN gl.Region = 'Europe and Northern America' THEN 24.43
        WHEN gl.Region = 'Latin America and the Caribbean' THEN 24.23
        WHEN gl.Region = 'Northern Africa and Western Asia' THEN 17.84
        WHEN gl.Region = 'Oceania' THEN 4.98
        WHEN gl.Region = 'Sub-Saharan Africa' THEN 33.65
    END AS Unemployment_Rate,
    'Regional Estimate' AS Data_Source
FROM 
    Geographic_Location gl
LEFT JOIN 
    Economic_Indicators ei ON gl.Country_name = ei.Country_name
WHERE 
    ei.Pct_unemployment IS NULL;
```
**Step 3: Combine Results with UNION**
```
-- Final query merging both datasets
SELECT 
    gl.Country_name,
    gl.Region,
    ei.Pct_unemployment AS Unemployment_Rate,
    'Country Data' AS Data_Source
FROM 
    Geographic_Location gl
LEFT JOIN 
    Economic_Indicators ei ON gl.Country_name = ei.Country_name
WHERE 
    ei.Pct_unemployment IS NOT NULL

UNION

SELECT 
    gl.Country_name,
    gl.Region,
    CASE 
        WHEN gl.Region = 'Central and Southern Asia' THEN 19.59
        WHEN gl.Region = 'Eastern and South-Eastern Asia' THEN 22.64
        -- Add other regions as needed
    END AS Unemployment_Rate,
    'Regional Estimate' AS Data_Source
FROM 
    Geographic_Location gl
LEFT JOIN 
    Economic_Indicators ei ON gl.Country_name = ei.Country_name
WHERE 
    ei.Pct_unemployment IS NULL;
```

---
## ⚡ Key Takeaways
✔ **UNION merges** results from multiple `SELECT` statements.
✔ **Missing data** can be replaced with **default values** (e.g., regional averages).
✔ Always **specify data** sources for clarity (e.g., `'Country Data'` vs. `'Regional Estimate'`).

---
📚 Resources
- [MySQL UNION Documentation][https://dev.mysql.com/doc/refman/8.0/en/union.html]

- Handling Missing Data in SQL

---
## ✍️ Author  
**ExploreAI Academy**  
*Data Science Education Provider*  

## 🔄 Adopted By  
*Ibrahim Ambale*  
*ALX Data Science Student*  
