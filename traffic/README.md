# Data City Traffic & Demographics Analysis

## Project Overview
**Data City** is experiencing rapid population and economic growth, resulting in traffic congestion across the city. This project analyzes traffic patterns at the busiest intersections, combined with district demographics, to highlight the most congested areas and provide insights for potential interventions such as bus routes or transit planning.

This dataset analyzes traffic patterns across the busiest intersections in Data City. Traffic counts were collected across three time periods (morning, afternoon, evening) and combined with district-level demographic data to identify congestion patterns and high-impact areas.

The goal of this dataset is to:
- Identify the busiest intersections
- Measure congestion relative to infrastructure
- Normalize traffic relative to population

---

## Data Sources

* **Raw Data:** `/traffic_raw.csv`
* **Processed Data:** `/traffic_metrics.csv`
* **Platform Used:** Google BigQuery
* **Spreadsheet (with Pivot Tables):**  
  https://docs.google.com/spreadsheets/d/1Rltvqp1YFLVGFa90vXsG263Zlpo_qyiuqZAPEleyN90/edit?usp=sharing

## Data Processing & Cleaning

Initial data was lightly cleaned in a spreadsheet to:

* Fix naming inconsistencies (road names, formatting)
* Remove invalid or blank values
* Standardize data types for SQL processing

Pivot tables were also created to:

* Compare traffic volumes across time periods  
* Identify high-traffic intersections  
* Validate totals before SQL processing  
---

###  Traffic
**Description:** Intersection-level traffic data collected across three 6-hour periods (morning, afternoon, evening). Each row contains traffic counts, lane information, and intersection types.

| Column | Description |
|--------|------------|
| intersection_id | Unique intersection identifier |
| district_id | Associated district |
| road_one / road_two | Names of intersecting roads |
| road_one_type / road_two_type | Road types (e.g., Arterial, Collector) |
| intersection_type | 3-way, 4-way, etc. |
| lane_count | Total number of lanes at the intersection |
| morning_traffic | Vehicles counted 4am–10am |
| afternoon_traffic | Vehicles counted 10am–4pm |
| evening_traffic | Vehicles counted 4pm–10pm |
| pedestrian_crossing | TRUE/FALSE indicator for pedestrian presence |

---

## Derived Metrics

The following metrics were calculated using SQL:

### 1. Traffic Density

Measures vehicles per lane at an intersection.

traffic_density = vehicle_count / lane_count

---

### 2. Congestion Index

Measures traffic relative to estimated free-flow capacity.

congestion_index = vehicle_count / (lane_count * 500)

---

### 3. Traffic per Capita

Normalizes traffic relative to district population.

traffic_per_capita = vehicle_count / population

---
## SQL Transformations

### 1. Create Intersection Name and Type
Combine road names and types for readability:

```sql
SELECT
  intersection_id,
  CONCAT(road_one, ' & ', road_two) AS intersection_name,
  CONCAT(road_one_type, ' & ', road_two_type) AS intersection_type
FROM `data-city-490717.Traffic.Data`;
```

### 2. Traffic by Time Period (Unpivot + Metrics + Join)

Results located in `/traffic_metrics.csv`

Notes:
* Converts wide traffic data into a time-based structure
* Calculates traffic density and congestion index
* Joins demographic data to normalize traffic
```sql
SELECT
  t.intersection_id,
  t.intersection_name,
  t.district_id,
  t.time_period,
  t.lane_count,

  -- traffic metrics
  t.vehicle_count,
  t.traffic_density,
  t.congestion_index,

  -- normalized metric
  ROUND(t.vehicle_count * 1.0 / d.population, 2) AS traffic_per_capita

FROM (
  SELECT
      intersection_id,
      district_id,
      CONCAT(road_one, ' & ', road_two) AS intersection_name,
      lane_count,
      time_period.time_period AS time_period,
      time_period.vehicle_count AS vehicle_count,

      ROUND(vehicle_count * 1.0 / lane_count, 2) AS traffic_density,
      ROUND(vehicle_count * 1.0 / (lane_count * 500), 2) AS congestion_index

  FROM `data-city-490717.Traffic.Data`,
  UNNEST([
    STRUCT('morning' AS time_period, morning_traffic AS vehicle_count),
    STRUCT('afternoon', afternoon_traffic),
    STRUCT('evening', evening_traffic)
  ]) AS time_period
) t

JOIN `data-city-490717.Demographics.Data` d
ON t.district_id = d.district_id;
```
----
Notes:

* Converts wide traffic data into a time-based structure
* Calculates traffic density and congestion index
* Joins demographic data to normalize traffic
----
### Output (Example Use Cases)

This dataset enables:
* Identification of peak traffic periods (morning, afternoon, evening)
* Ranking intersections by total traffic volume
* Detection of high congestion relative to infrastructure
* Comparison of traffic intensity across districts

---
### Assumptions & Notes

* Traffic counts are based on fixed 6-hour observation windows
* Congestion capacity is estimated at 500 vehicles per lane
* Pedestrian data is binary and not factored into congestion calculations
* Dataset is designed for analytical modeling, not real-world engineering precision

---

### Role in Project

This dataset is used to:
* Identify the busiest and most congested intersections
* Normalize traffic against population data
* Support decision-making for bus route placement
* Provide the foundation for congestion and accessibility analysis

It is combined with demographics data to generate insights and recommendations.

---

## Reproducibility

To reproduce results this should be completed following the demographics 
1. Upload `traffic.csv` to BigQuery as `Traffic.Data`
2. Run the SQL queries above changing FROM `data-city-490717.Traffic.Data` to `your-dataset-numbers.Demographics.Data`
   
