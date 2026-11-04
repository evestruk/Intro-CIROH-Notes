### Tutorial: Finding and Viewing NWM Data 
*For this tutorial we will be using Google Cloud, ...*

#### **1)** Access the [Google Cloud NWM Bucket HERE](https://console.cloud.google.com/storage/browser/national-water-model;tab=objects?utm_source=chatgpt.com&prefix=&forceOnObjectsSortingFiltering=false)
From here you should recognize the folder names format >nwm.yyyymmdd
Lets take a quick look at what all of these other folders are. 

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
* The **Forecast Folder** then predicts what will happen next. These predictions are then organized into the long, medium and short range folders. ☆ Ensembles are multiple versions of a forecast.
Reference [Water NOAA Web](https://water.noaa.gov/about/nwm?utm_source=chatgpt.com) and [Output File Contents](https://water.noaa.gov/about/output_file_contents) for more info on organization. 
* 


### Coding for Visual Streamflow Chart
#### Tutorial: Opening and Visualizing National Water Model (NWM) Streamflow Data

This tutorial walks through how to open a National Water Model (NWM) NetCDF file, inspect its contents, extract streamflow values, and create a bar chart using Python.

The tutorial is designed for beginners and uses Visual Studio Code (VS Code), Jupyter Notebook, and several Python packages.

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

Create a new folder for your project.

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

#### 2.1 Find an NWM output file

For this tutorial, we will use a short-range Channel Routed Output file from the National Water Model.

Example filename:

```text
nwm.t00z.short_range.channel_rt.f009.conus.nc
```

The file should be a NetCDF (`.nc`) file.

The example represents:

- `nwm`: National Water Model.
- `t00z`: Model initialization at 00:00 UTC.
- `short_range`: Short-range forecast simulation.
- `channel_rt`: Channel routing output.
- `f009`: Forecast hour 9.
- `conus`: Continental United States domain.
- `.nc`: NetCDF file format.

**Important:** NWM file naming conventions and available forecast products vary by model version and archive. Make sure you select a valid Channel Routed Output file that contains the `streamflow` variable.

The NWM data are available through NOAA's public data resources:

- [NOAA National Water Model](https://water.noaa.gov/about/nwm)
- [Google Cloud Public Dataset: NOAA National Water Model](https://console.cloud.google.com/marketplace/product/noaa-public/national-water-model)

Download or otherwise make the selected `.nc` file accessible to your project.

#### 2.2 Organize your project files

Through your file explorer, place the NetCDF file in the same folder as your Jupyter Notebook.

On VS Code *Explorer* side panel, your project should look similar to this:

```text
hydroinformatics_research/
│
├── nwm_streamflow_tutorial.ipynb
│
└── nwm.t00z.short_range.channel_rt.f009.conus.nc
```

This allows the notebook to reference the file by its filename without needing a full computer-specific path.

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
*replace "nwm.t00z.short_range.channel_rt.f009.conus.nc" with your nwm file name. 
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

You may recieve an output containing a lot of zeros. There are a few possible reasons:
1. Naturally low or no flow: Some river segments may be dry or have very little water.
2. Small streams: Smaller waterways can have extremely low flow, sometimes represented as zero.
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

## 4.3 Select specific river feature IDs

Instead of using the first entries in the dataset, we can select specific river feature IDs.

First, define the IDs we want to examine:

```python
feature_ids = [
    22812077,
    445308,
    445310,
    445312,
    101
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

#### 5.1 Create a bar chart of the first 20 feature IDs

Before plotting selected IDs, we can create a simple chart using the first 20 entries.

```python
plot_data = ds["streamflow"].isel(feature_id=slice(0, 20))

plt.figure(figsize=(12, 5))

plt.bar(
    plot_data.feature_id.values.astype(str),
    plot_data.values
)

plt.xlabel("River Feature ID")
plt.ylabel("Streamflow (m³/s)")
plt.title("NWM Short-Range Forecast Streamflow")

plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

This produces a bar chart with:

- X-axis: River feature IDs.
- Y-axis: Streamflow in cubic meters per second.
- Bars: Modeled streamflow values for each feature ID.

**Note:** The first 20 feature IDs are not necessarily geographically connected river segments.

#### 5.2 Create a chart using selected feature IDs

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

    plt.figure(figsize=(12, 6))

    plt.bar(
        selected.feature_id.values.astype(str),
        selected.streamflow.values
    )

    plt.xlabel("River Feature ID")
    plt.ylabel("Streamflow (m³/s)")
    plt.title("NWM Short-Range Forecast Streamflow")

    plt.xticks(rotation=45)
    plt.tight_layout()
    plt.show()

    if missing_ids:
        print("IDs not found:", missing_ids)
```

Run the cell to define the function.

Now call the function using your selected feature IDs:

```python
graph_streamflow(
    [22812077, 445308, 445310, 445312, 101],
    ds
)
```

The function will:

1. Check which feature IDs exist in the dataset.
2. Select the available IDs.
3. Retrieve their streamflow values.
4. Create a bar chart.
5. Report any missing IDs.

You can change the feature ID list whenever you want to visualize different river segments.

---

### Part 6: Save Your Extracted Data

Once the data have been extracted, they can be saved as a CSV file for later analysis.

```python
selected_df = ds.sel(feature_id=valid_ids)[["streamflow"]].to_dataframe().reset_index()

selected_df.to_csv("selected_streamflow.csv", index=False)

print("Streamflow data saved successfully.")
```

This creates a CSV file containing the selected feature IDs and their corresponding streamflow values.

The file can be opened using Excel, pandas, or other data-analysis software.

---

### Part 7: What You Have Accomplished

After completing this tutorial, you should be able to:

- [ ] Install the required Python packages.
- [ ] Set up a Jupyter Notebook in VS Code.
- [ ] Obtain an NWM NetCDF file.
- [ ] Open a NetCDF file using xarray.
- [ ] Inspect the dataset dimensions and variables.
- [ ] Identify the streamflow variable and its units.
- [ ] Connect streamflow values to river feature IDs.
- [ ] Extract selected feature IDs.
- [ ] Create a bar chart of streamflow values.
- [ ] Save extracted streamflow data as a CSV file.

#### Future Extensions

This tutorial focuses on one NWM forecast file and one forecast valid time.

Further development could include:

- Extracting streamflow from multiple short-range forecast files.
- Working with medium-range forecast output.
- Comparing streamflow across different forecast initialization times.
- Creating time-series visualizations for selected river segments.
- Comparing NWM forecast output with NWM analysis products.
- Investigating comparisons with observed streamflow data.

---

**Result:** A repeatable Python workflow for opening NWM Channel Routed Output NetCDF files, extracting streamflow values by river feature ID, and visualizing the results.


# Next step
can i find or make an easy way to retrieve feature IDs and can i make a visual map through vs code? 
also how do i start using claude?
