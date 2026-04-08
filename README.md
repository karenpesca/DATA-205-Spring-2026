This is my capstone project for data 205.
# Broadband Availability & Infrastructure Analysis  
## Odessa / Midland & Wichita Falls, Texas



## Project Overview

This project analyzes broadband availability using the FCC Broadband Data Collection (BDC) dataset and compares it with physical network infrastructure from the FiberLocator API.

The objective is to understand how broadband service availability relates to underlying fiber infrastructure in selected Texas markets, with a focus on business-relevant connectivity.



## Objectives

- Identify dominant broadband providers in each study area
- Analyze distribution of broadband technologies (fiber vs cable)
- Evaluate provider density across geographic areas
- Compare FCC-reported availability with FiberLocator infrastructure
- Focus specifically on business and mixed-use service locations



## Study Areas

Due to the absence of city-level identifiers in the FCC dataset, counties were used as proxies:

- **Ector County (48135)** → Odessa  
- **Midland County (48329)** → Odessa metropolitan area  
- **Wichita County (48485)** → Wichita Falls  

Filtering was performed using the first five digits of the `block_geoid` field.


## Data Sources

### FCC Broadband Data Collection (BDC)
- 2 Data Sets: Cable and fiber premises tecnohlogy.

These datasets provide location-level broadband availability, including:
- Provider (brand_name)
- Technology
- Business vs residential classification
- H3 spatial identifiers


### FiberLocator API
Used to enrich the analysis with infrastructure data:
- Metro fiber networks
- Long-haul fiber routes
- Connected commercial buildings

This allows comparison between **reported service availability** and **actual network infrastructure**.


## Methodology

### 1. Data Ingestion
- Loaded FCC fiber premises and cable datasets from Google Drive
- Combined the 2 datasets into a unified dataset by for analysis



### 2. Data Cleaning & Preparation
- Filtered records using county FIPS codes via `block_geoid`
- Removed residential-only records (`R`)
- Retained:
  - Business (`B`)
  - Business/Residential (`X`)
- Standardized categorical fields for clarity


### 3. Provider Analysis
- Calculated provider frequency using `brand_name`
- Identified top providers by number of serviceable locations
- Visualized results using bar charts


### 4. Spatial Aggregation (H3)

To enable geographic analysis:
- Data was grouped by H3 hexagon (`h3_res8_id`)
- Aggregated attributes per hexagon:
  - Total locations
  - Unique providers
  - Technologies present
  - Business categories

This converts row-level data into spatial summaries.


### 5. Geometry Creation

- Converted H3 indices into polygon geometries
- Built a GeoDataFrame using Shapely and GeoPandas
- Exported to GeoJSON for mapping


### 6. Mapping & Visualization

Maps were created using Folium:

#### FCC Maps
- Display broadband availability by H3 hexagon
- Visualize provider density and geographic distribution

#### FiberLocator Maps
- Overlay:
  - Metro networks
  - Long-haul routes
  - Connected buildings



### 7. Comparative Analysis

The project compares:

| FCC Data | FiberLocator API |

- Service availability | Physical infrastructure 
- Provider presence | Network footprint 
- Location-level coverage | Network topology 


## Key Insights

- Provider presence is concentrated in specific hexagonal clusters
- Some areas show strong infrastructure but fewer reported providers
- Business-focused connectivity differs from general residential availability
- Midland plays a critical role in the Odessa metro broadband landscape


##  Tools & Technologies

- Python
  - Pandas
  - GeoPandas
  - Folium
  - H3
- Google Colab
- FiberLocator API

## Repository Structure

- `BDC_TEXAS_2026.ipynb` → Main analysis notebook
- `README.md` → Project documentation


## Notes

- FCC data does not include city-level identifiers, requiring geographic filtering via FIPS codes
- H3 indexing was used to standardize spatial aggregation
- FiberLocator data requires authentication via API


## Future Improvements

- Add statistical comparison between regions
- Incorporate speed tiers (if available)
- Improve visualization styling and interactivity (maps)
- Expand analysis to additional Texas markets

