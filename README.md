# Health-Care-Data-Analysis-Insights
## 1. Problem Statement
  
  This project aims to analyze a comprehensive healthcare dataset
comprising medical examinations, hospitalization details, and customer 
profiles to extract insights into patient health profiles, medical histories, 
and healthcare costs. By exploring relationships between various health metrics,
identifying trends, and visualizing key patterns, we aim to deliver actionable 
insights to healthcare stakeholders for informed decision-making through rigorous 
data cleaning, transformation, exploration, and analysis.
## 2. Dataset Description

The project utilizes a multi-table relational healthcare dataset comprising **2,300+ patient records** organized across three primary sheets. The tables are linked through the common primary identifier **`Customer ID`**.

---

### 1. Table 1: `Customer Names`
Contains primary demographic identification details for each policyholder.

* **Total Records:** 2,335 rows
* **Primary Key:** `Customer ID`

| Field Name | Data Type | Description |
| :--- | :--- | :--- 
| `Customer ID` | Text / String | Unique alphanumeric identifier assigned to each customer | `Id1`, `Id2335` |
| `name` | Text / String | Full name of the customer (Title, Last Name, First Name) | `Hawks, Ms. Kelly` |

---

### 2. Table 2: `Medical Examinations`
Captures diagnostic biometrics, chronic medical history, and behavioral indicators relevant to clinical risk profiling.

* **Total Records:** 2,335 rows
* **Foreign Key:** `Customer ID`

| Field Name | Data Type | Description |
| :--- | :--- | :---
| `Customer ID` | Text / String | Alphanumeric identifier linking to patient  record 
| `BMI` | Decimal / Float | Body Mass Index measurement
| `HBA1C` | Decimal / Float | Glycated hemoglobin test value indicating blood sugar control 
| `Heart Issues` | Categorical / Text | Binary indicator of diagnosed cardiovascular conditions 
| `Any Transplants`| Categorical / Text | Indicator tracking prior organ or tissue transplants 
| `Cancer history` | Categorical / Text | History of prior or ongoing oncological conditions 
| `NumberOfMajorSurgeries` | Categorical / Text | Count of recorded major surgical procedures
| `smoker` | Categorical / Text | Tobacco smoking status (subject to data cleaning for `?` values)

---

### 3. Table 3: `Hospitalisation Details`
Logs hospitalization event timelines, policy dependency counts, geographical infrastructure classifications, and total claim billing.

* **Total Records:** 2,343 rows
* **Foreign Key:** `Customer ID` 

| Field Name | Data Type | Description |
| :--- | :--- | :---
| `Customer ID` | Text / String | Alphanumeric identifier linking to the patient master record
| `year` | Text / Number | Year of the hospitalization event or patient cohort birth year 
| `month` | Text / String | Abbreviated calendar month of the hospitalization event 
| `date` | Integer | Day of the month of the hospitalization event (1–31) 
| `children` | Integer | Number of dependent children covered under the policy 
| `charges` | Decimal / Float | Total hospitalization expense billed (USD) 
| `Hospital tier`| Categorical / Text | Facility grade based on medical infrastructure and capabilities 
| `City tier` | Categorical / Text | Municipal classification of the customer's location
| `State ID` | Text / String | Standard regional code representing administrative state boundaries
---

###  Summary
* **Relationship Type:** `Customer ID`.
* **Data Quality Notes:** Missing values (`?`) identified in `smoker`, `Hospital tier`, and `City tier` require categorical mode imputation during data pre-processing.
## 3. Data Preprocessing

Before proceeding with exploratory data analysis and dashboard development, raw data from three primary entities—**Customer Names**, **Medical Examinations**, and **Hospitalization Details**—underwent a structured data cleaning and feature engineering pipeline in Microsoft Excel to ensure integrity, consistency, and standard formatting.

---

### 1. Data Cleaning & Handling Missing Values

* **Identifying Missing Values:**
  * Explored across all tables to identify incomplete records flagged with placeholder characters (`"?"`) using the `COUNTIF` function:
    ```excel
    =COUNTIF(range, "?")
    
    ```

* **Imputation Strategy for Incomplete Records:**
  * **Month & Year:** Missing `Month` values were imputed with **September (`Sep`)**. The missing `Year` values were calculated using the column average rounded to the nearest integer (**1983**) and imputed via `Find & Replace (Ctrl + H)`.
  * **Mode Imputation for Categorical Features:** Computed the most frequently occurring values (modes) using `COUNTIF`:
    * `Hospital tier`: **tier - 2** (occurred 1,339 times)
    * `City tier`: **tier - 2** (occurred 812 times)
    * `smoker`: **No** (occurred 1,845 times)
    * Replaced all `"?"` entries in these columns with their respective mode values.
  * **State ID:** Replaced unknown/missing state identifiers with `"UNKNOWN"`.

* **Text Normalization & Data Cleansing:**
  * Cleaned excess whitespace and standardized casing across text fields using `PROPER()`.
  * In the `NumberOfMajorSurgeries` column, converted non-numeric strings (e.g., `"No Major surgery"`) to a clean numeric value of `0`.

---

### 2. Feature Engineering & Transformation

* **Customer Name Parsing:**
  * Deconstructed full customer names into distinct, atomic components: `Title`, `First Name`, and `Last Name` for improved indexing and personalization.

* **Date of Birth (DOB) Synthesis:**
  * Combined separate `Year`, `Month`, and `Date` columns into a unified `Date of Birth` field:
    ```excel
    =DATE(Year_Cell, Month_Cell, Date_Cell)
    ```
  * Formatted the entire column using custom date formatting: `DD-MMM-YYYY`.

* **Age Calculation:**
  * Calculated accurate customer ages based on the dataset collection date (**8th June 2023**) using `DATEDIF`:
    ```excel
    =DATEDIF(DOB_Cell, DATE(2023, 6, 8), "Y")
    ```

* **Health Metric Categorization:**
  * **Weight Status (BMI Classification):**
    Classified Body Mass Index (BMI) ranges using the `IFS` function:
    ```excel
    =IFs(BMI < 18.5, "Underweight", BMI <= 24.9, "Normal Weight", BMI <= 29.9, "Overweight", BMI >= 30, "Obesity")
    ```
  * **Diabetes Status (HbA1C Classification):**
    Segmented HbA1C levels using nested `IF` logic:
    ```excel
    =IF(HbA1C < 5.7, "Normal", IF(HbA1C <= 6.4, "Prediabetes", "Diabetes"))
    ```

* **Data Type & Financial Formatting:**
  * Explicitly formatted the `charges` column as standard currency (`$`, English - United States) rounded to 2 decimal places.

---

### 3. Data Integration & Modeling

* **Consolidation into Master Sheet (`Healthcare`):**
  * Used `Customer ID` as the unique relational key to join `Customer Names`, `Medical Examinations`, and `Hospitalization Details` into a consolidated master analysis sheet using `VLOOKUP`.
  * Retained core analytical attributes: `Customer ID`, `First Name`, `BMI`, `HbA1C`, `Heart Issues`, `Any Transplants`, `Cancer history`, `NumberOfMajorSurgeries`, `smoker`, `Weight Status`, `Diabetes Status`, `Date of Birth`, `charges`, `Hospital tier`, `City tier`, `State ID`, and `Age`.

## 4. Exploratory Data Analysis

## Overview
This project consolidates three separate source tables into a single, analysis-ready master sheet named **Health Care**. The consolidation is done entirely in Microsoft Excel using `VLOOKUP`, with **Customer ID** as the common key across all tables.

## Workbook Structure

| Sheet | Records | Description |
|---|---|---|
| **Customer Names** | 2,335 | Customer identity details: Customer ID, full name, title, first name, last name |
| **Medical Examinations** | 2,335 | Clinical indicators: BMI, HbA1c, heart issues, transplants, cancer history, major surgeries, smoking status |
| **Hospitalisation Details** | 2,343 | Hospital and demographic details: date of birth, age, children, charges, hospital tier, city tier, state ID |
| **Health Care** *(output)* | 2,335 | Consolidated master table combining all three sources |

## Methodology

### 1. Key Column
The **Customer ID** column was copied from the *Customer Names* sheet into column A of the new *Health Care* sheet. It serves as the lookup key for every other column.

### 2. Data Consolidation with VLOOKUP
All remaining columns in *Health Care* are populated with exact-match `VLOOKUP` formulas (`range_lookup = 0`), pulling values from the relevant source sheet.

```excel
=VLOOKUP(lookup_value, table_array, col_index_num, 0)
```

Example, fetching BMI from *Medical Examinations*:

```excel
=VLOOKUP(A2,'Medical Examinations '!A:J,2,0)
```

### 3. Column Mapping

| Health Care Column | Source Sheet | Source Column Index |
|---|---|---|
| First Name | Customer Names | 4 |
| BMI | Medical Examinations | 2 |
| HBA1C | Medical Examinations | 4 |
| Heart Issues | Medical Examinations | 6 |
| Any Transplants | Medical Examinations | 7 |
| Cancer History | Medical Examinations | 8 |
| Number of Major Surgeries | Medical Examinations | 9 |
| Smoker | Medical Examinations | 10 |
| Weight Status | Medical Examinations | 3 |
| Diabetes Status | Medical Examinations | 5 |
| Date of Birth | Hospitalisation Details | 5 |
| Charges | Hospitalisation Details | 8 |
| Hospital Tier | Hospitalisation Details | 9 |
| City Tier | Hospitalisation Details | 10 |
| State ID | Hospitalisation Details | 11 |
| Age | Hospitalisation Details | 6 |

## Outcome
A single clean dataset, one row per customer, ready for exploratory analysis, pivot tables and dashboards.

## Pivot Tables and Charts (Analysis Sheet)

All pivot tables are built from the combined **HealthCare** sheet.

### 1. Cancer History: Smokers vs Non-Smokers (Donut Chart)
**Pivot setup:** Rows = Smoker > Cancer history | Values = Count of Customer ID

| Smoker | No Cancer History | Cancer History | Total |
|---|---|---|---|
| Yes | 404 | 84 | 488 |
| No | 1,540 | 307 | 1,847 |

**Insight:** About 17.2% of smokers and 16.6% of non-smokers have a cancer history, so the difference is small.

### 2. Major Surgeries and Average HbA1c by Transplant History (Donut Chart)
**Pivot setup:** Rows = Any Transplants | Values = Sum of NumberOfMajorSurgeries, Average of HbA1c

| Any Transplants | Sum of Major Surgeries | Avg HbA1c |
|---|---|---|
| No | 1,417 | 6.67 |
| Yes | 162 | 5.19 |
| **Total** | 1,579 | 6.58 |

**Insight:** Patients with a transplant history have a lower average HbA1c (5.19 vs 6.67).

### 3. Healthcare Charges by Weight Status and Diabetes Status (Column Chart)
**Pivot setup:** Rows = Diabetes Status > Weight Status | Values = Average of charges

| Diabetes Status | Normal Weight | Obesity | Over Weight | Under Weight |
|---|---|---|---|---|
| Diabetes (avg $16,120) | $12,464 | $19,285 | $14,174 | $7,443 |
| Normal (avg $12,091) | $7,897 | $15,140 | $10,377 | $6,911 |
| Prediabetes | $10,492 | $15,283 | $9,741 | $5,648 |

**Insight:** Charges are highest for patients with obesity in every group, and lowest for underweight patients.

### 4. Average Charges by Hospital Tier Across States (Stacked Bar Chart)
**Pivot setup:** Rows = State ID | Columns = Hospital tier | Values = Average of charges

| Hospital Tier | Average Charges |
|---|---|
| Tier 1 | $30,189.68 |
| Tier 2 | $11,865.27 |
| Tier 3 | $9,462.27 |
| **Overall** | $13,523.52 |

**Insight:** Tier 1 hospitals cost far more than Tier 2 and Tier 3. States R1020, R1021, R1022 and R1025 show $0 for Tier 1 because they have no Tier 1 records.

### 5. Age vs BMI and HbA1c (Scatter Plot)
**Pivot setup:** Rows = Age group | Values = Average of BMI, Average of HbA1c

| Age Group | Avg BMI | Avg HbA1c |
|---|---|---|
| 18-27 | 30.21 | 5.59 |
| 28-37 | 31.28 | 5.54 |
| 38-47 | 30.74 | 6.52 |
| 48-57 | 31.26 | 8.05 |
| 58-67 | 32.06 | 8.10 |

**Insight:** HbA1c rises with age (5.59 to 8.10) while BMI stays almost flat (about 30 to 32).

### 6. Age vs Healthcare Charges (Scatter Plot)
**Pivot setup:** Rows = Age group | Values = Average of charges

| Age Group | Avg Charges |
|---|---|
| 18-27 | $8,622.41 |
| 28-37 | $12,159.76 |
| 38-47 | $14,874.16 |
| 48-57 | $16,480.54 |
| 58-67 | $19,232.18 |

**Insight:** Charges increase steadily with age.

---

## Interactive Dashboard

The **Dashboard** sheet brings all the charts together.

**KPI Cards**
- Total Patients: **2,335**
- Average Age: **39.22**
- Average Charges: **$13,529.92**

**Charts**
- Impact of Smoking on Cancer Prevalence (donut)
- Average Healthcare Charges by Hospital Tier Across States (stacked bar)
- Healthcare Expenditure across Weight and Diabetes Status (column)
- Major Surgeries & Average HbA1c by Transplant History (donut)
- Comparative Trends of BMI and HbA1c Across Age (scatter)
- Impact of Patient Age on Healthcare Expenditure (scatter)

**Slicers**
- **Weight Status** and **Diabetes Status** slicers are connected to the pivot tables through *Report Connections*, so one click filters the dashboard.

**Key Takeaways**
- Obesity combined with diabetes gives the highest charges.
- Age is the clearest driver of both HbA1c and charges.
- Hospital tier has a large effect on cost.


## Conclusion

This project demonstrates an end-to-end healthcare data analysis workflow in Excel. Raw data from multiple sheets was cleaned, transformed and combined using **VLOOKUP**. **Pivot tables** summarised key patterns, and an interactive **dashboard** with slicers presents the insights in one view.

Overall, structured data cleaning and pivot-based summarisation turn scattered healthcare records into clear, decision-ready insights.

## Tools Used

Microsoft Excel (VLOOKUP, Pivot Tables, Charts, Slicers)
