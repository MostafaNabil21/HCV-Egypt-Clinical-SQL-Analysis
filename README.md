# 🧬 HCV Egypt — Clinical Data Analysis with MySQL

<p align="center">
  <strong>Exploratory Clinical Data Analysis of Hepatitis C Virus (HCV) Patients Using Pure MySQL</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/SQL-MySQL-blue?style=for-the-badge&logo=mysql&logoColor=white">
  <img src="https://img.shields.io/badge/Jupyter-Notebook-orange?style=for-the-badge&logo=jupyter&logoColor=white">
  <img src="https://img.shields.io/badge/Domain-Healthcare-red?style=for-the-badge">
</p>

---

## 📌 Project Overview

This project is an **exploratory clinical data analysis of patients with Hepatitis C Virus (HCV) patients from Egypt** performed entirely using **MySQL**.

The analysis explores demographics characteristics, clinical symptoms, laboratory measurements, histological grading and staging, viral load, and treatment response.

Rather than using Python, Pandas, R, or other analytical programming libraries, the project focuses on demonstrating how far **pure SQL can be taken for real-world exploratory data analysis**.

The complete analysis is documented in a **Jupyter Notebook**, where SQL queries are organized into analytical sections and accompanied by explanations and query results.

---

# 🎯 Project Goals

The primary goal is to use SQL to transform raw clinical data into meaningful analytical insights.

The project focuses on five major objectives:

### 1. Understand the patient population

Analyze:
  - Age
  - Gender
  - BMI
  - Demographic distributions

### 2. Investigate disease severity

Analyze:
  - Baseline histological staging
  - Histological grading
  - Clinical symptoms
  - Blood measurements
  - Liver enzymes

### 3. Analyze viral load

Investigate:
  - Baseline RNA
  - RNA at Week 4
  - RNA at Week 12
  - End-of-treatment RNA
  - Viral-load changes

### 4. Examine treatment response

Investigate:
  - Response groups
  - Baseline viral load vs response
  - Fibrosis stage vs response
  - Viral-load quartiles vs treatment response

### 5. Demonstrate advanced SQL

Apply:
  - CTEs
  - Subqueries
  - Conditional aggregation
  - CASE
  - Window functions
  - Ranking
  - Percentiles
  - Quartile analysis

---

## 🗃️ Dataset

### Hepatitis C Virus (HCV) for Egyptian Patients

The project uses the **Hepatitis C Virus (HCV) for Egyptian patients** dataset available through the:

**[🔗 UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/503/hepatitis+c+virus+hcv+for+egyptian+patients)**

The dataset was donated to UCI on **September 29, 2019**. It contains clinical information from Egyptian patients who underwent HCV treatment over approximately 18 months. :contentReference[oaicite:1]{index=1}

## Dataset Information

| Property | Description |
|---|---|
| Dataset | Hepatitis C Virus (HCV) for Egyptian patients |
| Repository | UCI Machine Learning Repository |
| Dataset ID | 503 |
| Domain | Health & Medicine |
| Dataset Type | Multivariate |
| Instances | 1,385 |
| Features | 28 |
| Missing Values | No|
| Primary Task | Classification |
| License | CC BY 4.0 |
| DOI | 10.24432/C5989V |

These details are taken from the official dataset documentation.

## 🧪 Dataset Context

According to the UCI documentation, the dataset contains Egyptian patients who underwent HCV treatment dosages over approximately **18 months**. UCI also notes that discretization should be applied according to expert recommendations and provide a separate discretization-criteria file.

The original UCI repository provides:

```text
HCV-Egy-Data.csv
Discretization-Criteria.csv
```
The dataset includes demographics information, symptoms, blood measurements, liver-enzyme measurements, viral-load measurements, and histological variables.

## 📊 Dataset Variables

The dataset contains several categories of clinical variabes.

**👤 Demographics**

```text
Age
Gender
BMI
```

**🩺 Clinical Symptoms**

 ```text
Fever
Nausea/Vomting
Headache
Diarrhea
Fatigue & generalized bone ache
Jaundice
Epigastric pain
```

The UCI variable documentation identifies these symptoms variables as binary features representing absent/present states.

**🩸 Blood Measurements**

```text
WBC  → White Blood Cell count
RBC  → Red Blood Cell count
HGB  → Hemoglobin
Plat → Platelet count
```

**🧪 Liver Enzymes**

```text
AST
ALT
```

including measurements at different treatment time points.

**🦠 Viral Load**

The dataset contains viral-load measurements including:

```text
RNA BASE
RNA 4
RNA 12
RNA EOT
RNA EF
```

These allow analysis of viral-load progression during treatment.

**🔬 Histological Variables**

The project focuses particularly on:

```text
Baseline histological Grading
Baselinehistological staging
```

These variables allow the analysis to investigate differences between fibrosis / histological stages.

## 🛠️ Technology Stack

***Database***

**MySQL**

The analysis is performed using SQL directly against the MySQL database.

***Notebook Environment***

**Jupyter Notebook**

The notebook is used as the analystical documentation layer.

it contains:
  - Markdown explanations
  - SQL queries
  - Query output

## 🧠 SQL Techniques Demonstrated

This project intentionally progresses from basic SQL to advanced analytical SQL.

**1. Basic Querying**

```text
  SELECT
  FROM
  WHERE
  ORDER BY
  LIMIT
```

Used for dataset exploration and filtering.

**2. Aggregation**

```text
  COUNT()
  AVG()
  MIN()
  MAX()
```

Used to summarize patient-level measurements.

**3. Grouping**

```text
  GROUP BY
```

Used to compare clinical characteristics across:
  - Gender
  - Age groups
  - Fibrosis stages
  - Response groups
  - Viral-load quartiles

**4. Conditional Logic**

```text
  CASE
    WHEN ...
    THEN ...
    ELSE ...
  END
```

Used for:
  - Age groups
  - Response classification
  - Clinical categories
  - Binary indicators

**5. Conditional Aggregation**

For example:
```text
AVG(
    CASE
        WHEN Fever = 2 THEN 1
        ELSE 0
    END
)
```

This allows SQL to calculate percentages such as:

  ```text Percentage of patients experiencing fever.```

##🔗 Subqueries

Subqueries are used to compare group-level results with overall population statistics.

For example:

```text
100 * COUNT(*) /
(
    SELECT COUNT(*)
    FROM hcv_egy
)
```

This allows each fibrosis stage to be expressed as a percentage of the total patient population.

##🧩 Common Table Expressions

The project uses CTEs to make complex queries easier to understand.

Example:

```text
WITH response_class AS (
    SELECT
        Age,
        BMI,
        Gender,
        CASE
            WHEN `RNA 12` <= 5
                THEN 'Rapid responder'
            WHEN `RNA EOT` <= 5
                THEN 'EOT responder'
            ELSE 'Non-responder'
        END AS response_group
    FROM hcv_egy
)
SELECT
    response_group,
    COUNT(*) AS n
FROM response_class
GROUP BY response_group;
```

CTEs allow the analysis to be divided into logical stages rather than putting everything into one large query.

##🪟 Window Functions

One of the main purposes of this project is to demonstrate advanced SQL window functions.

```**ROW_NUMBER()**```

  Assigns a sequential number to each patient within a group.
  
**RANK()**

  Ranks observations while preserving ties.
  
**DENSE_RANK()**

  Ranks observations without gaps after ties.
  
**NTILE()**

  Divides patients into approximately equal groups.
  
**PERCENT_RANK()**

  Calculates the relative ranking of a patient's value within a group.
  
**CUME_DIST()**

  Calculates the cumulative distribution of a value within a group.

##🔬 Analytical Workflow

The project follows this workflow:

```text
                    ┌─────────────────────┐
                    │   Raw HCV Dataset   │
                    └──────────┬──────────┘
                               │
                               ▼
                  ┌────────────────────────┐
                  │ Database Exploration   │
                  └────────────┬───────────┘
                               │
                               ▼
                  ┌────────────────────────┐
                  │ Demographic Analysis    │
                  └────────────┬───────────┘
                               │
                               ▼
                  ┌────────────────────────┐
                  │ Fibrosis / Histology    │
                  └────────────┬───────────┘
                               │
                               ▼
                  ┌────────────────────────┐
                  │ Clinical Symptoms       │
                  └────────────┬───────────┘
                               │
                               ▼
                  ┌────────────────────────┐
                  │ Laboratory Analysis     │
                  └────────────┬───────────┘
                               │
                               ▼
                  ┌────────────────────────┐
                  │ Viral Load Analysis     │
                  └────────────┬───────────┘
                               │
                               ▼
                  ┌────────────────────────┐
                  │ Treatment Response      │
                  └────────────┬───────────┘
                               │
                               ▼
                  ┌────────────────────────┐
                  │ Advanced SQL Analysis   │
                  └────────────┬───────────┘
                               │
                               ▼
                  ┌────────────────────────┐
                  │ Key Findings & Summary  │
                  └────────────────────────┘
```

## 📚 Analysis Sections

**1. Database Exploration**

The first section establishes the database environment and examines the table.

Example:

```text
SELECT
  TABLE_SCHEMA,
  TABLE_NAME
FROM information_schema.TABLES
WHERE TABLE_NAME = 'hcv_egy';

SELECT *
FROM hcv_egy
LIMIT 10;
```

**2. Demographic Analysis**

Questions:
  - How many male and female patients are present?
  - What is the age distribution?
  - What are the main age groups?
  - How does BMI vary across patient groups?
  - How do demographic characteristics differ by fibrosis stage?

**3. Fibrosis & Histological Analysis**

This is one of the core sections.

Questions:

  - How are patients distributed across fibrosis stages?
  - What is the average age at each stage?
  - What is the average BMI at each stage?
  - What is the gender composition of each stage?
  - How does histological grading relate to staging?
  - How do laboratory markers vary across stages?

**4. Clinical Symptoms**

Symptoms are analyzed individually and by fibrosis stage.

Examples:

```text
Fever
Jaundice
Fatigue
Nausea/Vomiting
Headache
Diarrhea
Epigastric Pain
```
The analysis used conditional aggregation to calculate symptoms prevalence.

**5. Laboratory Analysis**

The project compares laboratory measurements across fibrosis stages.

Key measurements:

```text
WBC
RBC
HGB
Platelets
AST
ALT
```

Derived analysis:

```text
AST / ALT Ration
```

The objective is to investigate whether laboratory measurements show different patterns across histological stages.

**6. Viral Load Analysis**

The project analyzes viral load at multiple points during treatment.

```text
Baseline
   ↓
Week 4
   ↓
Week 12
   ↓
End of Treatment
```

Questions indlude:
  - What is the baseline viral-load distribution?
  - Does viral load differ by fibrosis stage?
  - How much does viral load decrease?
  - What percentage of patients reach the defined undetectable threshold?
  - How does baseline viral load relate to treatment response?

**7. Treatment Response**

A response classification is constructed using SQL ``` CASE```.

Example conceptual structure:

```text
                    Treatment Response
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
    Rapid Response    EOT Response     Non-response

```

The groups are then profiled using:
  - Patient count
  - Average age
  - Average BMI
  - Gender
  - Fibrosis stage
  - Baseline RNA

**8. Viral Load Quartile Analysis**

Baseline viral load is divided into four groups:

```text
Q1 → Lowest baseline RNA
Q2
Q3
Q4 → Highest baseline RNA
```

using:

```text
NTILE(4)
```

The response rate of each quartile is then compared.

This provides a practical example of using a window function to create analytical groups.

**9. Percentile Analysis**

The project used:

```text
PERCENT_RANK()
```

and

```text
CUME_DIST()
```

to determine where each patient's baseline viral load lies within their fibrosis stage.

This answers questions such as:

```text
Is this patient's viral load relatively low or high compared with other patients in the same fibrosis stage?
```

##📈 Key Analytical Themes

The project can be summarized around five major themes:

```text
👥 PATIENTS
   Demographics
   Age
   Gender
   BMI

        ↓

🧬 DISEASE SEVERITY
   Fibrosis Stage
   Histological Grade

        ↓

🩸 CLINICAL STATUS
   Symptoms
   Blood Measurements
   Liver Enzymes

        ↓

🦠 VIRAL LOAD
   Baseline
   Week 4
   Week 12
   EOT

        ↓

💊 TREATMENT RESPONSE
   Response Groups
   RNA Reduction
   Response by Stage
   Response by RNA Quartile
```

## 🎓 Learning Outcomes

By completing this project, the following SQL skills are demonstrated:

**Beginner**
  Database exploration
  Filtering
  Sorting
  Aggregation
  Grouping
**Intermediate**
  Conditional aggregation
  CASE
  Subqueries
  Percentage calculations
  Derived columns
**Advanced**
  CTEs
  Window functions
  Partitioning
  Ranking
  Percentile analysis
  Quartile segmentation
  Multi-stage analytical queries

## 📁 Repository Structure

```text
HCV-Egypt-Clinical-SQL-Analysis/
│
├── 📓 HCV_Egypt_Clinical_Data_Analysis.ipynb
│
└── 📄 README.md
```

## 🏆 Project Highlights

```text
✓ Real-world healthcare dataset
✓ Egyptian HCV patient data
✓ 1,385 patient records
✓ 28 features
✓ Pure MySQL
✓ Jupyter Notebook
✓ Exploratory Data Analysis
✓ Data Quality Assessment
✓ Demographic Analysis
✓ Fibrosis Analysis
✓ Histological Analysis
✓ Clinical Symptom Analysis
✓ Laboratory Analysis
✓ Viral Load Analysis
✓ Treatment Response Analysis
✓ CTEs
✓ Subqueries
✓ CASE Expressions
✓ Conditional Aggregation
✓ ROW_NUMBER()
✓ RANK()
✓ DENSE_RANK()
✓ NTILE()
✓ PERCENT_RANK()
✓ CUME_DIST()
```

