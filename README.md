# Capstone
The aim of this study is to construct a set of data-driven transport stop optimisation models to enhance the efficiency of public transport operations and improve the travel convenience of the accessible population. To this end, we synthesise key literature results on bus stop optimisation and accessible travel. Based on the incorporation of previous research, we combine techniques such as GIS spatial analysis, data mining and multi-criteria decision-making techniques to optimise the spatial layout of bus stops, thereby providing decision support for public transport planning in London and promoting the sustainable development of bus stops.

## Original Data Source
**1. Footfall Data**  
- Source:  
  https://app.powerbi.com/view?r=eyJrIjoiMjZjMmQwYTktZjYxNS00MTIwLTg0ZjAtNWIwNGE0ODMzZGJhIiwidCI6IjFmYmQ2NWJmLTVkZWYtNGVlYS1hNjkyLWEwODljMjU1MzQ2YiIsImMiOjh9
  or https://crowding.data.tfl.gov.uk/Network%20Demand/StationFootfall_2023_2024%20.csv  
- Description: Historical station-level passenger flow data from the London Underground (TfL)

**2. Station Location Data**  
- Bus Stop Locations: via [TfL API](https://api.tfl.gov.uk/StopPoint/Mode/bus?page={page})
- Tube Station Locations: https://foi.tfl.gov.uk/FOI-2209-2122/Station%20locations.csv  

**3. Accessibility Data**  
- Step-free Access Information: https://content.tfl.gov.uk/step-free-tube-guide-map.pdf  
- Note: Additional data sources will be explored to supplement this guide and ensure comprehensive coverage of accessibility infrastructure.
- After manually transferring the chart data into an Excel spreadsheet: [FINISH_station_facility.xlsx](./FINISH_station_facility.xlsx)



## Notebooks
**1. [Bus_stop_latest.ipynb](./Bus_stop_latest.ipynb) — TfL Bus Stop Data Collection & Processing**
- Includes: 
  - Retrieves data from the official TfL (Transport for London) StopPoint API to gather comprehensive information on all London bus stops.
  - Extracts key attributes: `id`, `naptanId`, `commonName`, `lat`, `lon`, `modes`, and `lines`.
  - Performs preprocessing by checking for missing values, filtering stops by service types, and merging line information.
  - Adds an `is_pure_bus` column to indicate whether a stop exclusively serves buses.
 
- Output datasets:
  - [latest_bus_stops.csv](./latest_bus_stops.csv): Raw full list of TfL bus stops
  - [processed_bus_stops.csv](./processed_bus_stops.csv): Cleaned and feature-enhanced version for further analysis
  
**2. [footfall_preprocess.ipynb](./footfall_preprocess.ipynb) — Station Footfall Data Preprocessing & Visualization**
- Includes:
  - Loads, cleans, and combines annual station footfall datasets (2019–2024), including entry and exit counts.
  - Converts dates to datetime format, handles missing/invalid values, and calculates total footfall per station per day.
  - Aggregates and ranks stations by total and average daily flow.
  - Plots visualizations such as bar charts and ranked station lists.
  
- Output datasets:
  - [StationFootfall_Total.csv](./StationFootfall_Total.csv): Total entry/exit counts per station from 2019 to 2024
  - [StationFootfall_DailyAverage.csv](./StationFootfall_DailyAverage.csv): Average daily footfall per station
 
**3. [Monte_Carlo.ipynb](./Monte_Carlo.ipynb) — Simulating OD Pairs with Monte Carlo(Latest version:[Monte_Carlo_numbat.ipynb](./Monte_Carlo_numbat.ipynb))**
- Includes:
  - Utilizes [StationFootfall_Total.csv](./StationFootfall_Total.csv) to simulate 10,000 random Origin-Destination (OD) pairs using weighted probabilities.
  - Excludes same-origin-destination pairs and identifies the most frequently simulated OD routes.
  - Plots visualizations such as bar charts, sunburst diagrams, and network graphs to present top OD pairs and route structures.
    (_Note: Due to GitHub's notebook preview limitations, the interactive sunburst chart does not render directly. [A static screenshot](./top_routes_sunburst_plot.png) has been included for reference._)

- Output datasets:
  - [SimulatedRoute_Frequency.csv](./SimulatedRoute_Frequency.csv): Frequency of simulated OD pairs for later modeling
 
**4.  [Accessibility for TFL.ipynb](./Accessibility%20for%20TFL.ipynb) - TFL Tube Station Data Collection & Consolidation and processing of station data**
- Includes:
  - Comprehensive analysis of the accessibility of the London Underground and bus systems, focusing on the coverage of accessible (step-free) routes.
	- The data of Tube stations and bus stops are merged, and the adjacent stations are labeled based on spatial distance (0.5 miles), which is used to assess the transportation connectivity.
	- Data processing includes: capturing bus station and subway line data through the TfL API, integrating station geolocation, and cleaning and merging station names.
	- Charting included:
	- Figure 1: Histogram of the distribution of the number of neighboring stops - showing the distribution of the number of neighboring stops within 0.5 miles of each stop, with the majority of stops having 10-20 neighbors, reflecting the density of the transportation network in the London area;
	- Figure 2[（london_station_heatmap.html）](./data/london_station_heatmap.html): Spatial Heat Map with Average Number of Neighbours - a geographically based visualization of the density of traffic around a site, revealing areas of accessibility difference.


- Output datasets:
	- [Station locations.csv](./data/Station%20locations.csv): metro station location data
	- [merged_stations.csv](./data/merged_stations.csv): Merged bus and subway station information.
	- [stations_with_neighbors.csv](./data/stations_with_neighbors.csv): Data table with 0.5 mile neighboring station information added.
 - NUMBAT: A typical day represents 0500-0459 for each daytype in autumn each year. Data is collected for each weekday, Saturday and Sunday in each autumn, with days affected by major disruptions, events and closures excluded. These typical day counts generally represent a busier time of the year with more consistent travel patterns. (Initial exploration of NUMBAT data is included in [Accessibility for TFL.ipynb](./Accessibility%20for%20TFL.ipynb))
   - [NBT23FRI_outputs.xlsx](./data/NBT23FRI_outputs.xlsx)
   - [NBT23MON_outputs.xlsx](./data/NBT23MON_outputs.xlsx)
   - [NBT23SAT_outputs.xlsx](./data/NBT23SAT_outputs.xlsx)
   - [NBT23SUN_outputs.xlsx](./data/NBT23SUN_outputs.xlsx)
   - [NBT23TWT_outputs.xlsx](./data/NBT23TWT_outputs.xlsx)

**5.  [Data_Exploration_maps.ipynb](./Data_Exploration_maps.ipynb) - High-occupancy Tube station and bus analysis, mapping**  
This notebook analyzes London Underground stations with high passenger traffic but lacking step-free (lift) access. It integrates data on [station footfall](./StationFootfall_Total.csv), [facilities](./FINISH_station_facility.xlsx), [locations](./data/Station%20locations.csv), and [bus stop locations](./processed_bus_stops.csv) to identify underserved areas.    
- Includes:  
  - Cleaning and merging raw datasets to isolate ‘lift-free’ stations with the highest total passenger counts
  - Calculating straight-line distances to the nearest bus stops using geospatial methods
  - Generating interactive Folium maps to visualize critical locations and highlight problem areas

- Output plots:
  - [no_stepfree_stations_map.html](./no_stepfree_stations_map.html): All high-traffic Tube stations without lift access
  - [crisis_stations_map.html](./crisis_stations_map.html): Top critical stations with no lift access and poor bus proximity (>200m)  
    *(Note: The current crisis map is a preliminary version. It only considers the straight-line distance from Tube stations to the nearest bus stops and does not yet account for factors like bus directions, transfer options, or service availability. Future updates will incorporate these factors for a more comprehensive analysis.)*

**6. [P-median_ratio.ipynb](./P-median_ratio.ipynb)**
Purpose: Ratio-based analysis using the accessibility efficiency metric η and P-Median recommendation.
- Includes:
  - Compute η = (step-free travel time) / (ideal shortest time) per OD pair
  - Plot histogram and CDF of η; determine threshold (η ≈ 3.74) using elbow method
  - Aggregate excess burden (η − threshold) across stations to rank candidates
  - Use P-Median (with p=3) to identify optimal locations for new accessible stops
  - Perform sensitivity analysis on the efficiency threshold η and facility number p to verify the reasonableness of the parameters.
