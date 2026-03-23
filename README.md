
----
# Data City Traffic Optimization Project

> End-to-end data analysis project identifying congestion patterns and optimizing transit decisions using SQL, Tableau, and real-world modeling techniques.

## Project Summary

This project analyzes traffic congestion in a rapidly growing city using a combination of SQL, data modeling, and visualization. Raw intersection-level traffic data and district demographics were collected, cleaned, and transformed into meaningful metrics such as congestion index, traffic density, and traffic per capita.

By combining these datasets, the analysis identifies high-impact intersections, uncovers inefficiencies in infrastructure relative to population, and highlights when and where congestion is most severe. The results are presented in an interactive Tableau dashboard designed to support data-driven transportation planning decisions, including bus route and stop placement.


*Special Thanks to Bruceyboy24804 and gnznroses for there mods that allowed me to see more data than the game allows*

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
- Comparisons between our Most and Least Busy Intersections

📊 **View Dashboard:**  
https://public.tableau.com/views/DataCityProject/Congestion?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link

---

## Key Insights

- Certain intersections show consistently high congestion across all time periods  
- Congestion is not always tied to population — indicating infrastructure inefficiencies  
- Peak congestion varies by location (not strictly rush-hour dependent)  


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

