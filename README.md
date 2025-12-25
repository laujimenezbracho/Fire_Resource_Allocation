# Project Overview Fire_Resource_Allocation
Wildfires in British Columbia have increased in frequency and severity, making strategic crew placement critical for minimizing response times and mitigating damage.
This project develops a large-scale optimization model to determine how different types of firefighting crews should be distributed across existing BC Wildfire Service (BCWS) stations to minimize response time–weighted distance, while respecting real-world operational constraints.
The model integrates historical wildfire data, geospatial clustering, and mixed-integer linear programming to support data-driven decision-making for wildfire response planning.

# Objectives
- Optimize the allocation of 4 firefighting crew types across 33 existing fire stations
- Ensure 100% coverage of wildfire demand zones
- Minimize response time–weighted travel distance
-Respect capacity, logistics, and aviation constraints
-Provide a scalable planning tool for BC Wildfire Service

# Problem Set Up
Supply Side:
- 33 BCWS fire stations across British Columbia
- Each station can host multiple crew types
- Per-station limits to prevent unrealistic crew concentration
Demand Side
- 25 wildfire demand zones, generated via K-Means clustering
- Zones represent fire-prone areas based on historical incidents
Crew Types
- IA (Initial Attack)	Helicopter-based first responders
- Rapattack	Helicopter-deployed rapid response crews
- Parattack	Parachute-deployed crews for remote access
- Unit Crew	Ground crews for extended campaign firefighting
Each crew has a seasonal capacity of 219 crew-days

# Data Sources
BC Wildfire Fire Incident Locations Database 
Government of British Columbia Open Data Portal
- Time period: 2012–2024 (filtered for recent relevance)

# Data Cleaning & Processing
- Removed fires < 0.1 hectares
- Included only “Full” response fires
- Removed missing values
- Converted coordinates to BC Albers projection for accurate distance calculations

  # Feature Engineering
- Fire duration estimation based on fire size (e.g. <1 ha → 0.5 crew-days)
- Aggregated historical incidents into annual crew-day demand
- Weighted clustering using log(fire size) to reflect severity

# Clustering
K-Means clustering (k = 25)
Features: X_COORDINATE, Y_COORDINATE
Output:
25 fire demand zone centroids
Average annual crew-day workload per zone

# Optimization Model
Decision Variables
Yᵢₖ: Number of crews of type k stationed at base i (integer)
Xᵢⱼₖ: Fraction of zone j workload served by crew k from station i (continuous)
Binary variables for aviation base eligibility and presence
Total variables: 3,564
(132 integer, 3,300 continuous, 132 binary)

# Objective 
Minimize total response time–weighted distance:
Accounts for:
Distance between stations and fire zones
Zone-level wildfire workload
Crew-specific travel speed

# Key Constraints
100% demand satisfaction for all fire zones
Crew capacity limits per station and per type
Province-wide crew availability constraints
Aviation basing limits (max 2 Parattack & 2 Rapattack bases)
IA precedence rule (advanced crews require IA presence)
Maximum response distance by crew type
Workload-sharing rules for large fires to ensure realistic multi-crew responses

# Extensions & Future Work
Real-time dynamic reallocation as fires emerge
Multi-objective optimization (cost, environmental impact)
Climate change scenario modeling
