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

### Coding for Visual Streamflow Chart
#### Tutorial: Opening and Visualizing National Water Model (NWM) Streamflow Data

This next part of the tutorial walks through how to open a National Water Model (NWM) NetCDF file, inspect its contents, extract streamflow values, and create a bar chart using Python.

The section is designed for beginners and uses Visual Studio Code (VS Code), Jupyter Notebook, and several Python packages.

**End goal:** Create a chart showing NWM-predicted streamflow for selected river feature IDs.

---

### Part 1: Prepare Your Environment

#### 1.1 Download the required software

Before starting, make sure you have the following installed:

| Software | Purpose | Download |
|---|---|---|
| Python | Programming language used to process the data. | [Download Python](https://www.python.org/downloads/) |
| Visual Studio Code | Code editor used to run Python and Jupyter Notebooks. | [Download VS Code](https://code.visualstudio.com/) |

##### VS Code extensions

Open VS Code and select the Extensions icon in the left sidebar.

Search for and install:

- **Python** — Microsoft
- **Jupyter** — Microsoft

These extensions allow you to write and execute Python code in notebook cells directly inside VS Code.

#### 1.2 Download the required Python packages

Python packages provide additional functionality that is not included in the basic Python installation.

The following packages are needed:

| Package | Purpose |
|---|---|
| `xarray` | Opens and works with NetCDF datasets. |
| `netCDF4` | Supports reading NetCDF files. |
| `numpy` | Performs numerical operations. |
| `pandas` | Organizes data into tables. |
| `matplotlib` | Creates charts and visualizations. |
| `ipykernel` | Allows Jupyter Notebook to run Python code. |

##### Install the packages

1. Open VS Code.
2. Select **Terminal** in the top menu.
3. Select **New Terminal**.
4. Copy and paste the following command into the terminal.

```powershell
python -m pip install xarray netCDF4 numpy pandas matplotlib ipykernel
```

Press Enter and allow the packages to install.

**What this does:** Installs all the Python libraries needed to open, process, and visualize NWM data.

#### 1.3 Create a project folder

In your file explorer app, create a new folder for your project.

For example:

```text
hydroinformatics_research/
```

Open this folder in VS Code:

1. Select **File**.
2. Select **Open Folder**.
3. Find and select your new project folder.

Keeping your notebook and data file in the same folder makes it easier for Python to locate the NetCDF file.

## 1.4 Create a Jupyter Notebook

1. In VS Code, open the Command Palette using `Ctrl + Shift + P`.
2. Search for `Create: New Jupyter Notebook`.
3. Select the command.
4. Click File in the top menu.
    - Select Save As...
     - Navigate to the project folder you created.
5. Save the notebook with a name such as:

```text
nwm_streamflow_tutorial.ipynb
```

A Jupyter Notebook allows you to run Python code in separate cells and view the results directly below each cell.

---

### Part 2: Obtain an NWM NetCDF File
*If you have already completed the previous setup steps and are repeating this tutorial on the same computer, you can skip ahead to this section.*

#### 2.2 Organize your project files

Through your file explorer, place the `nwm.t00z.short_range.channel_rt.f009.conus.nc` NetCDF file in the same folder as your Jupyter Notebook.

On VS Code *Explorer* side panel, your project should look similar to this:

```text
hydroinformatics_research/
│
├── nwm_streamflow_tutorial.ipynb
│
└── nwm.t00z.short_range.channel_rt.f009.conus.nc
```

This allows the notebook to reference the file by its filename without needing a full computer-specific path.

Note: If you are using a different date or forecast file, make sure to use the corresponding NetCDF file throughout the tutorial
---

### Part 3: Open and Inspect the NetCDF File

#### 3.1 Import the required packages

In the first notebook cell, copy and paste:

```python
import xarray as xr
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

Run the cell using `Shift + Enter`.
You will then be prompted to select a kernel. Select *Python Environment*. If you have multiple Python versions, choose the one you used when running the package installation command. Following this, click on your first notebook cell and rerun it (by using 'Shift + Enter')

**What this does:** Imports the Python packages used throughout the tutorial.

#### 3.2 Open the NetCDF file

In a new notebook cell, copy and paste:

```python
nc_name = "nwm.t00z.short_range.channel_rt.f009.conus.nc"

ds = xr.open_dataset(nc_name)

print(ds)
```

Run the cell.

**What this does:**

- `nc_name` stores the name of the NetCDF file.
- `xr.open_dataset()` opens the file.
- `ds` stores the resulting dataset.
- `print(ds)` displays the dataset's structure.

You should see information about the dimensions, coordinates, variables, and metadata.

#### 3.3 Understand the dataset dimensions

An example NWM Channel Routed Output dataset may contain:

```text
Dimensions:
    feature_id: 2729077
    time: 1
    reference_time: 1
```

These dimensions tell us how the data are organized.

| Dimension | Meaning |
|---|---|
| `feature_id` | Individual river or stream channel identifiers. |
| `time` | Forecast valid time. |
| `reference_time` | Forecast initialization/reference time. |

The `feature_id` dimension is particularly important because it allows us to identify streamflow values associated with specific river segments.

#### 3.4 Inspect the available variables

To see the variables stored in the dataset:

```python
print(ds.data_vars)
```

Common variables in Channel Routed Output files include:

- `streamflow`
- `nudge`
- `velocity`
- `qSfcLatRunoff`
- `qBucket`
- `qBtmVertRunoff`

For this tutorial, we will focus on `streamflow`.

#### 3.5 Inspect the streamflow variable

Copy and paste:

```python
print(ds["streamflow"])
```

The output should identify:

- The variable name.
- Its dimensions.
- Its data type.
- Its units.
- Its metadata.

For example:

```text
long_name: River Flow
units: m3 s-1
```

This tells us that streamflow is measured in cubic meters per second (m³/s).

---

### Part 4: Extract Streamflow Data

#### 4.1 Display the first ten streamflow values

Copy and paste:

```python
print(ds["streamflow"].values[:10])
```

This displays the first ten streamflow values in the dataset.

Each value corresponds to a river feature ID.

You may receive an output containing a lot of zeros. There are a few possible reasons:

1. Naturally low or no flow: Some river segments may have very little or no modeled flow.
2. Small streams: Smaller waterways can have extremely low flow.
3. Model output: The NWM calculates streamflow for millions of river segments, and not every segment will have measurable flow at every time.

#### 4.2 Create a table of feature IDs and streamflow

To make the data easier to interpret, convert the streamflow variable into a pandas DataFrame.

```python
streamflow_df = ds[["streamflow"]].to_dataframe().reset_index()

print(streamflow_df.head(10))
```

The resulting table should contain columns similar to:

| feature_id | streamflow |
|---:|---:|
| 101 | 9.16 |
| 179 | 0.00 |
| 181 | 0.00 |
| 183 | 0.00 |
| 185 | 0.00 |

The `feature_id` column identifies the river segment, while the `streamflow` column contains its modeled flow value.

Depending on the dataset, additional columns such as `time` and `reference_time` may also appear.

## 4.3 Select specific river feature IDs

Instead of using the first entries in the dataset, we can select specific river feature IDs. The feature IDs below follow Deer Creek to Utah Lake through the Provo River.

First, define the IDs we want to examine:

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
    10376192,
]
```

Check which IDs are available:

```python
available_ids = ds.feature_id.values

valid_ids = [
    fid for fid in feature_ids
    if fid in available_ids
]

missing_ids = [
    fid for fid in feature_ids
    if fid not in available_ids
]

print("Available IDs:", valid_ids)
print("Missing IDs:", missing_ids)
```

This checks whether each requested feature ID exists in the current NetCDF file.

**Important:** Not every NetCDF file necessarily contains every requested feature ID. If an ID is missing, it may not be included in that particular dataset.

---

### Part 5: Create a Streamflow Chart

To make this process reusable, create a function that accepts a list of feature IDs.

Copy and paste the entire function into a new notebook cell:

```python
def graph_streamflow(feature_ids, ds):

    available_ids = ds.feature_id.values

    valid_ids = [
        fid for fid in feature_ids
        if fid in available_ids
    ]

    missing_ids = [
        fid for fid in feature_ids
        if fid not in available_ids
    ]

    if not valid_ids:
        print("None of the selected IDs were found.")
        return

    selected = ds.sel(feature_id=valid_ids)

    streamflow_values = selected["streamflow"].squeeze()

    plt.figure(figsize=(12, 6))

    plt.bar(
        selected.feature_id.values.astype(str),
        streamflow_values.values
    )

    plt.xlabel("River Feature ID")
    plt.ylabel("Streamflow (m³/s)")
    plt.title("NWM Streamflow: Deer Creek to Utah Lake")

    plt.figtext(
    0.5, 0.91,
    " Short Range | Provo River | Initialization: 00:00 UTC | Forecast: +9 Hours",
    ha="center",
    fontsize=9

    plt.xticks(rotation=45)
    plt.tight_layout()
    plt.show()

    if missing_ids:
        print("IDs not found:", missing_ids)
```

Run the cell to define the function.

Now call the function using your selected feature IDs from Section 4.3:

```python
graph_streamflow(feature_ids, ds)
```

The function will:

1. Check which feature IDs exist in the dataset.
2. Select the available IDs.
3. Retrieve their streamflow values.
4. Create a bar chart.
5. Report any missing IDs.

You can change the feature ID list in Section 4.3 whenever you want to visualize different river segments.

---
Here is the graph based on the date Sept 17th 2018. Based on the date you selected, the data will vary. 
<img width="1190" height="590" alt="f3e07cb8-bb98-48d9-af09-038db341f578" src="https://github.com/user-attachments/assets/57ae3aa9-d02d-47c9-b351-cb766b9eb02b" />


### Part 6: Save Your Extracted Data

Once the data have been extracted, they can be saved as a CSV file for later analysis.

```python
selected_df = ds.sel(feature_id=valid_ids)[["streamflow"]].to_dataframe().reset_index()

selected_df.to_csv("selected_streamflow.csv", index=False)

print("Streamflow data saved successfully.")
```

This creates a CSV file containing the selected feature IDs, their corresponding streamflow values, and associated time information.

The file can be opened using Excel, pandas, or other data-analysis software.

 ## Next steps
 explore google collab wit hjupyter notbooks
 write some kind of notes (polish this one) for a newbie in the future. 
 Add in a tutorial of setting up and extracting one of these files and then reading it. Obviously would need to complete this myself as well. 
