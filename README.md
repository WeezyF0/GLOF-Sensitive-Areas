# GLOF-Sensitive-Areas

This project identifies anomalous regions (i.e., areas at risk of Glacial Lake Outburst Floods) by analyzing environmental factors such as:

- **Snow Albedo** (reflectivity of ice)
- **Snow Cover**
- **Lake Bottom Temperature**
- **Total Precipitation**
- **Volumetric Soil Moisture**
- **Dew Point**

---

## Overview

1. **Data Source**  
   All climate variables are sourced from the Copernicus ERA5 single‐level reanalysis dataset:  
   [ERA5 Single-Levels on Copernicus Climate Data Store](https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels?tab=overview)

2. **Objective**  
   Classify glacial regions into two categories:
   - **RISKY** (potential for GLOF)
   - **NOT RISKY** (low likelihood of outburst)

3. ## Methodology

1. **Data Loading & Preprocessing**  
   - Load `Glof.csv` containing six environmental features (`Snow_Albedo`, `snow_cover`, `Lake_bottom_temperature`, `total_precipitation`, `volumetric_soil`, `dew_point`) along with `Latitude` and `Longitude`.  
   - Handle missing values and (optionally) normalize each feature.

2. **Anomaly Detection (Isolation Forest)**  
   - Fit an `IsolationForest` on the six features.  
   - Label points with a prediction of `-1` as **RISKY** (anomalies) and `1` as inliers.

3. **Clustering (K-Means)**  
   - Fit `KMeans(n_clusters=2)` on the same features.  
   - Assign each inlier to one of two clusters. Determine which cluster aligns with high-risk characteristics by comparing centroids or overlap with Isolation Forest outliers.

4. **Combine Results**  
   - Any point flagged by Isolation Forest remains **RISKY**.  
   - For the remaining points, assign **RISKY** to the cluster overlapping anomalies; label the other cluster **NOT RISKY**.

5. **Visualization & Output**  
   - Plot `Latitude` vs. `Longitude` color-coded by final labels.  
   - Export a geospatial risk map (e.g., GeoJSON or PNG) highlighting all **RISKY** regions.

---
