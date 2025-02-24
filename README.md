# Rental Prices Analysis in Melbourne

## Research Question
This study investigates how proximity to landmarks (such as universities, hospitals, stations), crime rates, and land use patterns affect rental prices in different areas of Melbourne.

## Scope
The data covers the Greater Melbourne area in the year 2021.

## Data Sources
1. **Census Data**: Sourced from the "Census 2021 table G02 Victoria.gpkg," this dataset includes rental prices, median weekly rent, and socio-economic indicators for the Melbourne area.  
   Source: [ABS Census 2021](https://www.abs.gov.au/census/find-census-data/geopackages?release=2021&geography=VIC&topic=HIHC&gda=GDA2020)

2. **GIS Data**: From "Greater Melbourne (ABS) study area.shp," this dataset defines the geographical boundaries of the Greater Melbourne area.  
   Source: [ABS GIS Data](https://www.abs.gov.au/statistics/standards/australian-statistical-geography-standard-asgs-edition-3/jul2021-jun2026/access-and-downloads/digital-boundary-files)

3. **Landmark Data**: Obtained via the OpenStreetMap API, this dataset includes geographical location data for universities, hospitals, and train stations.  
   Source: [OpenStreetMap API](https://www.openstreetmap.org/relation/4246124#map=11/-37.7870/145.0140)

4. **Crime Data**: Extracted from "Data_Tables_LGA_Criminal_Incidents_Year_Ending_December_2023.xlsx," this dataset contains records of criminal incidents by postcode for the year 2021.  
   Source: [Crime Statistics Victoria](https://www.crimestatistics.vic.gov.au/crime-statistics/latest-victorian-crime-data/download-data)

5. **Land Use Raster Data**: Provided by the "55H_20210101-20220101.tif" and "55H_20210101-20220101_clipped.tif," these datasets offer information on land cover types within the study area.  
   Source: [Living Atlas](https://livingatlas.arcgis.com/landcoverexplorer/#mapCenter=39.18600%2C9.04200%2C10&mode=step&timeExtent=2017%2C2022&year=2020&downloadMode=true)

## Methodology

This study employs a range of data analysis techniques, including spatial econometrics and machine learning models, to explore the determinants of rental prices in Melbourne.

### Key Scenarios
1. **Landmarks**: The study analyzes how proximity to universities, hospitals, and train stations affects rental prices through correlation analysis and spatial autocorrelation methods, including Moran's I and LISA.
2. **Land Use**: The impact of different land cover types on rental prices is examined using unweighted and weighted overlay analyses, regression analysis, and K-means clustering.
3. **Crime**: The relationship between crime rates and rental prices is explored using spatial analysis, correlation coefficients, and regression models.

### Statistical Techniques
- **OLS Regression**: To determine linear relationships between rent and various factors.
- **Spatial Lag Model**: To adjust for spatial autocorrelation in residuals.
- **K-means Clustering**: For identifying patterns in land use and rent data.
- **Moran's I**: For global and local spatial autocorrelation analysis.

## Results

### Landmark Analysis
- **Universities**: Areas with more universities generally have higher rent levels, especially in central urban areas.
- **Hospitals**: The proximity to hospitals shows a moderate effect on rental prices, with some clustering of higher rent areas around medical precincts.
- **Stations**: Stations have a more significant impact on rental prices, with a clear trend indicating higher rent near public transport hubs.

### Land Use Analysis
- **Unweighted Analysis**: Land use types such as built-up areas and water regions showed higher average rents, whereas areas like flooded vegetation and clouds showed lower rents.
- **Weighted Analysis**: Built-up areas had the most significant contribution to the overall rent levels due to both their size and higher rent prices. 

### Crime Analysis
- **Correlation**: Crime rates show a weak positive correlation with rental prices, especially in high-density areas, which tend to have higher rents and crime rates.

## Code Example
```python
import geopandas as gpd
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.linear_model import OLS
from sklearn.model_selection import train_test_split

# Example for analyzing the relationship between university proximity and rent
universities = landmarks_gdf[landmarks_gdf['type'] == 'university']
university_joined = gpd.sjoin(census_polygons, universities, how="left", predicate="intersects")
university_counts = university_joined.groupby(university_joined['index']).size()

# Correlation between university count and median rent
correlation_uni = census_polygons['Median_rent_weekly'].corr(census_polygons['university_count'])
print(f"Correlation between university count and median weekly rent: {correlation_uni}")
