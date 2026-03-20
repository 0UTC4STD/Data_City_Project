# Data City Traffic & Demographics Analysis

## Project Overview
**Data City** is experiencing rapid population and economic growth, resulting in traffic congestion across the city. This project analyzes traffic patterns at the busiest intersections, combined with district demographics, to highlight the most congested areas and provide insights for potential interventions such as bus routes or transit planning.

The project demonstrates:
- Data collection and cleaning
- SQL transformations including unpivoting and metric calculation
- Joins between traffic and demographic datasets
- Key metrics such as traffic per lane, congestion index, and traffic per capita

---

## Datasets

### 1. Demographics
**File:** `demographics_employment_density.csv`  
**Description:** District-level population and employment metrics used to contextualize traffic data.

| Column | Description |
|--------|------------|
| district_id | Unique district identifier |
| district_name | District name |
| population | Total population |
| children | Population under 12 |
| teens | Population 13–19 |
| adults | Population 20–64 |
| seniors | Population 65+ |
| employed_pop | Number of employed residents |
| student_pop | Number of students |
| avg_income_usd_monthly | Average monthly income |
| area_acre | District area in acres |
| residential_count | Number of residential zones |
| commercial_count | Number of commercial zones |
| industrial_count | Number of industrial zones |
| office_count | Number of office zones |
| storage_count | Number of storage zones |

**Pivot Tables:** You can explore population distribution and employment metrics in the spreadsheet [here](https://docs.google.com/spreadsheets/d/1QdkJI5ZYgkQTXjlkQtyHN-Nq7UX0UQnZfXkYOEwo3i0/edit?usp=sharing).

---

### 2. Traffic
**File:** `traffic_intersections.csv`  
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

## SQL Transformations

### 1. Create Intersection Name
Combine road names and types for readability:

```sql
SELECT
  intersection_id,
  CONCAT(road_one, ' & ', road_two) AS intersection_name,
  CONCAT(road_one_type, ' & ', road_two_type) AS intersection_type
FROM `data-city-490717.Traffic.Data`;
