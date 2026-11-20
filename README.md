# Forest_fire_analysis_Nepal
This is my capstone project for the Environmental Data Science certificate program.
## Project Description
Forest fires pose a growing threat to Nepal's forest ecosystems, rural livelihoods, and carbon storage, particularly during the hot, dry pre-monsoon season. This capstone project analyzes the spatial distribution, climatic drivers, and economic cost of forest fires across all 77 districts of Nepal from 2015-2025. Using open access satellite and climate datasets, the study maps where and when fires occur most frequently, compares fire incidence inside versus outside protected areas.

## Research Question
Where and When are the forest fires must concentrated across Nepal's districts & PAs, how do rainfall and temperature affect fire frequency?

## Data Source
|Data|Description|Source|
|raw_data/modis_2015_2025_Nepal.csv|forest fire data for nepal from 2015-2025|https://firms.modaps.eosdis.nasa.gov/country/
|raw_data/Nepal_climate _data_2015-2025.csv/https://power.larc.nasa.gov/data-access-viewer/
|raw_data/geo_Boundaries_NPL-ADM2-all.shp|nepal administrative boundary shapefile|https://data.humdata.org/dataset/geoboundaries-admin-boundaries-for-nepal/resource/b7f062a7-4d8f-49fb-bbc2-4dd6fd30771d
|raw_data/protected and conserved area.geojson|satellite data for PCAs|https://data.humdata.org/dataset/unep_wdpca_npl/resource/bcb0ba01-fcbe-4e76-a646-9288276a9e00
## repository structure
Forest_fires_analysis_Nepal/
├── README.md ← you are here
├── raw_data/ ← original data, never edited
│ ├── modis_2015_2025_Nepal.csv
│ └── Western and eastern nepal temp data.csv
│ └── geo_Boundaries_NPL-ADM2-all.shp
│ └── protected and conserved area.geojson
│ └── 
 
├── clean_data/ ← output of 1_data_clean.ipynb
│ └── Forest_fire_analysis_Nepal.csv
├── figures/ ← plots and tables (coming in later scripts)
└── scripts/ ← notebooks, numbered in the order they run
├── 0_project_setup.ipynb
└── 1_data_clean.ipynb


## How to run the code
All scripts are Google Colab notebooks written in R. Run them in number order:
1. `scripts/0_project_setup.ipynb` creates the project folder and subfolders in
Google Drive.
2. `scripts/1_data_clean.ipynb` reads both raw files, reshapes and joins them, and
writes `clean_data/Forest_fire_analysis_Nepal.csv`.
3. `scripts/2_analysis.ipynb` *(coming soon)* will compare the relation between climatic factors and fires.
Each notebook mounts Google Drive with one Python cell (`drive.mount()`), then
switches to the R runtime. See the notes at the top of each notebook.
## Data Cleaning steps (1_data_clean>ipynb)
1. Reading & Standardizing Raw Tables
Combined all 11 years MODIS CSV files(2015-2025) into one table and standardized column names to lowercase. Read all 8 regional NASA POWER climate CSVs, skipping the 9-line begin header metadata block (skip = 9). so parameter, year,lat, lon,jan-dec, ANN aligns cleanly across all files.
2. Filtering Active Vegetation Fires and Remove Duplicates
I have filtered MODIS detections to type == 0 to keep only presumed vegetation/forest fires and exclude static non-forest thermal anomalies (such as brick kilns and industrial heat sources).
Dropped low-confidence detections(confidence<30) and row with missing coordinates (!is.na(latitute), !is.na(longitude)). so that thermal noise do not inflate fire counts.
Removed exact coordinate-timestamp duplicates using distinct(latitude, longitude, acq_data, acq_date, acq_time, .keep_all = TRUE)
Parsed acq_date(YYYY-MM-DD) as a Date (as.Date()) and extracted numeric year and month columns (mutate()) for temporal aggregation.
3. Reshaped wide climate Data to tidy long format(pivot_longer())
Reshaped the monthly columns(jan-dec) into long format(year, month, parameter, value) using pivot_longer().
Converted text abbreviations (jan-dec) to numeric months (1-12) using match (month_name, tolower(month.abb)) for calender sorting.
Filtered out missing values (!is.na(value)) and NASA POWER's missing-data sentinel value (value != -999 ), and removed duplicate grid-cell entries (distinct()).
4. Monthly Aggregation and Dataset join
Aggregated cleaned MODIS fire detections (fire_counts, mean_frp) and regional climate records(mean_value by parameter) to the monthly level(year, month).
Joined the monthly climate table and monthly fire summary with left_join(by = c("year", "month")) and checked join completeness with anti_join().
Exported clean_data/nepal_modis_clean.csv and clean_data/nepal_fire_climate_clean.csv by write_csv().
## Known Data Issues 7 Cleaning decisions
Zero-fire Months: Because left-join() keeps all year-month climate rows, months with zero recorded forest fires(such as peak monsoon months) retain NA in fire count rather than being dropped.
Spatial files reserved for analysis stage: District boundaries(geoBoundaries-NPL-ADM2-all) and Protected Area polygons(protected & conserved area.geojson) remain untouched in raw_data/ and will be spatially joined to nepal_modisclean.csv in scripts/2_analysis.ipyn.
## Status and Next Steps
Completed: project directory setup(0_project_setup.ipynb), raw data documentation, and data cleaning/merging pipeline(1_data_clean.ipynb)
Next Steps: Build scripts/2_analysis.ipynb to spatially join nepal_modis_clean.csv with Nepal's 77 district and protected Area, estimate district-level carbon losses(CO2e) and economic costes, and generate maps and plots in figures/.



