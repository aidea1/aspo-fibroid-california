# Methods

## Study Design

Cross-sectional, county-level integration of:

1. Digital search interest (DMA-level)
2. Structural specialty care infrastructure (facility-level)

Unit of analysis: California county (N = 58)  
Time horizon: 2019–2025  


## Exposure: Search-Derived Interest

Annual DMA-level interest scores were mapped to counties via a county–DMA crosswalk.

For each county `c` and year `y`:

Interest(c, y) = Interest_DMA(d, y)

where `d` is the DMA associated with county `c`.

This assumes each county inherits the interest value of its mapped DMA.


## Structural Access Metric

For each county `c`, the distance to the nearest tertiary facility was computed using the haversine formula applied to geographic coordinates.

Distance(c) = min_j Haversine(centroid_c, facility_j)


where:

- `centroid_c` = latitude/longitude of county centroid  
- `facility_j` = latitude/longitude of facility `j`  
- distance measured in miles  


## Facility Classification

Facilities were categorized into:

- Primary  
- Secondary  
- Tertiary  

Counts were computed per county:

n_primary(c)
n_secondary(c)
n_tertiary(c)


## Mismatch Definition

For each year `y`:

Let:

Q75(y) = 75th percentile of county-level interest in year y


Define:

Mismatch(c, y) = 1 if:
Interest(c, y) ≥ Q75(y)
AND
Distance(c) > 50 miles

Otherwise 0


Threshold parameters:

- Interest threshold = 75th percentile (configurable)
- Distance threshold = 50 miles (configurable)


## Spatial Assignment

Facility-to-county assignment was performed using:

- County boundary GeoJSON (EPSG:4326)
- Point-in-polygon spatial containment

County centroids were derived from boundary geometry.

## Visualization Specification

Interactive HTML map includes:

- Choropleth: Distance to tertiary care (Turbo colorscale)
- Bubbles: Year-specific search interest at county centroids
- Facility markers: categorized by care level
- Dropdown: year selector (2019–2025)


## Sensitivity Extensions (Planned)

- Population-adjusted access metrics
- Road-network travel time instead of geodesic distance
- Multi-year rolling averages of interest
- Alternative quantile thresholds (e.g., Q80)
- Insurance-stratified facility layers

