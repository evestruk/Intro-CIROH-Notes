# Intro-CIROH-Notes

### The National Water Model (NWM)

* A model that simulates and forecasts water flow, snowmelt, soil moisture, and water levels across USA: including southern Alaska, Hawaii, Puerto Rico, and the U.S. Virgin Islands.
* Uses radar and gauge data alongside weather prediction models.
* 4 different types of forecasts

  * Analysis (Right Now) Every hour updates of real time hydrologic simulation
  * Short-Range are 18hours out, high detail, hour by hour warnings for rain and flash floods
  * Medium-range (up to 10 days prediction) running multiple scenarios to show river predictions
  * Long-Range (30 days out) big picture stuff used for droughts or long term high water.
* BYU's role: Creating a HydroServer application to provide quickly accessible data from NWM.



### The National Weather Service (NWS)

* Weather, water, climate data to provide forecasts, warnings, and data for decision making



### CIROH (Cooperative Institute for Research to Operations in Hydrology)

* A large partnership built on Universities, governments, and private sectors
* Goal: Advance hydrological forecasting through research and technical products
* "Science for Society" connecting Academia, Industry, and government together.



### Adriene Cope's National Water Model Data Access Website

* What it the purpose of the "Flood Category Not Defined"?
* Why does Louisiana have more "data not current" observation dots?
* What is the difference between the observations \& forecasts vs hydrologic ensemble forecasts on the River Gauge Water noaa map?
* How accurate is the precipitation estimate compared to a regular persons weather app and local forecast? Basically who benefits from this interactive map? Is it purely for learning purposes?
* I looked at the Jordan River National water Model 3 day forecast on the interactive map. The rivers flow shows general consistency through the week, however as of 8am today (9/25/2026) it shows a steady decline with predictions dropping from 430.84 cfs flow to 186 within 24 hours. why is that?
* I could not get The Water NOAA image viewer to work. Could I get help or could provide a quick tutorial
* Http and FTP files are commonly used ways to transfer files and data across the web (hence the use of several online sources for the NWM)
* A NetCDF is a file format for science related fields. they can be used for temperature, humidity and wind. 
* A lot of these links work, however I could use some direction on how to use these resources and what stuff means. 
* What is a S3 bucket and how does one use it? - AWS
* i think including some of this information on what the practical uses of different stuff could be helpful. I find it easier to understand what all the files, ranges etc mean when I have a why each one would be used. ( https://github.com/awslabs/open-data-docs/tree/main/docs/noaa/noaa-nwm-pds )
* 

### NWM Forecast Model Data Notes
* Network Common Data Form format (NetCDF) have the following configurations
_ 1km gridded NetCDF (land surface variables and forcing)
_ 250m gridded NetCDF (ponded water depth and depth to soil saturation)
_ Point-type NetCDF (stream routing and reservoir variables)
_ Point-type reservoir NetCDF (water surface elevation, inflow, outflow)
* Files on the noaa-nwm-pds Amazon S3 Bucket are organized by daily forecasts. Each forecast begins with the prefix
>nwm.yyyymmdd
where September 29th 2026 data would be on file as
>nwm.20260929
* 



* 



\*\*\* Something I am really interested in (long term) learning more about is the decision making process and data available to Houston/Kingwood leaders that led to my house flooding. \*\*\*
\*\*\* Research Project Ideas include 1. compare forecast vs reality of Hurricane Harvey 2. Create a waterflow model of Kingwood and learn that software 3. Create a sample warning alert for my created forecast. 



How much does Hydroinformatic Civil Engineers work with Weather services including natural disasters. 

## Answering My Houston Question
_ _ What decision process and data was used for Houston/Kingwood members during Hurricane Harvey?

There are 3 types of data used for municipal decision-makers
1. Short-Range Forecast: 0-18 hours forecast that monitors streamflow
   _ This is used for emergency personnel use for a) targeted evacuations b) deploy sandbags c) close low-lying intersections before roadway flooding happens.
2. Medium-Range Forecast: 1-10 day forecast that provide advance warning for city councils to allocate emergency budgets, clear storm drains, notify critical infrastructure, and request mutual aid from neighboring jurisdictions.
3. Long-Term Data: Decades of modeled streamflow history that can help city councils and urban planners to analyze historical flow patterns in order to a) establish flood-fringe zoning laws (laws about development in flood prone areas. "Strict floodway" is where building is banned to keep water moving, however flood fringe moderates allowed development. Usually requires floor elevation standards, floodproofing, landscaping restrictions, and mandatory Floodplain Development Permits). b) redesign culverts (water flow channels to drainage ditches). c) retention basins construction d) capital improvement projects.

 ### Climate Change Increases the Importance of Accurate and Updated Forcasts
 Increased floods around the nation due to major storms and elevating oceans. Data is being used to help understand risks and make preventative decisions. 
 **What kind of data is available to urban planning and home builders? How are structural engineers adapting homebuilding due to these increased floods and rising ocean? 
 **How does one prevent flooding besides retention ponds?
 
