# Citi-Bike-Rebalancing-Station-Availability-Exploratory-Analysis
I analyzed New York City Citi Bike operations using September 2019 trip data to understand where, when, and how often bikes are being moved by the system (interventions) versus by riders.
Overview:
I analyzed New York City Citi Bike operations using September 2019 trip data to understand where, when, and how often bikes are being moved by the system (interventions) versus by riders. Using R, I engineered an “interventions” dataset, estimated station-level bike availability over time, and built explanatory graphics that go beyond the classic Center for Spatial Research (CSR) heatmap to show how rebalancing actually plays out at specific stations.
Process:
1. Data ingestion & cleaning
Loaded the full September 2019 Citi Bike trip CSV
Standardized column names, converted trip duration to hours, and focused on core operational fields (bike IDs, stations, timestamps, user type). 

2. Engineering an “interventions” dataset
Sorted rides by bikeid and start_time, then used lag() within each bike to compare where a bike’s previous ride ended vs where the next ride began.
Defined a Citi Bike intervention as any case where end_station_name of ride n ≠ start_station_name of ride *n+1`, i.e., the bike was moved by staff between rides. 
Filtered out non-interventions and set usertype = "Citi Bike" for these system moves; computed the time between moves as tripduration in hours.

3. Station-level operational metrics
Calculated how many customer trips originate from a key hub station near Madison Square Garden (W 31 St & 7 Ave) and what percent of stations have more departures (≈ top 4% of all stations by outbound trips). 
Using the interventions-only dataset, counted how many times Citi Bike removed bikes from that same station and where it ranks across the network (≈ 77th percentile for removals). 

4. Explanatory graphics
Built a histogram of intervention tripduration (time between moves), showing that most bikes are redeployed within 24 hours, with a long tail of bikes out of circulation for days (likely deeper maintenance or storage). 
Aggregated interventions by station and mapped them onto a Manhattan basemap using sf + ggplot2, encoding removals per station via point size and color. The map shows rebalancing activity concentrated in Midtown and Lower Manhattan. 

5. Estimating bike availability over time
Combined rider trips and interventions into allmoves, then reshaped into a long event table with +1 for arrivals and −1 for departures. 
Computed a cumulative bike count per station (with a baseline adjustment) to estimate relative bikes available over time.
For W 31 St & 7 Ave, plotted two time series:
Black line: all movements (riders + interventions).
Red line: interventions only.
The graph reveals a fast hourly rider rhythm (commuter cycles) and slower, step-like operational interventions, where red “jumps” often follow periods of heavy rider activity—showing rebalancing as a corrective mechanism.

Key Findings:
A small set of high-demand stations (like W 31 St & 7 Ave) generate disproportionately high departures and attract more operational attention. 
Most system interventions turn bikes around within 24 hours, but a non-trivial tail of long-duration moves hints at maintenance, storage, or special operations. 
Rebalancing activity is spatially clustered in Midtown and Lower Manhattan, aligning with commuter and tourist hotspots. 
At a key hub, rider demand fluctuates rapidly, while Citi Bike responds with batched, slower interventions, giving clear guidance for optimizing truck schedules and staff deployment. 
