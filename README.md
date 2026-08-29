# Location-Specific-Landslide-Risk-Scoring-Index-Sri-Lanka

**Assignment:** Location-Specific Landslide Risk Scoring Index (Sri Lanka)

**W.P.S.S.M Perera**
ID: 20357386

## LS-LRSI — Code Structure and Reproducibility Guide

Location-Specific Landslide Risk Scoring Index (LS-LRSI) for Ududumbara Divisional Secretariat Division, Kandy District, Sri Lanka.

This explains how to run the final reproducible code.

---

## 1. Required folder structure

Place the final code folder and dataset folder next to each other:

```
Ududumbara/
│
├── LS-LRSI-2025_Datasets/
├── LS-LRSI-2025_Final_Code/
│   ├── 00_check_environment.py
│   ├── 01_prepare_boundary.py
│   ├── 02_prepare_dem_terrain.py
│   ├── 03_prepare_rainfall.py
│   ├── 04_prepare_roads.py
│   ├── 05_prepare_streams.py
│   ├── 06_prepare_model_inputs.py
│   ├── 07_build_ahp_index.py
│   ├── 08_generate_eda.py
│   ├── 09_validate_nbro.py
│   ├── 10_clean_accimt_inventory.py
│   ├── 11_validate_accimt.py
│   ├── 12_final_qa_summary.py
│   ├── _project.py
│   └── README.md
│
└── LS-LRSI-2025_Final_Run/   # created automatically
```

---

## 2. Verified Python environment

The successful clean rerun used:

```
Python 3.14.3
numpy 2.4.6
pandas 2.3.3
geopandas 1.1.3
rasterio 1.5.0
scipy 1.17.0
shapely 2.1.2
matplotlib 3.10.8
pyproj 3.7.2
```

---

## 3. How to run the workflow

Run the scripts in numerical order.

```bash
python "LS-LRSI-2025_Final_Code\00_check_environment.py"
python "LS-LRSI-2025_Final_Code\01_prepare_boundary.py"
python "LS-LRSI-2025_Final_Code\02_prepare_dem_terrain.py"
python "LS-LRSI-2025_Final_Code\03_prepare_rainfall.py"
python "LS-LRSI-2025_Final_Code\04_prepare_roads.py"
python "LS-LRSI-2025_Final_Code\05_prepare_streams.py"
python "LS-LRSI-2025_Final_Code\06_prepare_model_inputs.py"
python "LS-LRSI-2025_Final_Code\07_build_ahp_index.py"
python "LS-LRSI-2025_Final_Code\08_generate_eda.py"
python "LS-LRSI-2025_Final_Code\09_validate_nbro.py"
python "LS-LRSI-2025_Final_Code\10_clean_accimt_inventory.py"
python "LS-LRSI-2025_Final_Code\11_validate_accimt.py"
python "LS-LRSI-2025_Final_Code\12_final_qa_summary.py"
```

---

## 4. What each script does

| Step | Script | Purpose |
|---|---|---|
| 00 | `00_check_environment.py` | Checks Python packages, project paths, and required source files. |
| 01 | `01_prepare_boundary.py` | Extracts the Ududumbara DSD boundary and checks area. |
| 02 | `02_prepare_dem_terrain.py` | Prepares SRTM DEM and derives slope, aspect, and curvature proxy. |
| 03 | `03_prepare_rainfall.py` | Creates the 2019–2023 CHIRPS five-year mean rainfall layer. |
| 04 | `04_prepare_roads.py` | Creates distance-to-road from the fixed OSM road snapshot. |
| 05 | `05_prepare_streams.py` | Creates distance-to-stream from the fixed OSM waterway snapshot. |
| 06 | `06_prepare_model_inputs.py` | Assembles and verifies the five model indicators. |
| 07 | `07_build_ahp_index.py` | Computes AHP weights and creates LS-LRSI score/class rasters. |
| 08 | `08_generate_eda.py` | Produces EDA, correlations, and supporting visual evidence. |
| 09 | `09_validate_nbro.py` | Performs a seven-site NBRO terrain-resolution diagnostic. |
| 10 | `10_clean_accimt_inventory.py` | Removes exact duplicate geometries from the ACCIMT inventory. |
| 11 | `11_validate_accimt.py` | Performs independent ACCIMT event-level validation. |
| 12 | `12_final_qa_summary.py` | Performs final reproducibility QA against archived outputs. |

---

## 5. Core analysis grid

All final model rasters use:

```
CRS: EPSG:32644
Grid: 1049 rows × 596 columns
Cell size: 30 m
```

---

## 6. Five LS-LRSI model indicators

The final AHP-WLC model uses:

1. Slope
2. Rainfall
3. Distance to streams
4. Curvature proxy
5. Distance to roads

AHP weights:

| Indicator | Weight | Percentage |
|---|---:|---:|
| Slope | 0.4174192 | 41.74% |
| Rainfall | 0.2633737 | 26.34% |
| Distance to streams | 0.1602272 | 16.02% |
| Curvature proxy | 0.0974765 | 9.75% |
| Distance to roads | 0.0615035 | 6.15% |

Consistency:

```
Lambda max = 5.068037
CI         = 0.017009
CR         = 0.015187
```

The CR is below 0.10, indicating strong internal consistency of the AHP pairwise-comparison matrix.

---

## 7. Verified model results

Continuous LS-LRSI:

```
Minimum = 0.144736
Mean    = 0.509475
Maximum = 0.886823
SD      = 0.120221
```

Final class distribution:

| Class | Cells | Area share |
|---|---:|---:|
| Low | 90,194 | 29.6582% |
| Medium | 58,952 | 19.3850% |
| High | 63,572 | 20.9041% |
| Very High | 91,394 | 30.0527% |

The model is a **relative landslide-susceptibility screening index**, not a calibrated probability model.

---

## 8. Rainfall processing

CHIRPS annual rainfall rasters from 2019–2023 are combined using this reproducible sequence:

```
Annual CHIRPS rasters
        ↓
Five-year mean on native CHIRPS grid
        ↓
Fill remaining native-grid NoData cells
        ↓
Reproject once to the 30 m UTM grid
        ↓
Rainfall_UTM.tif
```

Verified rainfall statistics:

```
Minimum = 2508.883 mm/year
Mean    = 2793.260 mm/year
Maximum = 3117.437 mm/year
SD      = 122.201 mm/year
```

The 30 m output is for spatial alignment only and does not imply true 30 m rainfall information.

---

## 9. OSM source counts

Fixed local OSM snapshots are used for reproducibility:

```
Road segments:     174
Waterway segments: 13
```

The final reproducibility workflow does not query live OSM data.

---

## 10. EDA result

A notable correlation reproduced by Step 08 is:

```
Rainfall–Distance-to-Stream Pearson r = -0.7201
```

This is a spatial correlation, not evidence of causation.

---

## 11. NBRO diagnostic

Seven detailed NBRO landslide sites are used as a terrain-resolution diagnostic.

```
Mean absolute SRTM–NBRO slope difference = 25.31°
```

This demonstrates that the 30 m SRTM surface smooths steep local initiation slopes.
This seven-site comparison is **not** an overall model-accuracy estimate.

---

## 12. ACCIMT event-level validation

ACCIMT Cyclone Ditwah 2025 inventory cleaning:

```
Original polygons:          4,225
Exact duplicates removed:    244
Clean polygons:             3,981
Total mapped area:       3,040.07 ha
```

Ududumbara validation:

```
Intersecting polygons: 537
Valid classified:      528
Unclassified/NoData:     9
Mapped clipped area: 427.41 ha
```

Validation by class:

| Class | Events | Event share | Density / km² | Concentration ratio |
|---|---:|---:|---:|---:|
| Low | 104 | 19.70% | 1.281 | 0.664 |
| Medium | 81 | 15.34% | 1.527 | 0.791 |
| High | 95 | 17.99% | 1.660 | 0.861 |
| Very High | 248 | 46.97% | 3.015 | 1.563 |

High + Very High:

```
343 / 528 valid events = 64.96%
High + Very High DSD area = 50.96%
High + Very High rasterised landslide area = 51.27%
```

The **64.96% value is not model accuracy**.

The validation indicates that event density increases across the susceptibility classes and that the Very High class contains a disproportionately large share of observed events relative to its area. The model is stronger at identifying where landslide events are concentrated than reproducing the complete mapped polygon extent of failures.

---

## 13. Final reproducibility QA

The completed clean run produced:

```
STEP 00 PASS
STEP 01 PASS
STEP 02 PASS
STEP 03 PASS
STEP 04 PASS
STEP 05 PASS
STEP 06 PASS
STEP 07 PASS
STEP 08 PASS
STEP 09 PASS
STEP 10 PASS
STEP 11 PASS
STEP 12 PASS
```

Step 12 confirmed:

```
Slope_UTM.tif              PASS
Rainfall_UTM.tif           PASS
DistanceToStream_UTM.tif   PASS
Curvature_UTM.tif          PASS
DistanceToRoad_UTM.tif     PASS
LS_LRSI_score.tif          PASS
LS_LRSI_class.tif          PASS
ACCIMT event counts        PASS
```

For all compared rasters:

```
Archived maximum absolute pixel difference = 0.0
```

Therefore, the clean workflow reproduces the archived final project outputs exactly.

---

## 14. Output location

New outputs are written automatically to:

```
Ududumbara/
└── LS-LRSI-2025_Final_Run/Outputs/Logs/
```

The source datasets are not overwritten.

---

## 15. Suggested assessor workflow

For the professor/examiner:

1. Confirm that `LS-LRSI-2025_Datasets` and `LS-LRSI-2025_Final_Code` are sibling folders.
2. Run `00_check_environment.py`.
3. Confirm `STEP 00 RESULT: PASS`.
4. Run Steps 01–12 in numerical order.
5. Confirm that every script reports `PASS`.
6. Inspect the final output of `12_final_qa_summary.py`.
7. Confirm that archived raster differences are `0.0`.

---

## 16. Main limitations

- LS-LRSI is a relative susceptibility-screening index, not a calibrated landslide probability.
- AHP CR evaluates pairwise-weight consistency; it is not a predictive accuracy metric.
- SRTM 30 m terrain data smooth steep local topography.
- CHIRPS rainfall is coarser than the 30 m analysis grid.
- OSM roads and waterways depend on the completeness of the fixed snapshots.
- Population, buildings, socioeconomic exposure, and vulnerability are not included as weighted factors in the current index.
- ACCIMT validation is retrospective event-level validation for Cyclone Ditwah 2025.
- Nine intersecting ACCIMT events remain unclassified because their representative points fall on NoData/boundary cells; these are retained transparently rather than forced into a class.

---

## Summary

The LS-LRSI-2025 workflow was rerun sequentially from the supplied local source datasets using scripts 00–12. Administrative boundary extraction, SRTM terrain processing, CHIRPS rainfall preparation, OSM distance surfaces, AHP-WLC modelling, exploratory analysis, NBRO terrain diagnostics, ACCIMT inventory cleaning, and independent event-level validation were regenerated. Final quality assurance confirmed that all core raster products reproduced the archived project outputs with a maximum absolute pixel difference of zero, while the ACCIMT event-validation table reproduced the established event counts and class-level validation metrics.

Use the numbered scripts in `LS-LRSI-2025_Final_Code/` — these are the **final implementation**.

Thank you.
