## Adaptive Population Signal Observatory (APSO): Linking Uterine Fibroid Search Behavior to Geographic Access to Care in California ##

Motivation
Uterine fibroids represent a high-burden women’s health condition characterized by delayed diagnosis, fragmented care pathways, and substantial geographic inequities in access to specialty services. Traditional epidemiologic indicators - claims, encounters, or procedure data - capture utilization only after patients successfully navigate structural barriers to care. As a result, unmet need remains largely invisible until late in the disease course.
This project evaluates whether population-level digital search behavior, when aligned with geographic access metrics, can function as an earlier, observable signal of unmet health care need. We frame this work as a proof-of-concept for an Adaptive Population Signal Observatory (APSO): a closed-loop digital epidemiology framework that continuously links population concern, structural capacity, and access gaps in near real time.

Dashboard: https://aidea1.github.io/aspo-fibroid-california/

This repository presents a county-level proof-of-concept implementation of the Adaptive Population Signal Observatory (APSO) framework.

The objective is to identify geographic areas in California where elevated population search behavior for uterine fibroid–related care co-occurs with limited geographic access to tertiary specialty services.

This is a transparent, decision-grade digital epidemiology prototype designed for methodological clarity and extension.


## Core Research Question

Where in California does high search-derived interest in fibroid care coincide with long travel distance to tertiary facilities?


## Study Design Overview

Unit of analysis: **California county (N = 58)**  
Time frame: **2019–2025**  
Design: Cross-sectional county-level integration of digital interest signals and structural care capacity.


## Data Components

### 1. Google Search Interest
- DMA-level annual interest values (2019–2025)
- Standardized and cleaned
- Mapped to counties via DMA crosswalk

### 2. Categorized Fibroid Care Facilities
Each facility classified as:
- Primary
- Secondary
- Tertiary

Facilities assigned to counties via spatial point-in-polygon methods.

### 3. County–DMA Crosswalk
Enables aggregation of DMA-level search interest to county-level estimates.


## Analytical Workflow

The APSO pipeline consists of six structured stages:

### Step 1 — DMA Interest Cleaning
- Standardize DMA codes
- Harmonize annual interest columns
- Normalize DMA names

### Step 2 — Facility Spatial Assignment
- Assign each facility to a county (WGS84)
- Validate coordinate bounds
- Retain care level and geolocation

### Step 3 — County Access Metrics
For each county:
- Count of Primary facilities
- Count of Secondary facilities
- Count of Tertiary facilities
- Distance (miles) to nearest facility (any level)
- Distance (miles) to nearest tertiary facility

Distance computed using the haversine formula.

### Step 4 — County × DMA Mapping
- Join counties to DMA via crosswalk
- Merge DMA-level interest into county dataset

### Step 5 — Mismatch Indicator Definition

For each year:

Mismatch =  
County interest ≥ 75th percentile  
AND  
Distance to tertiary care > 50 miles

Thresholds are explicitly parameterized.

### Step 6 — Interactive Visualization
The final output is a self-contained HTML map showing:

- County choropleth (distance to tertiary care)
- Interest bubbles at county centroids
- Facility markers (categorized)
- Year selector (2019–2025)

## Repository Structure

---

## Outputs

### 1. County-Level Dataset

`data/final_county_fibroid_access_interest_APSO.csv`

Includes:
- County FIPS
- County name
- Centroid coordinates
- Facility counts by level
- Distance metrics
- Annual interest values (2019–2025)
- Year-specific mismatch indicators

### 2. Interactive Map

`outputs/CA_fibroid_access_interest_county_APSO.html`

Fully portable. No server required.

---

## Observational Pattern (Pilot Signal)

The pilot reveals:

- Concentration of tertiary care in major metropolitan regions.
- Extended travel distances for interior counties.
- Persistent high-interest counties with limited specialty proximity.

This supports the hypothesis that digital interest signals can highlight potential geographic access gaps.

---

## Why APSO Matters

APSO operationalizes:

Digital demand signals × Structural capacity metrics  
→ Actionable geographic diagnostics.

The framework is:
- Parameter-transparent
- Condition-agnostic
- Scalable
- Reproducible

Fibroids serve as a proof-of-concept condition.

## Contact

Akshaya Bhagavathula, PhD, Associate Professor of Epidemiology,
North Dakota State University, Fargo, ND 

Email: akshaya.bhagavathula@ndsu.edu 

Adaptive Population Signal Observatory
