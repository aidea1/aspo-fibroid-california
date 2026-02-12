# Data Dictionary

## `final_county_fibroid_access_interest_APSO.csv`


## Identifiers

**county_fips**
Five-digit Federal Information Processing Standard (FIPS) code uniquely identifying each California county.
Primary geographic key used for merging spatial, DMA, and facility data.

**county_name**
Official California county name.


## Geographic Attributes

**centroid_lat**
Latitude of county centroid (WGS84 / EPSG:4326).

**centroid_lng**
Longitude of county centroid (WGS84 / EPSG:4326).

Centroids are derived from county boundary geometries and are used for:

* Distance calculations
* County-level visualization
* Bubble placement on the HTML map


## Facility Counts (Structural Capacity)

Facilities were classified into three care levels based on service scope.

**n_primary**
Number of primary-level facilities located within county boundaries.

**n_secondary**
Number of secondary-level facilities located within county boundaries.

**n_tertiary**
Number of tertiary-level facilities located within county boundaries.

Facility-to-county assignment was performed using point-in-polygon spatial containment against California county boundaries (EPSG:4326).


## Distance Metrics (Geodesic)

Distances are computed using the haversine (great-circle) formula between county centroid and facility coordinates.

**dist_any_mi**
Minimum geodesic distance (miles) from county centroid to the nearest facility of any care level.

Definition:

```
dist_any(c) = min_j Haversine(centroid_c, facility_j)
```


**dist_tertiary_mi**
Minimum geodesic distance (miles) from county centroid to the nearest tertiary facility.

Definition:

```
dist_tertiary(c) = min_j Haversine(centroid_c, tertiary_facility_j)
```

Notes:

* Units: miles
* Distance is straight-line (not road-network travel time)
* All coordinates use WGS84


## DMA Mapping

Counties are mapped to Nielsen Designated Market Areas (DMAs) using a county–DMA crosswalk.

Each county inherits the DMA-level search interest associated with its DMA.

```
Interest(c, y) = Interest_DMA(d, y)
```

Where:

* c = county
* d = DMA assigned to county c
* y = year

This produces county-level digital demand signals derived from DMA-level search data.


## Search Interest Variables

**2019 – 2025**

Annual DMA-derived search interest values mapped to counties.

* Relative index scale
* Comparable within-year
* Interpreted as digital demand intensity


## Mismatch Indicators

Binary indicator identifying structural access–demand discordance.

For each year y:

```
Mismatch(c, y) = 1 if:
    Interest(c, y) ≥ Q75(y)
    AND
    dist_tertiary(c) > 50 miles

Else:
    0
```

Where:

* Q75(y) = 75th percentile of county-level interest distribution in year y
* Distance threshold = 50 miles
* Distance metric used = dist_tertiary_mi

Interpretation:

1 → High digital demand and limited structural access
0 → Otherwise

This indicator flags counties potentially experiencing unmet structural need.

## Derived Concepts

**Structural Access**
Operationalized as geodesic distance to tertiary care.

**Digital Demand**
Operationalized as DMA-derived annual search interest.

**Access–Demand Mismatch**
Defined as simultaneous high demand (top quartile) and limited tertiary access (>50 miles).


## Technical Notes

* All spatial coordinates are WGS84 (EPSG:4326).
* Distance is geodesic, not travel-time based.
* DMA mapping is one-to-many (multiple counties per DMA).
* Thresholds (75th percentile; 50 miles) are configurable parameters in the APSO pipeline.
* Dataset includes all 58 California counties.

---

If you want, I can now generate a concise “Methodological Assumptions & Limitations” section to append directly below this in GitHub so the repository reads at a reviewer-ready level.
