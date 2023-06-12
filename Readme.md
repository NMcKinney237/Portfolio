# My Portfolio

Below are highlights and project samples related to my understanding of various softwares and code languages.


##SQL 

-- Pull station ID and name for join

WITH station AS (
    SELECT 
        name, 
        id AS station_id
    FROM stations
),

-- Join trip info to station table to get needed origin/destination info; ranked by trip distance from beginning station

trip_information AS (
    SELECT 
        trips.from_station_id AS origin_id,
        from_station.name AS origin_station,
        trips.to_station_id AS destination_id,
        to_station.name AS destination_station,
        trips.start_time_ts,
        trips.end_time_ts,
        tripduration / 60 AS trip_duration_minutes,
        ROW_NUMBER() OVER (PARTITION BY from_station_id ORDER BY tripduration DESC) AS duration_rank
    FROM trips
    LEFT JOIN station AS to_station ON to_station.station_id = trips.to_station_id
    LEFT JOIN station AS from_station ON from_station.station_id = trips.from_station_id
),

-- Output table; Grab the 2nd longest trip, accounting for the edge case of stations with less than two total trips

output AS (
    SELECT 
        origin_station,
        destination_station,
        trip_duration_minutes
        --, duration_rank
    FROM trip_information
    WHERE 
        duration_rank = 2
        OR (
            origin_id IN (
                SELECT origin_id 
                FROM trip_information
                GROUP BY origin_id
                HAVING COUNT(origin_id) < 2
            )
            AND duration_rank = 1
        )
),

-- Test for accuracy

test AS (
    SELECT * 
    FROM trip_information
    WHERE origin_station IN ('LaSalle (Wells) St & Huron St', 'State St & Randolph St', 'Canal St & Harrison St')
)

-- Select the output or test

SELECT *
FROM output;

## Python | Data Wrangling

Seen below:  A code example where I cleaned and re-organized multiple e-commerce databases into a usable data package showing granular order data. I also established code that allowed for a quick query, cutting the file size for ease of use in Excel or Tableau.

Data Cleaning Code:

<p align="center">
  <img src="https://github.com/NMcKinney237/Portfolio/blob/master/Graphics/Data_Clean_1.JPG">
</p>

<p align="center">
  <img src="https://github.com/NMcKinney237/Portfolio/blob/master/Graphics/Data_Clean_2.JPG">
</p>

<p align="center">
  <img src="https://github.com/NMcKinney237/Portfolio/blob/master/Graphics/Data_Clean_3.JPG">
</p>

Quick Analysis Code and Data Package:

<p align="center">
  <img src="https://github.com/NMcKinney237/Portfolio/blob/master/Graphics/Data_Query.JPG">
</p>

<p align="center">
  <img src="https://github.com/NMcKinney237/Portfolio/blob/master/Graphics/Data_Result.JPG">
</p>

## Tableau | Data Visualization

Seen below:  

A Tableau dashboard comprised of e-commerce data. Built in functionality includes a dynamic filtering system that allows you to seamlessly drill down by either city or customer, as well as a filtering system on the right hand side that allows for you to plot filtered data directly on a map viz. Lastly, there is functionality to download a JPEG/powerpoint file copy directly onto your computer.

Link to Tableau dashboard:  https://public.tableau.com/profile/nathan.mckinney8188#!/vizhome/BrazilE-CommerceData/Dashboard1

Original

<p align="center">
  <img src="https://github.com/NMcKinney237/Portfolio/blob/master/Graphics/Brazilian_Dashboard.JPG">
</p>

Filtered

<p align="center">
  <img src="https://github.com/NMcKinney237/Portfolio/blob/master/Graphics/Brazilian_Dashboard_Filter.JPG">
</p>

Map

<p align="center">
  <img src="https://github.com/NMcKinney237/Portfolio/blob/master/Graphics/Dashboard_Map.JPG">
</p>

## Tableau | Data Visualization

Seen below:  

A presentation slide element that was used alongside data tables to show the positioning of North American producers relative to higher demand regions. The dual-axis consisted of a heat map of annual product demand alongside a bubble map illustrating producer location/production capacity.

<p align="center">
  <img src="https://github.com/NMcKinney237/Portfolio/blob/master/Graphics/Dual_Axis_Map.jfif">
</p>

## Excel / Mekko Graphics | Data Visualization

Seen below:  

A Mekko Graphics slide element that highlighted global product trade flows over the span of a given year. The project required the integration of multiple datasets using Python scripting and Excel index_match functions.


<p align="center">
  <img src="https://github.com/NMcKinney237/Portfolio/blob/master/Graphics/EU_Trade.JPG">
</p>

## Excel | Pivot Tables and Dashboards 

Seen below:  

An analytical dashboard that utilized advanced pivot table and filtering functions as a means of seeing monthly sales premiums and tons by selling location, product, and transport mode, allowing the ability to analyze monthly sales metrics and determine how realized pricing at various locations compared to industry benchmarks over time.

<p align="center">
  <img src="https://github.com/NMcKinney237/Portfolio/blob/master/Graphics/Excel_Summary_Report.jpg">
</p>


