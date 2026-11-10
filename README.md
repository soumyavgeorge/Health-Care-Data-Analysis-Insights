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

## 4. Exploratory Data Analysis
