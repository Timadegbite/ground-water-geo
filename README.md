# Groundwater Potential Mapping for Kano State, Nigeria

This repository contains a Google Earth Engine (GEE) workflow for
groundwater potential mapping in Kano State, Nigeria.

The project applies a GIS-based Multi-Criteria Decision Analysis (MCDA)
approach to integrate seven groundwater-potential criteria: rainfall,
elevation, slope, land cover, soil texture, drainage density, and
Topographic Wetness Index (TWI).

The thematic layers are processed, reclassified to a common 1--5
suitability scale, weighted using the Analytic Hierarchy Process (AHP),
and combined through a weighted overlay to generate a Groundwater
Potential Index (GWPI) and five groundwater-potential classes.

------------------------------------------------------------------------

## Overview

The project:

-   Defines Kano State as the Area of Interest (AOI).
-   Prepares seven thematic layers: rainfall, elevation, slope, land
    cover, soil texture, drainage density, and TWI.
-   Uses Google Earth Engine datasets to process and analyse the
    thematic layers.
-   Clips datasets to Kano State and standardizes the processing scale.
-   Reclassifies thematic layers to a 1--5 groundwater-suitability
    scale.
-   Uses AHP to determine criterion weights and checks matrix
    consistency.
-   Performs a weighted overlay to calculate the GWPI.
-   Classifies GWPI into Very Low, Low, Moderate, High, and Very High
    zones.
-   Identifies High and Very High areas as priority zones for further
    investigation.
-   Provides raster and vector export functions.

## Study Area

The analysis focuses on **Kano State, Nigeria**. The study-area boundary
is obtained from FAO GAUL in Google Earth Engine, and thematic datasets
are clipped to the Kano State boundary.

### Spatial Reference and Resolution

-   **CRS:** WGS 84 / UTM Zone 32N
-   **EPSG:** 32632
-   **Target spatial scale:** 30 m

## Data Sources

  Dataset               Purpose
  --------------------- ---------------------------------------------
  FAO GAUL              Kano State administrative boundary
  NASA SRTM             Elevation and slope
  ESA WorldCover 2021   Land cover
  OpenLandMap           Soil texture
  CHIRPS Daily          Rainfall
  MERIT Hydro           Drainage and hydrological variables
  MERIT DEM             Terrain information used in TWI calculation

## Methodology

``` text
Data Acquisition
       ↓
Pre-processing
       ↓
Thematic Layer Preparation
       ↓
Reclassification to 1–5
       ↓
AHP Pairwise Comparison
       ↓
AHP Weight Calculation
       ↓
Consistency Check
       ↓
Weighted Overlay
       ↓
Groundwater Potential Index
       ↓
Five Groundwater Potential Classes
       ↓
Priority Area Identification
```

### 1. Elevation and Slope

Elevation is obtained from the NASA SRTM Digital Elevation Model (DEM).
Slope is derived from the DEM using Google Earth Engine terrain
functions. Gentler slopes receive higher suitability scores.

### 2. Land Cover

ESA WorldCover 2021 represents land-cover conditions across Kano State.
Land-cover classes are assigned relative groundwater suitability scores
and converted to the common 1--5 scale.

### 3. Soil Texture

OpenLandMap USDA soil texture data is used. The selected soil texture
classes are reclassified to the 1--5 groundwater-suitability scale.

### 4. Rainfall

CHIRPS Daily precipitation is used to estimate rainfall. Annual totals
are calculated for 2014--2023 and averaged to obtain mean annual
precipitation. Higher rainfall receives higher suitability.

### 5. Drainage Density

MERIT Hydro upstream drainage-area data is used to derive an approximate
drainage-density surface. A 100 km² flow-accumulation threshold is used
to identify stream pixels, and drainage density is approximated within a
5 km circular neighbourhood. Lower drainage density receives higher
suitability.

### 6. Topographic Wetness Index (TWI)

TWI is derived as a topographic wetness proxy using MERIT Hydro upstream
drainage area and MERIT DEM slope.

``` text
TWI = ln(flow accumulation / tan(slope))
```

Higher TWI receives higher suitability.

## Reclassification

All seven criteria are converted to a common 1--5 suitability scale.

    Score Suitability
  ------- -------------
        1 Very Low
        2 Low
        3 Moderate
        4 High
        5 Very High

-   Rainfall and drainage density use percentile-based classification.
-   Elevation, slope, and TWI use predefined thresholds.
-   Land cover and soil texture use class-based suitability assignments.

## Analytic Hierarchy Process (AHP)

AHP determines the relative importance of the seven criteria using a
pairwise-comparison matrix and normalized principal eigenvector.

  Criterion            Weight
  ------------------ --------
  Rainfall             21.25%
  Soil Texture         21.25%
  TWI                  20.34%
  Land Cover           11.61%
  Drainage Density     11.61%
  Elevation             7.45%
  Slope                 6.49%

The weights sum to 100%.

### AHP Consistency Check

The calculated Consistency Ratio is **0.0139**, below the commonly
accepted threshold of 0.10.

## Weighted Overlay

The seven reclassified layers are combined using a weighted linear
combination.

``` text
GWPI = Σ (Suitability Score × AHP Weight)
```

Using the derived weights:

``` text
GWPI =
(Rainfall × 0.2125)
+
(Soil Texture × 0.2125)
+
(TWI × 0.2034)
+
(Land Cover × 0.1161)
+
(Drainage Density × 0.1161)
+
(Elevation × 0.0745)
+
(Slope × 0.0649)
```

## Groundwater Potential Classification

The resulting GWPI is classified into five zones.

    Class Groundwater Potential
  ------- -----------------------
        1 Very Low
        2 Low
        3 Moderate
        4 High
        5 Very High

### Map Colour Scheme

  Groundwater Potential   Colour
  ----------------------- -------------
  Very Low                Red
  Low                     Orange
  Moderate                Yellow
  High                    Light Green
  Very High               Dark Green

### Groundwater Potential Index Range

The observed GWPI ranges from approximately **1.74 to 4.17**. The
observed range is divided into five equal intervals to produce the five
groundwater-potential classes.

## Priority Areas for Groundwater Development

High and Very High groundwater-potential areas are identified as
priority zones for further groundwater investigation and development.
These are screening zones and should not be interpreted as guaranteed
productive aquifer locations.

### Priority Area Statistics

-   **3,888.48 km²** of High and Very High groundwater-potential areas.
-   **19.36%** of the total Kano study area.
-   **20,082.21 km²** total study area.

## Results

The MCDA produced a continuous GWPI and a classified
groundwater-potential map for Kano State. Approximately **3,888.48
km²**, representing **19.36%** of the **20,082.21 km²** study area, was
identified as High or Very High groundwater-potential area.

## Outputs

### Study Area

`![Kano State Study Area](images/kano-study-area.png)`{=html}

### Mean Annual Rainfall

`![Mean Annual Rainfall](images/mean-annual-rainfall.png)`{=html}

### Groundwater Suitability Layers

`![Groundwater Suitability Layers](images/suitability-layers.png)`{=html}

### Final Groundwater Potential Map

`![Groundwater Potential Zones](images/groundwater-potential-zones.png)`{=html}

## How to Run the Notebook

### 1. Clone the Repository

``` bash
git clone https://github.com/Timadegbite/ground-water-geo.git
cd ground-water-geo
```

### 2. Create the Conda Environment

``` bash
conda create -n groundwater-potential-mapping
```

### 3. Activate the Environment

``` bash
conda activate groundwater-potential-mapping
```

### 4. Install the Required Libraries

``` bash
conda install -c conda-forge earthengine-api geemap pandas numpy geopandas
```

### 5. Open the Notebook

Open the notebook in Jupyter Notebook or Visual Studio Code. The
notebook is located under:

``` text
Notebook/KANO_GROUNDWATER_MAPPING/
```

### 6. Authenticate Google Earth Engine

``` python
ee.Authenticate()
```

### 7. Initialise Earth Engine

``` python
ee.Initialize(project="YOUR_PROJECT_ID")
```

### 8. Run the Notebook

Run cells sequentially from data acquisition through preprocessing,
reclassification, AHP analysis, weighted overlay, classification, and
export.

## Reproducibility

The notebook documents study-area definition, dataset acquisition,
clipping, thematic-layer preparation, reclassification, AHP weighting,
consistency assessment, weighted overlay, GWPI generation,
classification, priority-area identification, and export.

The weighted overlay is:

``` text
GWPI = Σ (Suitability Score × AHP Weight)
```

## Data Export

The notebook includes reusable Google Earth Engine export functions for:

-   Kano State study-area boundary as Shapefile.
-   Original thematic layers as GeoTIFF.
-   Seven reclassified suitability layers as GeoTIFF.
-   Groundwater Potential Index.
-   Groundwater Potential Zones.
-   Priority Groundwater Areas.

Raster exports use a target scale of 30 m and EPSG:32632.

## Limitations

1.  The model uses seven selected criteria and does not explicitly
    incorporate geology, aquifer characteristics, groundwater levels,
    fracture and lineament distribution, hydraulic properties, or
    recharge conditions.
2.  AHP weights depend on expert judgment and pairwise-comparison
    assumptions.
3.  Weighted linear combination assumes additive relationships between
    criteria.
4.  Differences in spatial resolution, data quality, temporal
    characteristics, and classification accuracy introduce uncertainty.
5.  Reclassification thresholds can influence the resulting spatial
    distribution.
6.  The final zones have not been independently validated against
    borehole yields, groundwater levels, pumping tests, geophysical
    surveys, or other direct hydrogeological measurements.

## Recommendations

1.  Prioritise High and Very High areas for field investigation and
    hydrogeological verification.
2.  Incorporate geology, aquifer characteristics, groundwater levels,
    borehole yields, geophysical surveys, and other hydrogeological
    information in future assessments.
3.  Validate the potential zones using independent field and
    hydrogeological data.
4.  Use higher-resolution and locally validated datasets where
    available.
5.  Conduct sensitivity analysis on criterion weights and suitability
    thresholds.
6.  Consider groundwater demand, recharge, abstraction pressure, and
    long-term sustainability when planning development.
7.  Use the final map as a screening and prioritisation tool rather than
    as a standalone basis for borehole siting.

## Repository Structure

``` text
ground-water-geo/
│
├── Notebook/
│   └── KANO_GROUNDWATER_MAPPING/
│       ├── main.ipynb
│       └── ELEVATION_MAP.png
│
├── images/
│   ├── groundwater-potential-zones.png
│   ├── kano-study-area.png
│   ├── mean-annual-rainfall.png
│   └── suitability-layers.png
│
└── README.md
```

## Tools and Technologies

-   Google Earth Engine
-   Python
-   geemap
-   NumPy
-   Pandas
-   GeoPandas
-   Jupyter Notebook
-   Visual Studio Code
-   Anaconda / Conda

## Contributors

-   [Oluwaseun Aribisogan](https://github.com/oluwaseun-tech)
-   [Timadegbite](https://github.com/Timadegbite)
-   [Bernard Kortor](https://github.com/kortor19)
-   [Elvis100314](https://github.com/Elvis100314)

## License

This project is open-source. The individual datasets used in the
analysis retain their respective licences and terms of use.
