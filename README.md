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
     - This forecast is used by emergency personnel use for a) targeted evacuations b) deploy sandbags c) close low-lying intersections before roadway flooding happens.

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
_ 1km gridded NetCDF (land surface variables and forcing)
_ 250m gridded NetCDF (ponded water depth and depth to soil saturation)
_ Point-type NetCDF (stream routing and reservoir variables)
_ Point-type reservoir NetCDF (water surface elevation, inflow, outflow)

☆ A very simple way to think about these different file types and their organization 
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
* 
