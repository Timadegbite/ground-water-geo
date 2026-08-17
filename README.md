# Groundwater Potential Mapping for Kano State, Nigeria

This repository contains a complete **Google Earth Engine (GEE)** workflow for groundwater potential mapping in Kano State, Nigeria. The project uses a GIS-based **Multi-Criteria Decision Analysis (MCDA)** approach to integrate rainfall, elevation, slope, land cover, soil texture, drainage density, and Topographic Wetness Index (TWI).

The thematic layers are prepared, reclassified to a common suitability scale, weighted using the **Analytic Hierarchy Process (AHP)**, and combined through a weighted overlay to generate a **Groundwater Potential Index (GWPI)** and five groundwater potential classes.

## Overview

The project:

- Defines Kano State as the Area of Interest (AOI).
- Prepares seven thematic layers relevant to groundwater potential:
  - Rainfall
  - Elevation
  - Slope
  - Land Cover
  - Soil Texture
  - Drainage Density
  - Topographic Wetness Index (TWI)
- Uses Google Earth Engine datasets to process and analyse the thematic layers.
- Clips the datasets to Kano State.
- Resamples the thematic layers to a common spatial resolution.
- Reclassifies the thematic layers to a 1–5 groundwater suitability scale.
- Uses the Analytic Hierarchy Process (AHP) to determine criterion weights.
- Checks the consistency of the AHP pairwise comparison matrix.
- Performs a weighted overlay to calculate the Groundwater Potential Index.
- Classifies the resulting GWPI into five groundwater potential zones:
  - Very Low
  - Low
  - Moderate
  - High
  - Very High
- Visualises the final groundwater potential map in Google Earth Engine.

## Study Area

The analysis focuses on **Kano State, Nigeria**. The administrative boundary for the study area is obtained from the FAO Global Administrative Unit Layers (GAUL) dataset in Google Earth Engine.

All thematic datasets are clipped to the Kano State boundary before further analysis.

## Data Sources

The project uses the following datasets:

| Dataset | Purpose |
|---|---|
| FAO GAUL | Kano State administrative boundary |
| NASA SRTM | Elevation and slope |
| ESA WorldCover 2021 | Land cover |
| OpenLandMap | Soil texture |
| CHIRPS Daily | Rainfall |
| MERIT Hydro | Drainage and hydrological variables |
| MERIT DEM | Terrain information used in TWI calculation |

## Methodology

The groundwater potential mapping workflow consists of the following major stages:

```text
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

### 1. Elevation and Slope

Elevation is obtained from the NASA SRTM Digital Elevation Model (DEM). Slope is derived from the DEM using Google Earth Engine's terrain functions.

Elevation and slope are used as topographic factors in assessing groundwater potential. The slope layer is reclassified to a 1–5 suitability scale, with gentler slopes receiving higher suitability scores in the model.

### 2. Land Cover

ESA WorldCover 2021 is used to represent land-cover conditions across Kano State.

The land-cover classes are reclassified according to their relative groundwater suitability and converted to a common 1–5 suitability scale.

### 3. Soil Texture

Soil texture data is obtained from OpenLandMap using the USDA soil texture classification.

The selected soil texture layer is reclassified to a 1–5 groundwater suitability scale based on the relative suitability of the soil classes.

### 4. Rainfall

Daily precipitation data from the CHIRPS dataset is used to estimate rainfall across the study area.

Annual precipitation totals are calculated for the period 2014–2023 and averaged to obtain mean annual precipitation.

Rainfall values are then reclassified into five suitability classes, with higher rainfall receiving higher groundwater suitability scores.

### 5. Drainage Density

MERIT Hydro data is used to derive drainage-related information for the study area.

Drainage density is incorporated as a groundwater-potential factor because drainage characteristics influence surface runoff and infiltration.

The resulting drainage density layer is reclassified to a 1–5 suitability scale, with lower drainage density receiving higher suitability scores.

### 6. Topographic Wetness Index (TWI)

The Topographic Wetness Index (TWI) is derived from hydrological and terrain information.

The TWI is used as an indicator of relative moisture accumulation and potential water availability.

The resulting TWI layer is reclassified to a 1–5 suitability scale, with higher TWI values receiving higher suitability scores.

## Analytic Hierarchy Process (AHP)

The Analytic Hierarchy Process (AHP) is used to determine the relative importance of the seven groundwater-potential criteria.

A pairwise comparison matrix is constructed to compare the criteria, and the principal eigenvector is used to derive the normalized criterion weights.

The resulting weights are:

| Criterion | Weight |
|---|---:|
| Rainfall | 21.25% |
| Soil Texture | 21.25% |
| TWI | 20.34% |
| Land Cover | 11.61% |
| Drainage Density | 11.61% |
| Elevation | 7.45% |
| Slope | 6.49% |

The weights sum to 100%.

### AHP Consistency Check

The consistency of the pairwise comparison matrix is assessed using the Consistency Index (CI) and Consistency Ratio (CR).

The calculated Consistency Ratio (CR) is **0.0139**, which is below the commonly accepted threshold of 0.10. This indicates that the pairwise comparisons used to derive the criterion weights are acceptably consistent.

## Weighted Overlay

The seven reclassified thematic layers are combined using a weighted linear combination.

The Groundwater Potential Index (GWPI) is calculated as:

```text
GWPI = Σ (Suitability Score × AHP Weight)
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


Then:
```markdown
## Groundwater Potential Classification

The Groundwater Potential Index is classified into five groundwater potential zones:

| Class | Groundwater Potential |
|---:|---|
| 1 | Very Low |
| 2 | Low |
| 3 | Moderate |
| 4 | High |
| 5 | Very High |

The final groundwater potential map uses the following colour scheme:

| Groundwater Potential | Colour |
|---|---|
| Very Low | Red |
| Low | Orange |
| Moderate | Yellow |
| High | Light Green |
| Very High | Dark Green |

## Results

The analysis produces:

- Seven processed thematic layers.
- Seven reclassified groundwater suitability layers.
- AHP-derived criterion weights.
- AHP consistency assessment.
- A Groundwater Potential Index (GWPI).
- A classified groundwater potential map for Kano State.

The final output identifies areas of Very Low, Low, Moderate, High, and Very High relative groundwater potential across the study area.

The groundwater potential map is intended as a spatial screening and prioritisation product. It does not replace detailed hydrogeological investigation, geophysical surveys, borehole information, pumping tests, or field validation.

## How to Run the Notebook

1. Clone this repository.

2. Create the Conda environment:

```bash
conda create -n groundwater-potential-mapping
3. Activate the environment:
conda activate groundwater-potential-mapping
4. Install the required libraries
conda install -c conda-forge earthengine-api geemap pandas numpy geopandas
5. Open the groundwater mapping notebook in Jupyter Notebook or VS Code
6. Authenticate Google Earth Engine when prompted:
```python
ee.Authenticate()
7. Initialise Earth Engine using your Google Cloud project:
```python
ee.Initialize(project="YOUR_PROJECT_ID")
8. Run the notebook cells sequentially.

## Reproducibility

The notebook documents the main processing steps and calculations used to generate the groundwater potential map.

The workflow includes:

- Study area definition
- Dataset acquisition
- Spatial clipping
- Thematic layer preparation
- Reclassification to a 1–5 suitability scale
- AHP pairwise comparison
- AHP weight calculation
- AHP consistency assessment
- Weighted overlay calculation
- Groundwater Potential Index generation
- Classification into five groundwater potential zones

The weighted overlay is calculated using:

```text
GWPI = Σ (Suitability Score × AHP Weight)

## Repository Structure

```text
ground-water-geo/

├── Notebook/
│   └── KANO_GROUNDWATER_MAPPING/
│       ├── [notebook filename].ipynb
│       └── ELEVATION_MAP.png
│
├── images/
│   ├── groundwater-potential-zones.png
│   ├── kano-study-area.png
│   ├── mean-annual-rainfall.png
│   └── suitability-layers.png
│
└── README.md

## Tools and Technologies
- Google Earth Engine
- Python
- geemap
- NumPy
- Pandas
- GeoPandas
- Jupyter Notebook
- Visual Studio Code
- Anaconda / Conda

---
```markdown
## Outputs

The project produces spatial outputs for the groundwater potential assessment, including the study area, thematic inputs, reclassified suitability layers, and the final groundwater potential classification.

### Study Area

The study area is defined by the Kano State administrative boundary.

![Kano State Study Area](images/kano-study-area.png)

### Mean Annual Rainfall

Mean annual precipitation is calculated from CHIRPS daily rainfall data for the period 2014–2023.

![Mean Annual Rainfall](images/mean-annual-rainfall.png)

### Groundwater Suitability Layers

The thematic groundwater-potential factors are reclassified to a common 1–5 suitability scale before the weighted overlay.

![Groundwater Suitability Layers](images/suitability-layers.png)

### Final Groundwater Potential Map

The final Groundwater Potential Index is classified into five groundwater potential zones:

- Very Low
- Low
- Moderate
- High
- Very High

![Groundwater Potential Zones](images/groundwater-potential-zones.png)


## Contributors

- [Oluwaseun Aribisogan](https://github.com/oluwaseun-tech)
- [Timadegbite](https://github.com/Timadegbite)
- [Bernard Kortor](https://github.com/kortor19)
- [Elvis100314](https://github.com/Elvis100314)

## License
This project is open-source. The individual datasets used in the analysis retain their respective licences and terms of use.
