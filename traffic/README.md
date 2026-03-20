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

---

###  Traffic
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

---

