# Demographics Data – Data City Project

## Overview

This dataset contains district-level demographic and land-use data for Data City. It serves as a foundational dataset for analyzing traffic patterns and supporting transportation planning decisions.

The primary goal of this dataset is to provide **population context and land-use indicators** that can be joined with intersection-level traffic data to identify congestion patterns and prioritize transit solutions.

---

## Data Sources

* **Raw Data:** `/data/raw/demographics.csv`
* **Processed Data:** `/data/processed/demographics_employment_density.csv`
* **Platform Used:** Google BigQuery
* **Spreadsheet (with Pivot Tables):**
  https://docs.google.com/spreadsheets/d/1QdkJI5ZYgkQTXjlkQtyHN-Nq7UX0UQnZfXkYOEwo3i0/edit?usp=sharing

---

## Dataset Description

Each row represents a single district and includes:

### Core Fields

* `district_id` – Unique district identifier
* `district_name` – District name
* `population` – Total population
* `employed_pop` – Number of employed residents
* `student_pop` – Number of students
* `area_acre` – District size in acres

### Land Use Fields

* `residential_count`
* `commercial_count`
* `industrial_count`
* `office_count`
* `storage_count`

---

## Data Processing & Cleaning

Initial data was lightly cleaned in a spreadsheet to:

* Fix naming inconsistencies (typos, whitespace)
* Standardize column formatting
* Validate population totals

Pivot tables were also created in the spreadsheet to:

* Summarize population distribution
* Compare employment and student populations across districts
* Validate totals before SQL processing


All analytical transformations and metric calculations were performed in **BigQuery SQL** to ensure reproducibility and scalability.


---

## Derived Metrics

The following metrics were calculated using SQL:

### 1. Employment Rate

Proportion of the population that is employed.

```
employment_rate = employed_pop / population
```

### 2. Population Density

Population per square mile (converted from acres).

```
population_density = population / (area_acre * 0.0015)
```

---

## SQL: Derived Metrics Query

```sql
SELECT
  district_name, district_id, population, employment_rate, population_density,
FROM
  (
    SELECT
      district_name,
      district_id,
      population,
      ROUND((NULLIF(employed_pop, 0) / population), 2) AS employment_rate,
      CAST(population / NULLIF(area_acre * 0.0015, 0) AS INT64)
        AS population_density
    FROM `data-city-490717.Demographics.Data`
  )
WHERE employment_rate IS NOT NULL

```
---

## Output (Sample)

| district_name   | district_id | population | employment_rate | population_density |
| --------------- | ----------- | ---------- | --------------- | ---------------------- |
| Downtown        | dc001       | 4701       | 0.66            | 11738                  |
| Willow Junction | dc002       | 1826       | 0.62            | 6441                   |
| Green Springs   | dc005       | 1281       | 0.64            | 3917                   |
| South Beach     | dc004       | 968        | 0.57            | 2473                   |
| University      | dc003       | 870        | 0.46            | 1415                   |
| Hillside Brook  | dc007       | 110        | 0.59            | 76                     |

Full dataset available in:
`/demographics/district_metrics_demographics.csv`

---



---

## SQL: Aggregate Totals Query

```sql
SELECT    
    SUM(population) AS total_population,
    SUM(employed_pop) AS total_employed,
    SUM(student_pop) AS total_students,
    SUM(residential_count) AS total_residential_zones,
    SUM(commercial_count) AS total_commercial_zones,
    SUM(industrial_count) AS total_industrial_zones,
    SUM(office_count) AS total_office_zones,
    SUM(storage_count) AS total_storage_zones
FROM `data-city-490717.Demographics.Data`;
```



## Assumptions & Notes

* Population density is derived using a standard acre-to-square mile conversion factor
* Districts with zero population are excluded from metric calculations to avoid divide-by-zero errors
* This dataset is intended for analytical use and not real-world urban planning precision

---

## Role in Project

This dataset is used to:

* Normalize traffic data by population
* Identify high-impact districts
* Support scoring models for transit placement
* Provide context for congestion analysis

It will be joined with **intersection-level traffic data** in later stages of the project.

---

## Reproducibility

To reproduce results:

1. Upload `demographics.csv` to BigQuery as `Demographics.Data`
2. Run the SQL queries above changing FROM `data-city-490717.Demographics.Data` to `your-dataset-numbers.Demographics.Data`

---





