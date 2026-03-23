# Data City Project

A demonstration of my Analysis skills on my self built and solved proposal for traffic restructuring in my fictional city of Data City (a city built using Cities Skylines 2)

----
# Data City Traffic Optimization Project

## Overview

Data City is experiencing rapid population and economic growth, leading to increased congestion across key intersections. This project analyzes traffic patterns and district-level demographics to identify high-impact areas and propose data-driven transportation solutions.

The objective is to:
- Identify the busiest and most congested intersections  
- Understand when and why congestion occurs  
- Normalize traffic relative to population  
- Support recommendations for public transit improvements (bus routes and stops)  

---

---

## Data Pipeline

### 1. Data Collection
- Manually collected intersection traffic data from Cities Skylines 2  
- Gathered district-level demographic and land-use data  

---

### 2. Data Cleaning
- Cleaned datasets in spreadsheets:
  - Fixed formatting and naming inconsistencies  
  - Removed invalid or null values  
  - Verified totals using pivot tables  

- Spreadsheet references:
  - Demographics:  
    https://docs.google.com/spreadsheets/d/1QdkJI5ZYgkQTXjlkQtyHN-Nq7UX0UQnZfXkYOEwo3i0/edit?usp=sharing  
  - Traffic:  
    https://docs.google.com/spreadsheets/d/1Rltvqp1YFLVGFa90vXsG263Zlpo_qyiuqZAPEleyN90/edit?usp=sharing  

---

### 3. Data Transformation (SQL – BigQuery)

All transformations were performed in **Google BigQuery**.

#### Key Techniques Used:
- UNNEST (to unpivot time-based traffic data)
- JOIN (traffic + demographics)
- Aggregations (SUM, ROUND)
- Calculated metrics

---

### 4. Key Metrics

#### Traffic Metrics
* **Traffic Density** = Vehicles / Lane Count
  - Traffic Density is total vehicles compared by total lanes a higher density leads to lower speeds and more traffic
* **Congestion Index** = Vehicles / (Lane Count × 500)  
  - Congestion Index is the total vehicles compared to the available free flow we used a average of 500 vehicles per lane which shows us how full an intersection is compared to what it handles, closer to 1.0 means complete congestion and it is being overloaded
  
#### Normalized Metric
* **Traffic per Capita** = Vehicles / Population
  - Vehicles in a district compared to population

#### Demographic Metrics
- **Employment Rate** = Employed Population / Total Population  
- **Population Density** = Population / Area  

---

### 5. Data Visualization (Tableau)

Interactive dashboard built in Tableau to explore:

- Top congested intersections  
- Traffic volume by time period  
- Traffic Density per intersection 
- Total traffic per intersection  

📊 **View Dashboard:**  
https://public.tableau.com/views/DataCityProject/Congestion?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link

---

## Key Insights

- Certain intersections show consistently high congestion across all time periods  
- Congestion is not always tied to population — indicating infrastructure inefficiencies  
- Peak congestion varies by location (not strictly rush-hour dependent)  
- Some lower-population districts experience disproportionately high traffic  

---

## Tools & Technologies

- **SQL (BigQuery)** – Data transformation and analysis  
- **Tableau** – Data visualization and dashboarding  
- **Google Sheets** – Data cleaning and validation  
- **GitHub** – Version control and project documentation  

---

## How to Reproduce

1. Upload datasets to BigQuery:
   - `Demographics.Data`
   - `Traffic.Data`

2. Run SQL queries located in:
   - `/demographics/README.md`
   - `/traffic/README.md`

3. Export results as CSV

4. Connect Tableau to the processed datasets

---

## Project Outcome

This project demonstrates an end-to-end data analysis workflow:

- Raw data collection  
- Data cleaning and validation  
- SQL-based transformation and metric creation  
- Data visualization and storytelling  

The final output supports **data-driven transportation planning decisions**, specifically identifying where transit solutions (bus routes and stops) would have the highest impact.

---

## Next Steps

- Develop a scoring model to prioritize intersections  
- Propose optimized bus routes based on traffic flow  
- Incorporate time-based routing strategies  
- Expand dataset for long-term trend analysis  

---
