# Forest_fire_analysis_Nepal
This is my capstone project for the Environmental Data Science certificate program.
## Project Description
Forest fires pose a growing threat to Nepal's forest ecosystems, rural livelihoods, and carbon storage, particularly during the hot, dry pre-monsoon season. This capstone project analyzes the spatial distribution, climatic drivers, and economic cost of forest fires across all 77 districts of Nepal from 2015-2025. Using open access satellite and climate datasets, the study maps where and when fires occur most frequently, compares fire incidence inside versus outside protected areas.

## Research Question
Where and When are the forest fires must concentrated across Nepal's districts & PAs, how do rainfall and temperature affect fire frequency?

## Data Source
|Data|Description|Source|
|raw_data/modis_2015_2025_Nepal.csv|forest fire data for nepal from 2015-2025|https://firms.modaps.eosdis.nasa.gov/country/
|raw_data/Western and eastern nepal climate data.csv/https://power.larc.nasa.gov/data-access-viewer/
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
