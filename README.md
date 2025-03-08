In my Austin Bikeshare analysis project, I followed a structured workflow to extract meaningful insights and create an interactive dashboard.
Understanding the Dataset: I started by familiarizing myself with the Austin Bikeshare dataset (data dictionary) available on Kaggle. This dataset included details about bikeshare stations, trips, durations, and status of the stations, timestamps, busy areas. 
Data Exploration and Queries:
Query 1: Calculated peak usage times to identify hours with the highest activity. This query identifies the busiest hours for bikeshare usage, which is essential for resource allocation (e.g., increasing bike availability during peak hours) and operational planning
Query 2: Found the top busiest stations. Understanding which stations handle the highest trip volumes helps identify key locations and evaluate station performance.
Query 3: Analyzed total trips, average trip duration, and the operational status of each station. This provided key performance metrics per station to gauge usage and efficiency. This query provides a comprehensive overview of station performance, including trip volume, average trip duration, and operational status. It helps assess not just how busy a station is but also how efficiently it is used.
The queries I wrote were tailored to extract meaningful insights from the dataset and align with the project goals of understanding bikeshare usage patterns, station performance, and overall system efficiency. Here's why each query was written and an explanation of what it accomplishes:
Query 1: Calculating Peak Usage Time
SELECT 
    s.station_id,
    s.name AS station_name,
    EXTRACT(HOUR FROM t.start_time) AS start_hour,
    COUNT(t.trip_id) AS trips_count
FROM 
    bigquery-public-data.austin_bikeshare.bikeshare_stations s
JOIN 
    bigquery-public-data.austin_bikeshare.bikeshare_trips t
ON 
    s.station_id = t.start_station_id
GROUP BY 
    s.station_id, s.name, EXTRACT(HOUR FROM t.start_time)
ORDER BY 
    trips_count DESC;
Explanation:
Extracted the hour from the trip start times to categorize trips by hour.
Counted the number of trips per hour for each station.
Sorted the results by trip count in descending order to highlight peak usage times.

Query 2: Top 5 Busiest Stations by Total Trips
SELECT 
    s.station_id, 
    s.name AS station_name, 
    s.location, 
    COUNT(t.trip_id) AS total_trips
FROM 
    bigquery-public-data.austin_bikeshare.bikeshare_stations as s 
JOIN 
    bigquery-public-data.austin_bikeshare.bikeshare_trips as t 
ON 
    s.station_id = t.start_station_id
GROUP BY 
    s.station_id, s.name, s.location
ORDER BY 
    total_trips DESC
LIMIT 100;
Explanation:
Joined the station data with trip data to link trips with their starting stations.
Aggregated the total number of trips for each station.
Ordered the results by trip count to rank stations and retrieve the busiest ones.

Query 3: Total Trips, Average Trip Duration, and Station Status
sql
Copy
Edit
SELECT 
    s.station_id,
    s.name AS station_name,
    COUNT(t.trip_id) AS total_trips,
    AVG(t.duration_minutes) AS avg_trip_duration,
    s.status
FROM 
    bigquery-public-data.austin_bikeshare.bikeshare_stations as s
JOIN 
    bigquery-public-data.austin_bikeshare.bikeshare_trips as t
ON 
    s.station_id = t.start_station_id
GROUP BY 
    s.station_id, s.name, s.status
ORDER BY 
    total_trips DESC;

Explanation:
Aggregated the total trips and calculated the average trip duration for each station.
Included station status (active/closed) to evaluate operational effectiveness.
Sorted by total trips to prioritize the most frequently used stations.
Identified the busiest stations and peak usage times, critical for resource allocation. Highlighted average trip durations for each station, offering insights into user behavior and station relevance. Determined the proportion of active versus closed stations, which helped in operational assessments.
How the Queries Contributed to the Dashboard

Query 1 powered the Trips Count by Start Hour (Line Chart), providing a clear visualization of peak usage times. The line chart depicts the distribution of trips across different hours of the day. The steep decline in trips after peak hours is visually evident, making it simple to identify the busiest hours (e.g., 5 PM and 3 PM).
Query 2 formed the basis for the Total Trips by Station Name (Bar Chart), highlighting the busiest stations. The bar chart highlights the total number of trips for the busiest stations. It emphasizes the dominance of the top station ("21st & Speedway @PCL") with over 179K trips, compared to others.
Query 3 provided data for the Average Trip Duration by Station (Table). The table lists stations with their average trip durations, ranked in descending order. This visualization makes it easy to compare and identify stations where users spend more time on average.
Status by Record Count (Pie Chart):A pie chart breaks down the proportion of active vs. closed stations, clearly showing that a majority (79.6%) of stations are active. This chart provides an at-a-glance understanding of the system's operational health.
By strategically designing these queries, I ensured that all key performance indicators (KPIs) could be derived and visualized effectively in the dashboard, providing actionable insights for stakeholders.
Key Performance Indicators (KPIs): Peak hours of bike usage. Top-performing stations in terms of trips. Average trip duration for each station. Operational status distribution (active vs. closed).

Interactive Filters:
The dashboard includes interactive filters, allowing users to refine the data by selecting specific stations, trip durations, or operational statuses. This makes the dashboard dynamic and user-friendly for deeper insights.
Each visualization complements the data analysis, ensuring the information is not only accurate but also accessible to stakeholders for quick decision-making and performance evaluation.
The process provided actionable insights into bikeshare usage patterns, station performance, and overall service efficiency. The dashboard effectively communicates these findings to stakeholders, aiding in decision-making and resource optimization

Access to dashboard: https://lookerstudio.google.com/u/0/reporting/ebb92595-2ab6-4141-a25c-7593bb32efd6/page/KlDeE/edit
