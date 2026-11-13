# Intro-CIROH-Notes

### CIROH (Cooperative Institute for Research to Operations in Hydrology)

* Partnership between [National Oceanic and Atmospheric Administration (NOAA)](https://www.noaa.gov/) and The University of Alabama
*  A large group of scientists from universities, governments, and private sectors
* Goal: Advance hydrological forecasting through research and technical products

For more info about CIROH read more [HERE](https://ciroh.ua.edu/)

### The National Water Model (NWM)

* A model that simulates and forecasts water flow, snowmelt, soil moisture, and water levels across USA: including southern Alaska, Hawaii, Puerto Rico, and the U.S. Virgin Islands.
* Uses radar and gauge data alongside weather prediction models.
* 4 different types of forecasts

  * Analysis (Right Now) Every hour updates of real time hydrologic simulation

  * Short-Range are up to 18 hours out, high detail, hour by hour forecasts.
     - This forecast is used by emergency personnel use for
      a) targeted evacuations
      b) deploy sandbags
      c) close low-lying intersections before roadway flooding happens.

   * Medium-range (up to 10 days prediction) running multiple scenarios to show river predictions.
     - These predictions provide advance warning for city councils to allocate emergency budgets, clear storm drains, notify critical infrastructure, and request mutual aid from neighboring jurisdictions.

   * Long-Range (30 days out) big picture stuff used for droughts or long term high water. Decades of modeled streamflow history that can help city councils and urban planners to analyze historical flow patterns in order to
  
    a) establish flood-fringe zoning laws (laws about development in flood prone areas. "Strict floodway" is where building is banned to keep water moving, however flood fringe moderates allowed development. Usually requires floor elevation standards, floodproofing, landscaping restrictions, and mandatory Floodplain Development Permits).

    b) redesign culverts (water flow channels to drainage ditches).

    c) retention basins construction

    d) capital improvement projects.

☆ Analysis: An estimate of current hydrologic conditions, incorporating observations in configurations that assimilate data.

☆ Forecast: A prediction of future hydrologic conditions, initialized using an analysis or open-loop analysis.

* [BYU's role](https://hydroinformatics.byu.edu/national-water-model): Assisting in the creation of a HydroServer application to provide quickly accessible data from NWM (Next Gen). 


### The National Weather Service (NWS)

* Weather, water, climate data to provide forecasts, warnings, and data for decision making

### NWM Forecast Model Data Notes

* Http and FTP files are commonly used ways to transfer files and data across the web (hence the use of several online sources for the NWM)
* A Network Common Data Form format (NetCDF) is a file format for science related fields. They are  used for temperature, humidity and wind. 
* NetCDF files have the following configurations
     -  1km gridded NetCDF (land surface variables and forcing)
     -   250m gridded NetCDF (ponded water depth and depth to soil saturation)
     -   Point-type NetCDF (stream routing and reservoir variables)
     -   Point-type reservoir NetCDF (water surface elevation, inflow, outflow)

☆ Simple flowchart of file organization and usage

<img width="250" alt="mermaid-diagram" src="https://github.com/user-attachments/assets/c6f6e6bd-d910-40ad-ab5c-45a3985a5408" />



| Term | What it is | " for dummies " |
| --- | --- | --- |
| HTTP/FTP | A way to access/dowload files over the internet | a delivery method |
| NetCDF (.nc) | A scientific data file format | A container |
| chrt_out | Channel Routed Output data produced by the NWM | The contents of the container |
| Reach |	An individual section of a river  | A location |
| feature_id | Unique identifier assigned to a river reach | An ID number |
| streamflow | Volume of water flowing through a reach per unit of time | A measurement |
| Timestamp | The time associated with a data value | A time label |
| Forecast reference time | The time the forecast run was initialized | When the prediction started |
| Forecast lead time | How far into the future a prediction extends | Time elapsed since initialization |



* The NWM data is stored on files across several major cloud and data platforms.  
* Files on the noaa-nwm-pds Amazon S3 Bucket are organized by daily forecasts. Each forecast begins with the prefix
>nwm.yyyymmdd
where September 29th 2026 data would be on file as
>nwm.20260929
* More information about Google Cloud specific buckets can be found [HERE](https://docs.cloud.google.com/storage/docs/bucket-metadata)

### Tutorial: Finding and Viewing NWM Data 
*For this tutorial we will be using Google Cloud, accessing short range, and then looking at streamflow data points*
**Important:** NWM file naming conventions and available forecast products vary by model version and archive. Make sure you select a valid Channel Routed Output file that contains the `streamflow` variable.


#### **First** Access the [Google Cloud NWM Bucket HERE](https://console.cloud.google.com/storage/browser/national-water-model;tab=objects?utm_source=chatgpt.com&prefix=&forceOnObjectsSortingFiltering=false)
From here you should recognize the folder names format >nwm.yyyymmdd

After choosing a date, you will see 11 different folders. Lets take a quick look at what all of these folders are. 

| Folder | What it is |
| --- | --- |
| analysis_assim/ | Analysis and assimilation model output. Estimates of current hydrologic conditions. |
| forcing_analysis_assim/ | Weather data used to drive the analysis simulations. |
| forcing_medium_range/ | Meteorological forecast inputs used to drive Medium-Range simulations. |
| forcing_short_range/ | Meteorological forecast inputs used to drive Short-Range simulations. |
| long_range_mem1/ | Long-Range forecast output, ensemble member 1. |
| long_range_mem2/ | Long-Range forecast output, ensemble member 2. |
| long_range_mem3/ | Long-Range forecast output, ensemble member 3. |
| long_range_mem4/ | Long-Range forecast output, ensemble member 4. |
| medium_range/ | Medium-Range forecast output. |
| short_range/ | Short-Range forecast output. |
| usgs_timeslices/ | USGS streamflow observation data organized into time-based files |

* **Forcing data** is the weather information supplied to the model. Current precipitation, temperature, and weather forecasts are in these folders.
* With the forcing data, the model estimates current water conditions in the **Analysis Folder**.
For example, the forcing data provides the model with information that 2 inches of rain fell over a watershed. The analysis estimates that a nearby river flows at 150 cubic meters per second based on this information.
* The **Forecast Folder** then predicts what will happen next. These predictions are then organized into the long, medium and short range folders.
*  Ensembles are multiple versions of a forecast.
*  Reference [Water NOAA Web](https://water.noaa.gov/about/nwm?utm_source=chatgpt.com) and [Output File Contents](https://water.noaa.gov/about/output_file_contents) for more info on organization. 

**Next** 
Choose short_range/ for the purpose of this tutorial. There are then many many files to look at. Depending on your goal, here is another quick look at the different folders.

- each file starts with a format of "nwm.t00z" where the numerical value represents our forecast initialization.
  >nwm.t04z the forecast initialization at 04:00 UTC

- The next part of the filename tells us what output data it will have. 

| Folder | What it is |
| --- | --- |
| channel_rt | River/channel routing data |
|land | Land-surface conditions, including snow and soil moisture |
| reservoir | Reservoir and lake information, including inflow and outflow |
| terrain_rt | Terrain routing and water movement across the land surface |

- The next part of a filename is the forecast hour. The short-range configuration produces forecasts out to 18 hours.
- The last part of a filename is the geographic domain.
           - `conus` = Continental United State
           - Others domains can include Alaska, Hawaii, and Puerto Rico.
**Important:** The forecast hour is relative to the initialization time, not the calendar date. For example, a forecast initialized at 00:00 UTC with `f009` represents a forecast for 09:00 UTC.


### Example

Filename:

`nwm.t00z.short_range.channel_rt.f009.conus.nc`

| Filename Component | Meaning |
|---|---|
| `nwm` | National Water Model |
| `t00z` | Forecast initialization at 00:00 UTC |
| `short_range` | Short-range forecast configuration |
| `channel_rt` | Channel routing output |
| `f009` | Forecast hour 9 (9 hours after initialization) |
| `conus` | Contiguous United States domain |
| `.nc` | NetCDF file format |

For this exercise, download the following NetCDF file from the Google Cloud Storage bucket:

```text
nwm.t00z.short_range.channel_rt.f009.conus.nc
```

You can easily find it by `ctrl + c` and `ctrl + v` into the `filter objects and folders` bar. 

Proceed to download the file 

# Coding for Visual Streamflow Charts
## Tutorial: Opening and Visualizing National Water Model (NWM) Streamflow Data in Google Colab

This tutorial explains how to open National Water Model (NWM) NetCDF files, extract streamflow values for specific river locations, create visualizations, and connect NWM feature IDs to river locations using an interactive map.

The tutorial uses the Provo River in Utah as an example, focusing on the river reach from Deer Creek Reservoir to Utah Lake.

---

# Part 1: Set Up Google Colab

Google Colab is a browser-based environment that allows you to write and run Python code in a Jupyter Notebook without installing Python or setting up a local coding environment.

### 1.1 Open Google Colab

1. Go to [Google Colab](https://colab.research.google.com/).
2. Sign in with your Google account.
3. Select **New Notebook**.
4. Rename the notebook by clicking its title in the upper-left corner. For example:

   `NWM_Streamflow_Tutorial.ipynb`

5. Save the notebook to your Google Drive.

### 1.2 Install Required Packages

Google Colab already includes Python, but we need to install the packages used to read and visualize NWM data.

Create a code cell and run:

```python
!pip install xarray netCDF4 numpy pandas matplotlib requests folium
```

These packages provide the following functionality:

| Package | Purpose |
|---|---|
| `xarray` | Opens and analyzes NetCDF datasets |
| `netCDF4` | Supports reading NetCDF files |
| `numpy` | Handles numerical data |
| `pandas` | Organizes data into tables |
| `matplotlib` | Creates graphs and charts |
| `requests` | Retrieves data from online APIs |
| `folium` | Creates interactive maps |

**Note:** If the Colab runtime disconnects or resets, you may need to run the installation cell again.

---

# Part 2: Obtain an NWM NetCDF File

The National Water Model stores its output data in NetCDF files. These files contain information about modeled river conditions, including streamflow and velocity.

### 2.1 Locate an NWM File

For this tutorial, we will use an NWM short-range channel routing output file.

An example filename is:

`nwm.t00z.short_range.channel_rt.f009.conus.nc`

The filename provides information about the dataset:

| Filename component | Meaning |
|---|---|
| `nwm` | National Water Model |
| `t00z` | Forecast initialized at 00:00 UTC |
| `short_range` | Short-range forecast |
| `channel_rt` | Channel routing output |
| `f009` | Forecast hour 9 |
| `conus` | Continental United States |
| `.nc` | NetCDF file format |

### 2.2 Upload the NetCDF File to Google Colab

Download the desired NetCDF file from the Google Cloud NWM data bucket.

Once you have the file:

1. Open the **Files** panel on the left side of Google Colab.
2. Select the upload icon.
3. Choose your downloaded `.nc` file.
4. Wait for the upload to finish.

Alternatively, you can upload a file using Python:

```python
from google.colab import files

uploaded = files.upload()
```

Select the NetCDF file when the upload window appears.

**Important:** Uploaded files are stored temporarily in the Colab runtime. If the runtime disconnects or resets, you may need to upload the file again.

---

# Part 3: Open and Inspect the NetCDF Dataset

### 3.1 Import the Required Libraries

Create a new code cell and run:

```python
import xarray as xr
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

### 3.2 Open the NetCDF File

Set the filename to match the exact name of the file you uploaded.

```python
nc_name = "nwm.t00z.short_range.channel_rt.f009.conus.nc"

ds = xr.open_dataset(nc_name)

print(ds)
```

This opens the NetCDF file using `xarray` and displays an overview of the dataset.

### 3.3 Understand the Dataset Dimensions

The dataset contains several dimensions that organize the information.

| Dimension | Description |
|---|---|
| `feature_id` | Unique identifier for an individual river segment |
| `time` | Valid time of the forecast data |
| `reference_time` | Time when the forecast was initialized |

To display the dimensions:

```python
print(ds.dims)
```

To display the variables:

```python
print(ds.data_vars)
```

Some of the variables available in NWM channel routing output files include:

- `streamflow`
- `velocity`
- `nudge`
- `qSfcLatRunoff`
- `qBucket`
- `qBtmVertRunoff`

For this tutorial, we will focus on `streamflow`.

### 3.4 Inspect the Streamflow Variable

```python
print(ds["streamflow"])
```

This displays information about the streamflow variable, including its dimensions, attributes, and units.

NWM streamflow is measured in cubic meters per second:

**m³/s**

---

# Part 4: Extract Streamflow Values

### 4.1 Display the First 10 Streamflow Values

To get a quick look at the streamflow data:

```python
streamflow = ds["streamflow"].values.flatten()

print(streamflow[:10])
```

The `.values` command retrieves the numerical data, while `.flatten()` converts it into a one-dimensional array.

### 4.2 Organize the Data into a Table

We can use Pandas to display the streamflow values alongside their corresponding feature IDs.

```python
df = pd.DataFrame({
    "Feature ID": ds["feature_id"].values,
    "Streamflow (m3/s)": streamflow
})

print(df.head(10))
```

This creates a table showing the first 10 river segments and their corresponding streamflow values.

### 4.3 Select Specific Feature IDs

Instead of examining every river segment in the CONUS dataset, we can extract values for specific locations.

For this example, we will use selected feature IDs along the Provo River, from Deer Creek Reservoir toward Utah Lake.

```python
feature_ids = [
    10375840,
    10375896,
    10375924,
    10376654,
    10375950,
    10375954,
    10375972,
    10376656,
    10375992,
    10375994,
    946020369,
    946020370,
    10376032,
    10376584,
    10376040,
    10376080,
    10376596,
    10376192
]
```

### 4.4 Extract the Selected Streamflow Values

```python
selected_streamflow = ds["streamflow"].sel(
    feature_id=feature_ids
).values.flatten()

print(selected_streamflow)
```

The `.sel()` function selects the streamflow values associated with the specified feature IDs.

### 4.5 Check That the Feature IDs Exist

It is important to verify that all selected feature IDs are present in the dataset.

```python
available_ids = ds["feature_id"].values

valid_ids = [
    fid for fid in feature_ids
    if fid in available_ids
]

missing_ids = [
    fid for fid in feature_ids
    if fid not in available_ids
]

print("Valid Feature IDs:", valid_ids)
print("Missing Feature IDs:", missing_ids)
```

If a feature ID is missing, it may not be included in that particular dataset.

---

# Part 5: Create a Streamflow Bar Chart

We can visualize the extracted streamflow values using Matplotlib.

### 5.1 Create the Graphing Function

The following function creates a bar chart showing streamflow for each selected feature ID.

```python
def graph_streamflow(feature_ids, ds):

    streamflow = ds["streamflow"].sel(
        feature_id=feature_ids
    ).values.flatten()

    plt.figure(figsize=(14, 6))

    plt.bar(
        range(len(feature_ids)),
        streamflow
    )

    plt.xlabel("Feature ID")
    plt.ylabel("Streamflow (m³/s)")

    plt.xticks(
        range(len(feature_ids)),
        feature_ids,
        rotation=45
    )

    plt.title("NWM Streamflow – Provo River")

    plt.figtext(
        0.5,
        0.01,
        "Location: Deer Creek Reservoir to Utah Lake | "
        "Variable: Streamflow | Units: m³/s",
        ha="center",
        fontsize=9
    )

    plt.grid(axis="y", alpha=0.3)
    plt.tight_layout(rect=[0, 0.05, 1, 1])

    plt.show()
```

### 5.2 Generate the Graph

Run the function using the selected feature IDs:

```python
graph_streamflow(feature_ids, ds)
```

The resulting graph displays the streamflow values for the selected river segments.

Each bar represents one feature ID, allowing us to compare modeled streamflow at different locations along the river.

---

# Part 6: Export the Extracted Data

The extracted streamflow values can also be saved as a CSV file for additional analysis.

### 6.1 Create a DataFrame

```python
streamflow_df = pd.DataFrame({
    "Feature ID": feature_ids,
    "Streamflow (m3/s)": selected_streamflow
})

print(streamflow_df)
```

### 6.2 Export the CSV

```python
streamflow_df.to_csv(
    "selected_streamflow.csv",
    index=False
)
```

### 6.3 Download the CSV from Colab

```python
from google.colab import files

files.download("selected_streamflow.csv")
```

This downloads the CSV file to your computer.

---

# Part 7: Connect Feature IDs to River Locations

The feature IDs used in the NWM dataset correspond to individual river segments in the National Water Model's river network.

To understand where these segments are located, we can retrieve river flowline information from NOAA's National Water Model reference flowlines service.

This allows us to visualize the river network and identify the feature IDs associated with individual river segments.

### 7.1 Retrieve River Flowline Data

We will use NOAA's ArcGIS REST API:

[NOAA Static NWM Flowlines Feature Service](https://maps.water.noaa.gov/server/rest/services/reference/static_nwm_flowlines/FeatureServer/0/query)

First, import the required libraries:

```python
import requests
import folium
```

### 7.2 Define the Study Area

The following bounding box covers the Provo River study area.

```python
bbox = "-111.90,40.10,-111.40,40.50"
```

The coordinates represent:

- Minimum longitude: -111.90
- Minimum latitude: 40.10
- Maximum longitude: -111.40
- Maximum latitude: 40.50

### 7.3 Query NOAA's Flowline Service

```python
url = (
    "https://maps.water.noaa.gov/server/rest/services/"
    "reference/static_nwm_flowlines/FeatureServer/0/query"
)

params = {
    "geometry": bbox,
    "geometryType": "esriGeometryEnvelope",
    "spatialRel": "esriSpatialRelIntersects",
    "outFields": "feature_id,name,nwm_vers,strm_order",
    "returnGeometry": "true",
    "outSR": "4326",
    "f": "geojson"
}

response = requests.get(url, params=params)

data = response.json()

print("Number of flowlines:", len(data["features"]))
```

This retrieves flowline information within the specified bounding box.

### 7.4 Create an Interactive Map

We can use Folium to display the flowlines on an interactive map.

```python
m = folium.Map(
    location=[40.2338, -111.6585],
    zoom_start=11,
    tiles="OpenStreetMap"
)

folium.GeoJson(
    data,
    name="NWM Flowlines",
    style_function=lambda feature: {
        "color": "blue",
        "weight": 2
    },
    tooltip=folium.GeoJsonTooltip(
        fields=["feature_id", "name", "strm_order"],
        aliases=["Feature ID:", "River Name:", "Stream Order:"]
    )
).add_to(m)

folium.LayerControl().add_to(m)

display(m)
```

The map displays river flowlines within the study area.

You can zoom in, move around the map, and click individual river segments to view their associated information.

---

# Part 8: Understanding the Connection Between the Map and Streamflow Data

The interactive map and the NetCDF dataset provide two different ways of examining the National Water Model.

**The map provides geographic information:**
- Where individual river segments are located.
- The feature ID associated with each segment.
- Additional flowline information, such as river name and stream order.

**The NetCDF file provides modeled information:**
- Streamflow for each feature ID.
- Velocity for each feature ID.
- Other channel routing variables.

The `feature_id` is the connection between these two datasets.

For example, if a river segment on the map has feature ID `10375840`, we can use that same ID to retrieve its streamflow value from the NetCDF file.

This makes it possible to connect the geographic location of a river segment with its modeled hydrologic conditions.

---

# Part 9: Potential Next Steps

Once the basic workflow is working, the notebook can be expanded to support additional analysis.

Possible next steps include:

1. **Compare multiple years:** Open NetCDF files from different years and compare streamflow at the same feature IDs.
2. **Create time-series graphs:** Examine how streamflow changes over time at selected river locations.
3. **Compare additional variables:** Examine velocity and other channel routing variables alongside streamflow.
4. **Improve the interactive map:** Display selected feature IDs and their corresponding streamflow values when a river segment is clicked.
5. **Automate data retrieval:** Retrieve NetCDF files directly from the Google Cloud bucket instead of manually downloading and uploading them.

These additions can help build a more flexible workflow for examining historical NWM forecasts and comparing modeled river conditions across different locations and dates.

---

## Summary

This tutorial demonstrated how to:

- Set up a Google Colab notebook.
- Install and import the required Python packages.
- Upload and open an NWM NetCDF file.
- Inspect dataset dimensions and variables.
- Extract streamflow values for selected feature IDs.
- Create a streamflow bar chart.
- Export streamflow data to a CSV file.
- Retrieve NOAA river flowline information.
- Display an interactive map connecting feature IDs to geographic river locations.

The overall goal is to create a reproducible workflow that connects NWM forecast data with geographic river information, allowing streamflow conditions to be examined at specific locations along a river network.
