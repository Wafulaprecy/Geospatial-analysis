# NYC Taxi Geospatial Analysis

## Project Overview
This project focuses on geospatial analysis of NYC taxi data using OpenStreetMap data and NYC Yellow Taxi trip records. The goal is to visualize and analyze taxi trip patterns, identify hotspots, and build predictive models for demand forecasting.

## Technologies Used
- **Python**: Main programming language
- **Geopandas**: Handling geospatial data
- **Folium**: Creating interactive maps
- **Pandas**: Data manipulation and analysis
- **OSMnx**: Downloading and analyzing OpenStreetMap (OSM) data

## Datasets
### **1. NYC Yellow Taxi Trip Data**
- Downloaded from Kaggle: [`NYC Yellow Taxi Trip Data`](https://www.kaggle.com/datasets/elemento/nyc-yellow-taxi-trip-data)
- Contains pickup and drop-off locations, timestamps, passenger counts, trip distances, and fare details.
- After downloading, extract the ZIP file and load the CSV:
  ```python
  import pandas as pd
  df = pd.read_csv("nyc_taxi_data/yellow_tripdata_2018-01.csv")
  df.head()
  ```

### **2. OpenStreetMap (OSM) Data**
- Extracted using `osmnx` to get NYC's road network.
- Example code to download and save the data:
  ```python
  import osmnx as ox
  place = "New York City, New York, USA"
  G = ox.graph_from_place(place, network_type="drive")
  ox.save_graphml(G, "nyc_street_network.graphml")
  ```

## Data Preprocessing
- Convert taxi pickup and dropoff points into a GeoDataFrame:
  ```python
  import geopandas as gpd
  from shapely.geometry import Point
  
  df["geometry"] = gpd.points_from_xy(df["pickup_longitude"], df["pickup_latitude"])
  gdf_pickup = gpd.GeoDataFrame(df, geometry="geometry", crs="EPSG:4326")
  ```

## Visualization
### **Plot NYC Taxi Pickup Locations on a Map**
```python
import folium

m = folium.Map(location=[40.7128, -74.0060], zoom_start=12)

for _, row in gdf_pickup.sample(100).iterrows():  # Sample 100 points for better performance
    folium.CircleMarker(
        location=[row.geometry.y, row.geometry.x],
        radius=2,
        color="blue",
        fill=True,
        fill_opacity=0.5
    ).add_to(m)

m.save("nyc_taxi_map.html")
```

## Next Steps
1. **Cluster Analysis**: Identify taxi demand hotspots using K-Means.
2. **Demand Prediction**: Train a model to predict taxi trip demand based on historical data.
3. **Route Optimization**: Use OpenStreetMap to analyze the most efficient taxi routes.

## Installation & Setup
To run this project, install dependencies:
```bash
pip install pandas geopandas folium osmnx shapely
```

## How to Use
1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/nyc-taxi-geospatial-analysis.git
   cd nyc-taxi-geospatial-analysis
   ```
2. Download datasets (from Kaggle and OpenStreetMap).
3. Run the analysis scripts:
   ```bash
   python analysis.py
   ```
4. Open `nyc_taxi_map.html` to view the interactive map.

## License
This project is licensed under the MIT License.
```

