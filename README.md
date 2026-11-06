
## Created an Interactive Map to Find Feature ID's
``` python
import requests

# --- Define the API endpoint ---
# This is NOAA's ArcGIS Feature Service for NWM Flowlines, Layer 0
url = "https://maps.water.noaa.gov/server/rest/services/reference/static_nwm_flowlines/FeatureServer/0/query"

# --- Define a bounding box around the Provo River ---
# This covers Deer Creek Dam down to Utah Lake
# Format: xmin, ymin, xmax, ymax (longitude/latitude in decimal degrees)
bbox = "-111.90,40.10,-111.40,40.50"

# --- Set query parameters ---
params = {
    "where": "1=1",           # Return all features in the area (no name filter yet)
    "geometry": bbox,          # Only return features inside our bounding box
    "geometryType": "esriGeometryEnvelope",  # The bounding box shape type
    "inSR": "4326",            # Input coordinate system (standard lat/lon)
    "outSR": "4326",           # Output coordinate system (standard lat/lon)
    "outFields": "*",          # Return ALL available fields so we can see what exists
    "f": "geojson",            # Return data in GeoJSON format (easy to work with)
    "resultRecordCount": 5     # Only get 5 records for now — just a quick test
}

# --- Send the request ---
print("Sending request to NOAA API...")
response = requests.get(url, params=params)

# --- Check if it worked ---
print(f"Status code: {response.status_code}")  # 200 means success

if response.status_code == 200:
    data = response.json()
    
    # How many features came back?
    features = data.get("features", [])
    print(f"\nNumber of features returned: {len(features)}")
    
    # Look at the first feature
    if features:
        first = features[0]
        print("\n--- Fields available in the data ---")
        for key, value in first["properties"].items():
            print(f"  {key}: {value}")
else:
    print("Request failed. Check the URL and your internet connection.")
```

and 
```python
import requests
import folium

# --- Query the NOAA API for ALL river segments in our area ---
url = "https://maps.water.noaa.gov/server/rest/services/reference/static_nwm_flowlines/FeatureServer/0/query"

bbox = "-111.90,40.10,-111.40,40.50"

params = {
    "where": "1=1",
    "geometry": bbox,
    "geometryType": "esriGeometryEnvelope",
    "inSR": "4326",
    "outSR": "4326",
    "outFields": "feature_id,name,nwm_vers,strm_order",  # Only the fields we need
    "f": "geojson",
    "resultRecordCount": 1000   # Get up to 1000 segments
}

print("Fetching river data...")
response = requests.get(url, params=params)
data = response.json()
features = data.get("features", [])
print(f"Retrieved {len(features)} river segments")

# --- Build the Folium map ---
# Center on Provo, Utah
provo_lat, provo_lon = 40.2338, -111.6585

m = folium.Map(location=[provo_lat, provo_lon], zoom_start=11)

# --- Add each river segment to the map ---
for feature in features:
    props = feature["properties"]
    geom  = feature["geometry"]

    # Build the popup text shown when you click a segment
    popup_text = f"""
    <b>Feature ID:</b> {props.get('feature_id', 'N/A')}<br>
    <b>Name:</b> {props.get('name', 'N/A')}<br>
    <b>NWM Version:</b> {props.get('nwm_vers', 'N/A')}<br>
    <b>Stream Order:</b> {props.get('strm_order', 'N/A')}
    """

    folium.GeoJson(
        feature,
        style_function=lambda x: {
            "color": "steelblue",
            "weight": 2,
            "opacity": 0.7
        },
        popup=folium.Popup(popup_text, max_width=250)
    ).add_to(m)

# --- Display the map ---
print("Map ready — click any blue river segment to see its feature ID.")
m
```
*Error* The NWM versions do not match. 
<img width="901" height="586" alt="image" src="https://github.com/user-attachments/assets/ca034957-4f8b-472a-af08-1a760b7381f7" />
<img width="890" height="570" alt="image" src="https://github.com/user-attachments/assets/d372c053-f7fb-487a-80d4-0360484d3a66" />


## Feature IDs
Deer Creek to Utah Lake through Provo River Feature IDs

10375840, 10375896, 10375924, 10376654, 10375950, 10375954, 10375972, 10376656, 10375992, 10375994, 946020369, 946020370, 10376032, 10376584, 10376040, 10376080,  10376596, 10376192, 

<img width="1190" height="590" alt="image" src="https://github.com/user-attachments/assets/97ed71fd-eef9-4f62-b98d-ef11533b6f23" />


I then used the same short_range.channel_rt.f009.conus files to compare Sept 17th 2018 and 2019
## Streamflow Comparison: 2018 vs. 2019

| Segment # | Feature ID | Streamflow 2018 (m³/s) | Streamflow 2019 (m³/s) |
|---:|---:|---:|---:|
| 1 | 10375840 | 2.3300 | 0.0000 |
| 2 | 10375896 | 3.2600 | 0.0600 |
| 3 | 10375924 | 3.6900 | 0.0600 |
| 4 | 10376654 | 5.3200 | 0.1000 |
| 5 | 10375950 | 5.3900 | 0.1000 |
| 6 | 10375954 | 5.5100 | 0.1500 |
| 7 | 10375972 | 6.4100 | 0.1500 |
| 8 | 10376656 | 6.7200 | 0.1500 |
| 9 | 10375992 | 6.8300 | 0.1500 |
| 10 | 10375994 | 6.8900 | 0.1600 |
| 11 | 946020369 | 7.0400 | 0.1700 |
| 12 | 946020370 | 7.1300 | 0.1700 |
| 13 | 10376032 | 7.6800 | 0.1800 |
| 14 | 10376584 | 7.7300 | 0.1800 |
| 15 | 10376040 | 7.7900 | 0.1800 |
| 16 | 10376080 | 7.9100 | 0.1800 |
| 17 | 10376596 | 7.3000 | 1.8700 |
| 18 | 10376192 | 5.3000 | 3.7500 |

**Data notes:**
- Streamflow is measured in cubic meters per second (m³/s).
- 2018 dataset: NWM 1.2.
- 2019 dataset: NWM 2.0.
- Both datasets represent forecast hour 9 (f009), with a valid time of 09:00 UTC.

**Research limitation:** Because the two datasets use different NWM versions, differences may reflect changes in model configuration as well as actual hydrologic conditions. These results should be interpreted with caution.
<img width="1390" height="633" alt="image" src="https://github.com/user-attachments/assets/ba9141ff-e046-47d5-9a99-ade5ac6946a2" />

### Next Steps
compare days using the same NWM version.
