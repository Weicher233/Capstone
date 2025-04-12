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
 
**3. [Monte_Carlo.ipynb](./Monte_Carlo.ipynb) — Simulating OD Pairs with Monte Carlo**
- Includes:
  - Utilizes [StationFootfall_Total.csv](./StationFootfall_Total.csv) to simulate 10,000 random Origin-Destination (OD) pairs using weighted probabilities.
  - Excludes same-origin-destination pairs and identifies the most frequently simulated OD routes.
  - Plots visualizations such as bar charts, sunburst diagrams, and network graphs to present top OD pairs and route structures.
    (_Note: Due to GitHub's notebook preview limitations, the interactive sunburst chart does not render directly. [A static screenshot](./top_routes_sunburst_plot.png) has been included for reference._)

- Output datasets:
  - [SimulatedRoute_Frequency.csv](./SimulatedRoute_Frequency.csv): Frequency of simulated OD pairs for later modeling
