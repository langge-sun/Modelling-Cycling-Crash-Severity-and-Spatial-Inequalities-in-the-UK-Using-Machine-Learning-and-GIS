# Modelling Cycling Crash Severity and Spatial Inequalities in the UK Using Machine Learning and GIS

Analysis code for an MSc Data Science extended research project (DATA72000, University of
Manchester, 2026). The study links police crash records to neighbourhood deprivation and to
Census cycling counts across three English city regions — **Greater London, Greater Manchester
and the West Midlands** — and combines GIS hotspot mapping, exposure normalisation and
interpretable machine learning.

## What the notebook produces

| Cell | Produces |
|---|---|
| **A** | Data loading and linkage; exposure-normalised crash rates → **Table 4**, **Figures 4–6** |
| **B** | Deprivation co-location of crash locations, two baselines → **Table 3** |
| **C** | Severity classification and SHAP attribution → **Tables 5–7**, **Figures 7–12** |
| **D** | Categorical-encoding sensitivity check → **Appendix D** of the report |

Cells A to C must be run in order; B and C reuse objects created by A. Cell D is independent of
Chapter 5 and writes to separate filenames (`*_categorical_*.png`,
`appendix_d_categorical_encoding.csv`) so it cannot overwrite Cell C's output.

Figures 1–3 (kernel density hotspot maps) are produced in QGIS with a 500 m bandwidth, not in
this notebook. The rendered maps are in `figures/` as `*_kde_hotspot_map.png`; the full QGIS
procedure and every parameter are documented in the technical appendix submitted with the
project.

## Data

The data are **not** included in this repository. All four inputs are published openly under the
Open Government Licence and should be downloaded from source.

| Dataset | Publisher | Where to obtain |
|---|---|---|
| STATS19 cyclist casualty records, 2023–2024 | Department for Transport | data.gov.uk — DfT road safety data. One CSV of cyclist casualty points per region. |
| English Indices of Deprivation 2019, File 1 | MHCLG, 26 September 2019 | gov.uk, "English indices of deprivation 2019". Sheet `IMD2019`. |
| Census 2021 Table TS061, method used to travel to work, LSOA level | ONS, released 8 December 2022 | ons.gov.uk dataset TS061. The `Bicycle` category is used. |
| LSOA (2011) to LSOA (2021) to LAD (2022) Best Fit Lookup for EW | ONS Open Geography Portal | geoportal.statistics.gov.uk |

Filenames do not matter. The notebook locates each input by pattern, searching the notebook
folder, then `~/Downloads`, then `~/Desktop`, recursively. Cell A prints the absolute path of
every file it resolved before any analysis runs — check these are the files you intended.

## Environment

Produced with Python 3.13.5 (Anaconda). The code was not tested against other versions.

```
pandas 2.2.3          scikit-learn 1.6.1        matplotlib 3.10.0
numpy 2.1.3           imbalanced-learn 0.13.0   seaborn 0.13.2
scipy 1.15.3          lightgbm 4.6.0            shap 0.52.0
openpyxl (reads the IMD .xlsx release)
```

Mapping was carried out in QGIS 3.x.

## How to run

1. Download the four datasets above and place them anywhere under this folder, `~/Downloads`
   or `~/Desktop`.
2. Install the packages listed above.
3. Open the notebook and run Cell A. Check the resolved paths it prints.
4. Run Cell B, then Cell C.
5. Cell D is optional and reproduces Appendix D only.

Runtime is a few minutes on a laptop; the grid search in Cell C is the slowest step. All random
seeds are fixed, so on identical inputs the printed numbers reproduce the reported values exactly.

## Results

The four summary tables written by the notebook are under `results/`:

- `table_3_colocation_representation_ratios.csv` — Table 3
- `table_4_exposure_normalised_rates.csv` — Table 4
- `tables_5_to_7_model_performance.csv` — Tables 5–7
- `appendix_d_categorical_encoding.csv` — Appendix D

Figures are under `figures/`: `*_kde_hotspot_map.png` (Figures 1–3, from QGIS),
`*_exposure_normalised_rate.png` (Figures 4–6), `*_roc.png` (Figures 7–9) and
`*_shap_*.png` (Figures 10–12). Cell D also writes a `*_categorical_*.png` set; those
figures are not reproduced in the report and are not included here.

The notebook writes all of these into whichever directory it is run from, not into `figures/`
and `results/`. Those two folders hold the copies produced by the run reported in the project;
move newly generated files into them if you want to replace those copies.

## Notes on reproducibility

Two specification choices materially affect the results and are documented in the report rather
than left implicit in the code:

- The exposure denominator is built across **all** LSOAs in a region, including those recording
  no casualty. Aggregating commuter counts across casualty rows instead produces a spurious
  deprivation gradient that is consistent across regions and easily mistaken for a finding.
- The STATS19 code fields enter the reported models on their numeric scale. Cell D refits them as
  unordered categories: the feature attributions in Section 5.4 are not robust to that choice,
  while the predictive ceiling is.
